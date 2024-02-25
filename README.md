# Unofficial Danalock web API

This repo contains instructions for integrating the Danalock V3 Smart lock using web API requests.

Key use cases:

- ✅ Lock
- ✅ Unlock
- ✅ Lock status, i.e. determine if the lock is open or locked

Table of contents:

- A Node-RED flow that simplifies interacting with Danalock locks.
- OpenAPI docs that describe how to interact with the Danalock locks.
- Home Assistant configuration samples.

Requirements:

- [Danalock V3 Smart Lock](https://danalock.com/products/danalock-v3-smart-lock) - The main unit unit.
- [Danabridge](https://danalock.com/products/danabridge-v3) - To enable API access to the lock over HTTP.
- [Danalock account](https://api.danalock.com/account/create) - For authorization and configuration using the Danalock mobile app and/or Danalock web UI.

## Implementation option 1 - Simplified integration using a Node-RED flow

This option involves running a Node-RED flow that abstracts away the complexity of dealing with the Danalock web APIs.

>**IMPORTANT NOTE:** The API operations exposed by the Node-RED flow are NOT secure:
>
> - Anyone with local network access can access the API operations exposed by the Node-RED flow (and indirectly the Danalock).
> - Data is not encrypted during transport. Instructions for securing data in transit can be found here [How To Secure Nginx with Let's Encrypt on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-22-04).

### Step 1 - Set a lock name that meets the requirements

Set the lock name in the [Danalock web UI](https://my.danalock.com/#/login).

Lock naming rules:

- Use alphanumeric characters: `0-9`, `a-z`, and `A-Z`. Note that the name is also case-sensitive.
- Underline (`_`) is also safe to use.
- Do NOT use white spaces (" ") or hyphens ("-").

A good example is `garden_storage`.

### Step 2- Install Node-RED

See [Node-RED installation options](https://nodered.org/docs/getting-started/local).

In all subsequent examples, Node-RED is assumed to be installed in in a host named `nodered`, and the web UI and services are exposed from [http://nodered:1880](http://nodered:1880).


### Step 3 - Create a file that contains your Danalock credentials

The Node-RED flow will look for Danalock credentials in a file named `danalock.cfg` located in the working directory of the Node-RED process.

Examples:

|  Running Node-RED | Path to `danalock.cfg`   |
|---|---|
| As a user named `nodered` | `/home/nodered/danalock.cfg` |
| As the root user in a LXC container (e.g. using Proxmox)  | `/root/danalock.cfg`|
| As an addon in Home-Assistant.  | [Set up a Node-RED context store](https://nodered.org/docs/user-guide/context#saving-context-data-to-the-file-system)|

Create a file in the working directory of your Node-RED installation named `danalock.cfg` that follows the structure below, and add your Danalock account credentials:

```JSON
{
    "username": "username@somemail.com",
    "password": "yourpassword"
}
```

### Step 4- Import and deploy the Node-RED flow

Import `danalock.json` to Node-RED. The flow logs events using Node-RED's standard logging capability. In some Node-RED installations the log can be viewed using the command `node-red-log`.


### Step 5 - How to interact with the lock

In the below examples, the name of the lock is "storage_room".

|  Operation| API request   |
|---|---|
| List locks        |`GET http://nodered:1880/danalock/locks`|
| Get state         |`GET http://nodered:1880/danalock/storage_room/get-state`|
| Lock the lock     |`GET http://nodered:1880/danalock/storage_room/lock`|
| Unlock the lock   |`GET http://nodered:1880/danalock/storage_room/unlock`|
| Get battery level |`GET http://nodered:1880/danalock/storage_room/battery-level`|

Note: All operations above (except "List locks") result in a call being made to the lock. Such calls often take about 5-7 seconds to complete.

## Implementation option 2 - Build from scratch

Another option is to build your Danalock API client using API calls directly to the Danalock APIs. See OpenAPI docs `unofficial-danalock-web-api.yaml` and `unofficial-danabridge-web-api.yaml` for more info.

### Example: How to retrieve the lock's status

In this example, the server URLs are excluded.

First, retrieve the bridge's serial number. Requests are described in `unofficial-danalock-web-api.yaml`.

- Retrieve the lock’s serial-number using `GET /locks/v1`
- Use the lock’s serial-number in `GET /devices/v1/{lock-serial_number}/paired_devices` to retrieve paired devices.


Second, ask the Danalock bridge for status. Requests are described in `unofficial-danabridge-web-api.yaml`.

- Use the `lock serial_number` + `operation` (i.e. "afi.lock.get-state") in `POST /bridge/v1/execute` to ask the bridge to prepare a status message. This call will return an `job id` to be used in next request.
- Wait about 5 - 7 seconds for the bridge to retrieve status from lock
- Use ``job id`` from previous call to poll the bridge for the status message ``POST /bridge/v1/poll``


## Integrate Danalock with Home Assistant using standard components

Danalock can be integrated into [Home Assistant](https://www.home-assistant.io/) using standard platform components. Instructions are provided in `home-assistant\recipe-unlock-15-minutes.md`.

Integrating Danalock to Home Assistant is discussed in the [Unoffical danalock web API](https://community.home-assistant.io/t/unoffical-danalock-web-api/238097) forum post.