One note about the current label: this issue is currently labeled `enhancement`, but I don't think this is only a feature/enhancement request.

The warning/copy problem is part of the current bug mitigation for #591 / #685: the existing behavior can relaunch unrelated apps during routine display state changes, and the current warning still under-communicates the risk of repeated relaunches and possible unsaved work loss.

So I would consider this bug-related UX/safety work rather than a new feature. I do not have permission to change labels myself, but I would suggest using `bug` or another bug-related label here, possibly in addition to any implementation-tracking label you prefer.
