# Align eeprom defaults

## HAKRC_K_F421
### M450
 * Input type: DSHOT (1)
 * Motor KV: 420 (10, value*40+20)
 * Timing advance: 15 deg (26, value-10*0.9375)
 * Running brake level: 3 (1 to 10)
 * Ramp rate: 2 %/ms (20)
 * Minimum duty cycle: 5 %

### M460
 * Input type: DSHOT (1)
 * Motor KV: 380 (9, value*40+20)
 * Timing advance: 10 deg (21, value-10*0.9375)
 * Startup power: 150 %
 * Running brake level: 3 (1 to 10)
 * Ramp rate: 4 %/ms (40)

### M3
 * Input type: DSHOT (1)
 * Motor KV: 660 (16, value*40+20)
 * Timing advance: 15 deg (26, value-10*0.9375)
 * Sinusoidal startup: enabled (1)

### MR25
 * Input type: DSHOT (1)
 * Motor KV: 2380 (59, value*40+20)
 * Startup power: 150 %
 * Minimum duty cycle: 2 %

## Align (WIP)

### Pinout
  STM32L431 UFQFPN32 Complete Pinout (ALIGN_40A_L431_CAN)
  Hardware: HARDWARE_GROUP_L4_B + HSE on PA0/CK_IN

  Pin#  | Signal    | Function              | Notes
  ------|-----------|-----------------------|---------------------------
  6     | PA0       | **HSE CK_IN**         | 24 MHz external oscillator input
  7     | PA1       | ADC Temperature       | ADC_CHANNEL_6 (internal temp sensor)
  8     | PA2       | RC input              | DSHOT/PWM input (TIM15_CH1)
  9     | PA3       | Current ADC           | ADC_CHANNEL_8
  10    | PA4       | COMP2- (Phase C)      | Comparator input
  11    | PA5       | COMP2- (Phase B)      | Comparator input
  12    | PA6       | Voltage ADC           | ADC_CHANNEL_11
  13    | PA7       | Phase C Low FET       | Motor output
  14    | PB0       | Phase B Low FET       | Motor output
  15    | PB1       | Phase A Low FET       | Motor output
  16    | VSS       | Ground                | Power
  17    | VDD       | 3.3V                  | Power
  18    | PA8       | Phase C High FET      | Motor output
  19    | PA9       | Phase B High FET      | Motor output
  20    | PA10      | Phase A High FET      | Motor output
  21    | PA11      | CAN_RX                | DroneCAN
  22    | PA12      | CAN_TX                | DroneCAN
  23    | PA13      | SWDIO                 | Debug
  24    | PA14      | SWCLK                 | Debug
  25    | PA15      | **RGB LED RED** 🔴    | GPIO Output
  26    | PB3       | **RGB BLUE** 🔵       | GPIO Output
  27    | PB4       | COMP2+ Common Ref     | Comparator common reference
  28    | PB5       | **RGB LED GREEN** 🟢  | GPIO Output
  29    | PB6       | USART1_TX             | Serial Telemetry
  30    | PB7       | COMP2- (Phase A)      | Comparator input
  31    | VSS       | Ground                | Power
  32    | VDD       | 3.3V                  | Power

### Hardware Configuration
  - **MCU**: STM32L431KBU6 (UFQFPN32 package)
  - **Clock Source**: 24 MHz external oscillator on PA0/CK_IN (HSE bypass mode)
  - **Motor Control**: HARDWARE_GROUP_L4_B (COMP2, phases on PB7/PA5/PA4)
  - **RGB LED**: Common anode on PB3 (Red), PB5 (Green), PA15 (Blue)
  - **Communication**: DroneCAN (CAN on PA11/PA12), Serial Telemetry (PB6)

### External Oscillator Requirements
  For reliable DroneCAN operation at high temperatures (up to 120°C):
  - **Type**: TCXO, MEMS, or standard XO rated -40°C to +125°C
  - **Frequency**: 24.000 MHz
  - **Accuracy**: ±50 ppm or better over temperature range
  - **Recommended**: SiTime SiT8008BI or Abracon ASE-24.000MHZ-LC-T
  - **Connection**: Oscillator output → PA0 (pin 6), GND to VSS

### Old defines
#ifdef ALIGN_M450_F421
#define FIRMWARE_NAME "Align M450  "
#define FILE_NAME "ALIGN_M450_F421"
#define DEAD_TIME 20
#define HARDWARE_GROUP_AT_045
#define HARDWARE_GROUP_AT_C
#define USE_SERIAL_TELEMETRY
#define TARGET_VOLTAGE_DIVIDER 110
#define MILLIVOLT_PER_AMP 75
#define ADC_CHANNEL_VOLTAGE ADC_CHANNEL_3
#define ADC_CHANNEL_CURRENT ADC_CHANNEL_6
#define ALIGN_DISABLE_STARTUP_TUNE // disable startup tune
/* defaults EEPROM values
 * INPUT_TYPE 1 // DSHOT
 * MOTOR_KV 9 // *40+20 = 380 kv
 * TIMING_ADVANCE 10 // [10 42] -10*0.9375 deg
 * RUNNING_BRAKE 3 // 1 to 10
 * MAX_RAMP 40 // old RAMP_SPEED_HIGH_RPM 4
 */
#endif



