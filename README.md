# Codeschnipsel-Govee-Smart-Heater
Codeschnipsel für Govee Smart Heater


Code für die configuration.yaml Datei

```
rest_command:
  rest_govee_appliance:
    url: https://developer-api.govee.com/v1/appliance/devices/control
    method: PUT
    headers:
        Content-Type: application/json
        Govee-API-Key: EUER API KEY
    content_type:  'application/json; charset=utf-8'
    payload: '{"device": "{{ device }}","model": "{{ model }}","cmd": {"name": "{{ cmd_name }}","value": "{{ cmd_value }}"}}'
```


```
fan:
  platform: template
  fans:
    govee_smart_heater:
      friendly_name: "Govee Smart Heater"
      value_template: "{{ states('input_boolean.EUER_SCHALTER') }}"
      preset_mode_template: "{{ states('input_select.EUER_DROPDOWN') }}"
      turn_on:
        service: script.EUER_SKRIPT_AN
      turn_off:
        service: script.EUER_SKRIPT_AUS
      set_preset_mode:
        service: script.EUER_SKRIPT_MODUS
        data:
          govee_smart_heater_modus: >
            {% set mapper = {'Low': 1, 'Medium': 2, 'High': 3} %}
            {{ mapper[preset_mode] }}
      speed_count: 3
      preset_modes:
        - 'Low'
        - 'Medium'
        - 'High'
```


Code für die Skripte
```
alias: Govee Smart Heater An
sequence:
  - service: rest_command.rest_govee_appliance
    data:
      device: EUER_DEVICE_ID
      model: EUER_MODELL
      cmd_name: turn
      cmd_value: "on"
    enabled: true
  - delay:
      hours: 0
      minutes: 0
      seconds: 0
      milliseconds: 500
  - service: input_boolean.turn_on
    target:
      entity_id: input_boolean.EUER_SCHALTER
    data: {}
mode: single
```
```
alias: Govee Smart Heater Aus
sequence:
  - service: rest_command.rest_govee_appliance
    data:
      device: EUER_DEVICE_ID
      model: EUER_MODELL
      cmd_name: turn
      cmd_value: "off"
    enabled: true
  - delay:
      hours: 0
      minutes: 0
      seconds: 0
      milliseconds: 500
  - service: input_boolean.turn_off
    target:
      entity_id: input_boolean.EUER_SCHALTER
    data: {}
  - service: input_select.select_option
    entity_id: input_select.EUER_DROPDOWN
    data:
      option: "Off"
mode: single
```
```
alias: Govee Smart Heater Modus
sequence:
  - service: input_select.select_option
    entity_id: input_select.EUER_DROPDOWN
    data:
      option: >
        {% set mapper = {1: "Low", 2: "Medium", 3: "High"} %} 
        {{mapper[govee_smart_heater_modus] }}
    enabled: true
  - delay:
      hours: 0
      minutes: 0
      seconds: 0
      milliseconds: 500
  - service: rest_command.rest_govee_appliance
    data:
      device: EUER_DEVICE_ID
      model: EUER_MODELL
      cmd_name: mode
      cmd_value: "{{ govee_smart_heater_modus }}"
  - service: input_boolean.turn_on
    target:
      entity_id: input_boolean.EUER_SCHALTER
    data: {}
mode: single
icon: mdi:radiator
```





