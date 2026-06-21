# Clarification Comment for #685 RU

Спасибо за ответ. Думаю, мне стоит уточнить несколько моментов, потому что часть моего сообщения могла быть прочитана шире, чем я имел в виду.

Во-первых, я согласен, что не каждое подключение/отключение/переключение дисплея обязательно вызывает relaunch wave. Мой point был не в этом. Более точная формулировка:

> When a display transition requires Thaw to apply a different menu bar spacing, Thaw can relaunch apps with menu bar items without asking the user at that moment.

То есть `each time` в моём предложении означало не “every display event unconditionally”, а “each time a display transition causes Thaw to apply a different spacing”.

Во-вторых, мой основной concern не в том, что manual Apply меняет spacing. Это я понимал. Неочевидная часть в том, что spacing can be saved/configured earlier, а затем позже автоматически применяться при display changes through app relaunches. Как пользователь, я бы скорее ожидал, что такая system-level setting применится либо сразу после явного Apply, либо после relogin/reboot/system restart. Я не ожидал, что routine display change later может стать моментом, когда Thaw применит setting и перезапустит unrelated apps.

Это особенно важно для diagnosis. Пользователь может изменить setting, увидеть warning или даже не прочитать его внимательно, продолжить работать, а через какое-то время при подключении/отключении дисплея получить relaunch wave. В этот момент он может вообще не понять, что причиной был Thaw и более ранняя настройка spacing. Именно это я имею в виду под shadow/delayed behavior.

Про non-active display warning: я не обязательно прошу modal/gating при каждом edit non-active display. Если будет JIT warning в момент реального риска, когда Thaw уже знает, что display transition сейчас потребует relaunch, то отдельные blocking warnings при каждом non-active edit действительно могут быть избыточными.

Но если JIT warning нет, тогда delayed-risk warning становится намного важнее, потому что иначе пользователь сохраняет configuration now, а destructive effect может проявиться later without any prompt. Для меня лучший вариант:

1. persistent warning clearly explains that this can be applied later during display transitions;
2. JIT warning appears when a display transition would actually require relaunching apps;
3. users who understand the risk can bypass/disable that JIT warning.

That would address my main concern much better than relying only on a persistent warning in Settings.
