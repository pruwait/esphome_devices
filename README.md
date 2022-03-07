# esphome_devices
Устройства на базе EspHome, которые работают у меня

Счетчик электроэнергии Меркурий 200.02

Кондиционер Hair as09tl4hra

Счетчик газа с датчиком импульсов.
Иногда esp перезагружается. При этом обнуляется счетчик.
Чтобы не заполнять его вручную, создал две автоматизации.
```
alias: Сохранения счетчика газа
description: Восстанавливает значение счетчика при перезагрузках
trigger:
  - platform: state
    entity_id: sensor.gas_volume
condition:
  - condition: numeric_state
    entity_id: sensor.gas_volume
    above: '50'
action:
  - service: counter.configure
    data:
      value: '{{ states(''sensor.gas_volume'') | float * 100 }}'
    target:
      entity_id: counter.gas_vol_memory
mode: single
```
```
alias: Восстановление счетчика газа
description: Восстанавливает значение счетчика при перезагрузках
trigger:
  - platform: state
    entity_id: sensor.gas_volume
condition:
  - condition: numeric_state
    entity_id: sensor.gas_volume
    below: '50'
action:
  - service: esphome.gas_bk_g4t_set_gas
    data:
      curr_count: '{{ states(''counter.gas_vol_memory'') | int + 1 }}'
mode: single
```
