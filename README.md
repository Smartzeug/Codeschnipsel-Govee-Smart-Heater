# Codeschnipsel-Govee-Smart-Heater
Codeschnipsel für Govee Smart Heater


Code für die configuration.yaml Datei
```
"rest_command:
  rest_govee_appliance:
    url: https://developer-api.govee.com/v1/appliance/devices/control
    method: PUT
    headers:
        Content-Type: application/json
        Govee-API-Key: EUER API KEY
    content_type:  'application/json; charset=utf-8'
    payload: '{"device": "{{ device }}","model": "{{ model }}","cmd": {"name": "{{ cmd_name }}","value": "{{ cmd_value }}"}}'"
```


