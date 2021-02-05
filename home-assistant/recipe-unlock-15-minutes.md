# Recipe for unlocking Danalock lock for 15 minutes

Unlocking + locking are activities that may need to be carried out several times a day, by different people in your home, hence you may want to provide an accessable and easy to use way of getting it done.

In this recepie, we'll going to define a trigger, preferably a button, that will unlock the lock, and later lock it when no movement has been detected for 15 minutes.

## Requirements:
- Motion sensor - [Hue Outdoor sensor](https://www.philips-hue.com/en-us/p/hue-outdoor-sensor/046677541736) used in example 
- Triggers for unlocking & locking - [Hue Dimmer Switch](https://www.philips-hue.com/en-us/p/hue-dimmer-switch/046677473372) used in example
- Configuration detailed in [configuration.yaml](configuration.yaml) in this project
- Something to indicate unlocking + locking activity - a light blinking in this example


```
# configuration.yaml

timer:
  danalock:
    duration: '00:15:00'
```

```
# automations.yaml

- alias: 'Unlock storage-room'
  trigger:
  - platform: device
    device_id: 2132132131asdsad0f
    domain: hue
    type: remote_button_long_release
    subtype: turn_on
  action:
  - service: light.turn_on
    data:
      entity_id: light.my-light
      flash: short
  - service: rest_command.storage-room_unlock
  - service: timer.start
    entity_id: timer.danalock

- alias: 'Lock storage-room'
  trigger:
  - platform: device
    device_id: 2132132131asdsad0f
    domain: hue
    type: remote_button_long_release
    subtype: turn_off
  - platform: event
    event_type: timer.finished
    event_data:
      entity_id: timer.danalock
  action:
  - service: light.turn_on
    data:
      entity_id: light.my-light
      flash: short
  - service: rest_command.storage-room_lock



# Reset timer.danalock if movement (while timer active)
- alias: 'Movement detected - reset timer.danalock'
  trigger:
  - platform: state # Or no movement for 15 minutes
    entity_id: binary_sensor.hue_outdoor_motion_sensor_1_motion
    to: 'on'
  condition:
  - condition: state
    entity_id: 'timer.danalock'
    state: 'active'
  action:
  - service: timer.start
    data:
      entity_id: timer.danalock
      duration: '00:15:00'