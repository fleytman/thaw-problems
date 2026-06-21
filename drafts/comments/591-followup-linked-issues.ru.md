# Follow-up comment for #591 RU

Спасибо за уточнение. Я вынес follow-up work в отдельные issues, как вы и попросили:

1. https://github.com/stonerl/Thaw/issues/685 - behavior-level issue про display connect/disconnect/switch, который может повторно relaunch menu bar apps без just-in-time confirmation, когда spacing differs.
2. https://github.com/stonerl/Thaw/issues/686 - более узкое issue про warning/copy: сделать риск понятнее, включая `each time`, `each app with a menu bar item` и возможную потерю unsaved input/progress/state.

Так runtime behavior concern отделён от warning text / visual severity concern.

При этом я всё ещё считаю, что закрывать #591 как resolved рискованно. Основная проблема не только в том, что поведение было недостаточно задокументировано. Проблема в том, что routine display state changes могут снова неожиданно перезапустить unrelated apps и привести к потере пользовательской работы. Я ожидал, что manual Apply изменит spacing в момент нажатия или, возможно, после перезагрузки системы, но не ожидал, что последующие display connect/disconnect/switch events будут заново применять spacing и relaunch apps без запроса в этот момент. Warning улучшает discoverability, но не убирает unsafe path и не делает default behavior безопасным.

Отдельно это затрудняет diagnosis: если настройка была сохранена заранее, а destructive effect проявился позже при display change, пользователь может даже не понять, что relaunch wave вызвал именно Thaw.

Закрытый статус исходного issue также усложняет поиск проблемы для других пользователей, которые столкнутся с такой же relaunch wave. Workaround и объяснение фактически оказываются buried in a closed issue, хотя practical harm всё ещё возможен.

Поэтому я понимаю просьбу вынести follow-up work в отдельные issues, и сделал это. Но я всё равно предложил бы reopen #591 или иначе считать его unresolved до practical mitigation, а новые issues залинковать как конкретные implementation tasks.
