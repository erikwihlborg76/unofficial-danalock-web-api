# Node-RED Danalock web API wrapper

A Node-RED flow that exposes **unprotected endpoints** - hence you need to ensure these endpoints are only accessable from within your local network.

The flow is designed based on the assumption that you only have a single danalock device.

Before deployment, you must set your Danalock user and passord in node "Set Danalock account credentials" (at the top).

Supported GET endpoints:
- http://node-red-host:1880/log/v1/lock
- http://node-red-host:1880/locks/v1
- http://node-red-host:1880/bridge/v1/execute/status
- http://node-red-host:1880/bridge/v1/execute/lock
- http://node-red-host:1880/bridge/v1/execute/unlock