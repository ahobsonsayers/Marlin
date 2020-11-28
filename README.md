<!-- Configuration.h
#define STRING_CONFIG_H_AUTHOR "(arranhs, Ender-3)" // Who made the changes. -->

# Marlin Setup

This is for a SKR Mini E3 v1.2 \
<https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/>

## Initial Steps

Need to do basic setup Marlin setup so the SKR Mini E3 works.

- Go to Config > examples > BigTreeTech > SKR Mini E3 1.2
- Copy all files to root of Marlin folder and overwrite Existing Files

- In platformio.ini, change:

  - `default_envs` &rarr; to on of the `STM32F103RC_bigtree` varients e.g. `STM32F103RC_bigtree_512k`
  - If want to use NeoPixel
    - Under `[common]`
      - Comment out:
        - `Adafruit NeoPixel@1.2.5`
      - Add:
        - `https://github.com/bigtreetech/Adafruit_NeoPixel`
    - Under `[env:STM32F103RC_bigtree]` or the selected variant
      - Delete `Adafruit NeoPixel` from `lib_ignore`

- In Configuration.h &rarr; (These should all be set by the copied config files)

  - Ensure:
    - `#define SERIAL_PORT` &rarr; `2`
    - `#define SERIAL_PORT_2` &rarr; `-1`
    - `#define BAUDRATE` &rarr; `115200`
    - `#define MOTHERBOARD` &rarr; `BOARD_BTT_SKR_MINI_E3_V1_2`
    - `#define X_DRIVER_TYPE`, `Y_DRIVER_TYPE`, `Z_DRIVER_TYPE`, `E0_DRIVER_TYPE` &rarr; `TMC2209`
    - `#define X_BED_SIZE` and `Y_BED_SIZE` &rarr; `235` (Not 220)
    - `#define DEFAULT_AXIS_STEPS_PER_UNIT` &rarr; `{ 80, 80, 400, 97.83 }`
    - `#define CR10_STOCKDISPLAY` &rarr; Enabled
    - `#define SPEAKER` &rarr; Disabled

- In Configuration_adv.h
  - Enable:
    - `#define SDCARD_CONNECTION ONBOARD` - So can access SD Card via Raspberry Pi

<!-- - Fix EEPROM Problems
  - Marlin > src > Pins > stm32 > pins_BTT_SKR_MINI_E3_DIP.h
  - Change:
    - `#define EEPROM_START_ADDRESS`: `1024` to `2048` -->

## Additional Steps

These are for added, personal extras

- In Configuration.h
  - Enable `#define PIDTEMPBED`
  - Set Bed PID values
    - Comment any Predefined PID values
    - Run `M303 E-1 C8 S90`
    - Use values output to set:
      - DEFAULT_bedKp
      - DEFAULT_bedKi
      - DEFAULT_bedKd
    - Recommended:
      ```
      #define DEFAULT_bedKp 50.71
      #define DEFAULT_bedKi 9.88
      #define DEFAULT_bedKd 173.43
      ```
  - `#define FAN_SOFT_PWM` &rarr; For possibly better fan noise

## Setup BLTouch

- In Configuration.h

  - Enable:

    - `#define BLTOUCH`
    - `#define ENDSTOPPULLUPS`
    - `#define AUTO_BED_LEVELING_BILINEAR`
    - `#define RESTORE_LEVELING_AFTER_G28`
    - `#define EXTRAPOLATE_BEYOND_GRID`
    - `#define Z_SAFE_HOMING`
    - `#define LCD_BED_LEVELING`
    - `#define MULTIPLE_PROBING` &rarr; `3`
    - `#define EXTRA_PROBING` &rarr; `2`
    - For better probing (if needed)
      - `#define PROBING_FANS_OFF`
      - `#define PROBING_STEPPERS_OFF`

  - Change:
    - `#define NOZZLE_TO_PROBE_OFFSET`
      - For Bullseye:
        - `{ -48, -10, 0 }`
      - For Hero Me 2:
        - `{ -41, -20, 0 }`
    - `#define GRID_MAX_POINTS_X` &rarr; `5`
    - `#define X_MIN_POS` &rarr; `1`
    - `#define Y_MIN_POS` &rarr; `-5`
    - `#define X_MAX_POS` &rarr; `X_BED_SIZE + 14`
    - `#define Y_MAX_POS` &rarr; `Y_BED_SIZE - 4`
    - `#define PROBING_MARGIN` &rarr; `20`

- In Configuration_adv.h

  - Enable `#define BABYSTEP_ZPROBE_OFFSET`
  - `#define BABYSTEP_MULTIPLICATOR_Z` &rarr; `5` (so baby steps are bigger)

## Trialling

- For more accurate probing

  - uncomment WAIT_FOR_BED_HEATER
  - uncomment DELAY_BEFORE_PROBING
  - uncomment BLTOUCH_DELAY

- Comment #define MIN_SOFTWARE_ENDSTOP_Z
- In Configuration_adv.h
- Uncomment #define POWER_LOSS_RECOVERY

- MIN_PROBE_EDGE = 5
- Z_AFTER_PROBING 5
- #define DEFAULT_MINSEGMENTTIME 50000
  - <http://lokspace.eu/bad-print-quality-with-usb-or-octoprint-the-solution-is-here/>
