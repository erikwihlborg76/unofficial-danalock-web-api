# Quickest way to get started?

## Expose Danalock functionality through a Node-RED flow 
The easiest and quickest way to use the Danalock web API, is by using a Node-RED Danalock web API wrapper (see folder  ``node-red``)

## Integrate Danalock w/ home-assistant using standard components
Endpoints exposed from the Node-RED implementation mentioned above can be integrated to home-assistant using platform standard components. See folder ``home-assistant`` to set it up.

 
# How to retrieve lock status?

Calls to two different Danalock APIs, ``unofficial-danabridge-web-api.yaml`` & ``unofficial-danalock-web-api.yaml``, are required to retrieve the lock's status

**First, retrieve bridge's serial number using calls described in  unofficial-danalock-web-api.yaml**
* Retrieve ``lock’s serial-number`` using ``GET /locks/v1``
* Use ``lock’s serial-number`` in ``GET /devices/v1/{lock-serial_number}/paired_devices`` to retrieve paired devices

**Second, ask bridge for status using calls described in unofficial-danabridge-web-api.yaml**
* Use ``device’s serial_number`` + ``operation`` (i.e. afi.lock.get-state)  in ``POST /bridge/v1/execute`` to ask the bridge to prepare a status message. This call will return an ``job id`` to be used in next request.
* Wait about **8 seconds** for the bridge to retrieve status from lock
* Use ``job id`` from previous call to poll the bridge for the status message ``POST /bridge/v1/poll``


# Motivation
I wanted to schedule my lock to get locked at a specific time, but Danalock only provide APIs references to partners. So I investigated how API calls were done, and described some of them using OpenAPI specification.

API calls has been split into two different files because of different server URLs - authorization is same in both though.
Both APIs are web-based (runs over HTTP) but follow different styles.

API calls described in``unofficial-danalock-web-api.yaml`` can be executed by anone with a danalock web account. REST style API. But you need a danabridge to use ``unofficial-danabridge-web-api.yaml `` RPC style API

**Note: Danalock may decide to change these APIs without future notice, resulting in breaking changes for anyone using them!**