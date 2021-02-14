# Node-RED Danalock web API wrapper

>A Node-RED flow that exposes **unprotected endpoints** - i.e. URLs that anyone with access to your network can access

The flow is designed based on the assumption that you only have a single danalock device, and requires [node-red-contrib-oauth2](https://flows.nodered.org/node/node-red-contrib-oauth2)

Before deployment, you must set your Danalock user and passord in node "Set Danalock account credentials" (at the top).

Exposed GET endpoints:
- http://node-red-host:1880/danalock/log/v1/lock
- http://node-red-host:1880/danalock/locks/v1
- http://node-red-host:1880/danalock/bridge/v1/execute/status
- http://node-red-host:1880/danalock/bridge/v1/execute/lock
- http://node-red-host:1880/danalock/bridge/v1/execute/unlock