# Follow-up Comment Draft RU

Created: 07.06.2026 15:03:56 +05
Updated: 08.06.2026 14:25:08 +05

> Русская версия для проверки смысла. Если согласуем, потом можно сделать английскую версию для GitHub.

Спасибо за ссылку на изменение и за то, что вы продолжаете поддерживать проект после Ice. Я это ценю: именно поэтому я и пробую перейти на Thaw как на более поддерживаемую и, надеюсь, более стабильную версию.

Я посмотрел commit `e6227bbe` / PR `#670` и текущий код на `development`. Новый persistent warning в Displays settings действительно улучшает discoverability:

> Connecting a display whose spacing differs from the current spacing relaunches every menu bar app. Keep spacing consistent across displays to avoid relaunches.

Но мне кажется, что это всё ещё не закрывает основную проблему. Это предупреждение документирует опасное поведение, но не устраняет dangerous path и не делает default безопасным.

Я бы предложил как минимум сделать текст ещё более явным. Например:

> Connecting or disconnecting a display whose spacing differs from the current spacing relaunches each app with a menu bar item. Keep spacing consistent across displays to avoid relaunches. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

Для меня здесь важны три уточнения:

1. `connecting or disconnecting`, потому что для пользователя это обычное изменение display state, а не осознанное применение опасной настройки;
2. `each app with a menu bar item`, потому что это понятнее, чем абстрактное “menu bar app”;
3. явное предупреждение про возможную потерю unsaved input/progress/state. В моём случае это не теоретический риск: я действительно потерял введённый текст в некоторых приложениях после неожиданной relaunch wave.

После проверки кода я также вижу несколько причин, почему warning-only mitigation недостаточен.

Во-первых, per-display spacing технически не является локальной per-display настройкой в привычном смысле. В коде `DisplayIceBarConfiguration.itemSpacingOffset` прямо описан как значение, которое нужно записывать в single system-wide `NSStatusItemSpacing`, поэтому Thaw записывает системное значение заново и перезапускает приложения, когда этот дисплей становится active menu bar display. Это значит, что UI выглядит как обычный cosmetic per-display slider, но фактически управляет global system setting с destructive side effect.

Во-вторых, опасность существует не только при ручном Apply. При `screenParametersChanged` Thaw обновляет known displays и, если active menu bar display изменился, вызывает apply spacing для активного дисплея. Если on-disk spacing не совпадает с desired spacing для этого дисплея, `applyOffset()` пишет defaults и relaunches the affected apps. Насколько я вижу, в этом runtime path нет подтверждения пользователя. Поэтому пользователь может столкнуться с relaunch wave именно во время обычного подключения/отключения/переключения дисплея, когда он занят в другом приложении.

В-третьих, часть UI всё ещё не делает риск достаточно очевидным. Для active display Apply показывает alert, но текущий текст говорит “will briefly relaunch all apps with menu bar items” и не говорит о потере данных/state. Для non-active display с active profile сообщение говорит только “Save the new spacing…”, без явного объяснения, что later switching to that display may relaunch apps. Для non-active display без active profile код, насколько я понимаю, вообще коммитит spacing сразу без alert. Это не обязательно запускает relaunch immediately, но может создать будущий mismatch, который проявится позже при смене дисплея.

В-четвёртых, global spacing slider меняет saved global template сразу при движении slider. Комментарий в коде объясняет, что relaunch wave запускается не на самом slider movement, а когда template применяется к per-display configurations. Но этот global template также используется как seed для newly connected displays. Поэтому даже если immediate relaunch нет, пользователь может изменить template и позже получить неожиданный relaunch при display change, если итоговый desired spacing расходится с текущим system spacing.

Поэтому, на мой взгляд, более правильная модель была бы такой:

1. unified spacing across displays должен быть safe default;
2. per-display spacing должен быть advanced/dangerous opt-in, например через отдельный checkbox вроде “Enable advanced per-display spacing”;
3. warning должен быть styled as danger/red, потому что речь может идти о потере unsaved work, а не просто о cosmetic inconvenience;
4. если Thaw видит, что display connect/disconnect/switch сейчас потребует relaunch wave, лучше спросить пользователя в моменте: relaunch now, postpone, keep current spacing for now, or disable per-display spacing/use unified spacing;
5. все alert/callout тексты должны явно говорить не только “briefly relaunch”, но и “may cause unsaved input/progress/state to be lost”.

Отдельно про bug vs feature request. Я понимаю, что конкретный вариант “respect global spacing only” можно оформить как feature request. Но сама проблема, на мой взгляд, не только feature request. UX bug здесь в том, что обычная на вид настройка и обычное подключение/отключение дисплея могут привести к неожиданному перезапуску unrelated apps и потере пользовательской работы.

Поэтому закрывать этот bug как resolved после warning-only mitigation кажется преждевременным. Warning улучшает disclosure, но не устраняет unsafe default/path. Более корректно было бы оставить bug/UX issue открытым до practical mitigation, а конкретные решения вроде “respect global spacing only”, “runtime confirmation/postpone” или “advanced per-display spacing gate” трекать как linked implementation tasks.
