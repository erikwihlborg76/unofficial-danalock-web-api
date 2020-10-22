# Motivation
I wanted to schedule my lock to get locked at a specific time, but Danalock only provide APIs references to partners. So I investigated how API calls were done, and described some of them using OpenAPI specification.

API calls has been split into two different files because of different server URLs - authorization is shared though.
Both APIs are web-based (runs over HTTP) but follow different styles.

API calls described in``unofficial-danalock-web-api.yaml`` can be executed by anone with a danalock web account. REST style API. But you need a danabridge to use ``unofficial-danabridge-web-api.yaml `` RPC style API

**Note: Danalock may decide to change these APIs without future notice, resulting in breaking changes for anyone using them!**

 
# How to retrieve lock status?
Calls to both APIs are required to retrieve the lock's status

**First, retrieve bridge's serial number using calls described in  unofficial-danalock-web-api.yaml**
* Retrieve ``lock’s serial-number`` using ``GET /locks/v1``
* Use ``lock’s serial-number`` in ``GET /devices/v1/{lock-serial_number}/paired_devices`` to retrieve paired devices

**Second, ask bridge for status using calls described in unofficial-danabridge-web-api.yaml**
* Use ``device’s serial_number`` + ``operation`` (i.e. afi.lock.get-state)  in ``POST /bridge/v1/execute`` to ask the bridge to prepare a status message. This call will return an ``execution id`` to be used in next request.
* Use ``execution id`` from previous call to poll the bridge for the status message ``POST /bridge/v1/poll``
