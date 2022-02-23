# Node-RED Danalock web API wrapper

>A Node-RED flow that exposes **unprotected endpoints** - i.e. URLs that anyone with access to your network can access

The flow now supports multiple locks. A requirement is to set the lock names (configured in the danalock web UI) to names that does not that does not contain white spaces (" ") or hyphens ("-"). Note that the name is also case sensitive. A good example is "storage_room"

Before deployment, you must set your Danalock user and passord in node "Set Danalock account credentials" (at the top).

Exposed GET endpoints (example based on a lock called "storage_room, and Node-RED runs on the host http://node-red:1880"):
- http://node-red:1880/danalock/bridge/storage_room/execute/status
- http://node-red:1880/danalock/bridge/storage_room/execute/lock
- http://node-red:1880/danalock/bridge/storage_room/execute/unlock
- http://node-red:1880/danalock/bridge/storage_room/execute/battery-level
- http://node-red:1880/danalock/api/storage_room/log
- http://node-red:1880/danalock/api/storage_room/locks
