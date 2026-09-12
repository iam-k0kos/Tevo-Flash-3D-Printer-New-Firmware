# Tevo Flash — Marlin 1.1.9.1 (плата MKS Gen L V1.0 + TMC2208 standalone)
База: официальный Marlin 1.1.9.1.
Поверх базы перенесены настройки из прошивки Tevo Flash (Homers3D/Tevo-Flash), адаптированные под фактическую плату MKS Gen L V1.0.

## Что изменено в Configuration.h
- MOTHERBOARD → BOARD_MKS_GEN_L (было BOARD_MKS_BASE в оригинале Tevo)
- TEMP_SENSOR_BED = 1, TEMP_RESIDENCY_TIME = 5
- PID хотэнда: Kp=12.56 Ki=0.91 Kd=43.20
- Включен PIDTEMPBED, PID стола: Kp=138.56 Ki=24.51 Kd=195.80
- Включены PID_AUTOTUNE_MENU, PROBING_HEATERS_OFF, PROBING_FANS_OFF
- X/Y/Z_MIN_ENDSTOP_INVERTING = true (NC-логика концевиков, как на Tevo)
- DEFAULT_AXIS_STEPS_PER_UNIT = { 80.058, 80.058, 399.2901, 408 }
- DEFAULT_MAX_FEEDRATE = { 400, 400, 50, 45 }
- DEFAULT_MAX_ACCELERATION = { 3000, 3000, 300, 10000 }
- DEFAULT_ACCELERATION = 1500, DEFAULT_RETRACT_ACCELERATION = 10000
- X_BED_SIZE / Y_BED_SIZE = 240, X_MIN_POS = -12, Z_MAX_POS = 260
- HOMING_FEEDRATE_XY = 100*60, HOMING_FEEDRATE_Z = 25*60
- PREHEAT_1_TEMP_HOTEND = 200
- NOZZLE_PARK_POINT = { X_MIN_POS, Y_MIN_POS, 10 }
- DEFAULT_NOMINAL_FILAMENT_DIA = 1.75
- Включены EEPROM_SETTINGS, SDSUPPORT, ENCODER_PULSES_PER_STEP=4,
  ENCODER_STEPS_PER_MENU_ITEM=1, INDIVIDUAL_AXIS_HOMING_MENU, MINIPANEL

## Что изменено в Configuration_adv.h
- E0_AUTO_FAN_PIN = 7
- Z_HOME_BUMP_MM = 1, HOMING_BUMP_DIVISOR = {15, 15, 10}
- Включен BABYSTEPPING, BABYSTEP_MULTIPLICATOR = 10
- DOUBLECLICK_MAX_INTERVAL = 2500
- Включены ADVANCED_PAUSE_FEATURE, PARK_HEAD_ON_PAUSE, HOME_BEFORE_FILAMENT_CHANGE
- PAUSE_PARK_RETRACT_FEEDRATE=5, PAUSE_PARK_RETRACT_LENGTH=10
- FILAMENT_CHANGE_UNLOAD_FEEDRATE=50, FILAMENT_CHANGE_UNLOAD_LENGTH=400
- FAN_KICKSTART_TIME = 100
- Включено CUSTOM_USER_MENUS с пунктами быстрого доступа как в стоке Tevo:
  Home Me First / Front Left / Front Right / Rear Right / Rear Left

Для сборки в Arduino IDE понадобится библиотека **U8glib** (нужна для LCD
MINIPANEL) — поставьте её через Library Manager: Sketch → Include Library →
Manage Libraries → найти "U8glib" → Install. Без неё будет ошибка
"U8glib.h: No such file or directory".

## Перед прошивкой
1. Обязательно сделайте резервную копию текущих EEPROM-настроек (M503) —
   после смены версии Marlin рекомендуется сделать Factory Reset (M502 + M500)
   после первой прошивки, чтобы не тянуть несовместимые старые значения из EEPROM.
2. Откройте проект в Arduino IDE (или PlatformIO), выберите плату
   "Arduino Mega 2560" (плата MKS Gen L основана на ATmega2560),
   скомпилируйте и залейте через USB.
3. После прошивки: G28 (хоуминг), затем проверьте PID (M303) и при
   необходимости откалибруйте заново — тепловые PID сильно зависят от
   конкретного нагревателя/термистора и окружения.

* тесты и копию можно сделать через Printrun под Windows.

## Мой принтер
Стоковый Tevo Flash по железу, по X и Y установлены рельсы с каретками, драйверы TMC2208, мотор Z оси один, экструдер один.
