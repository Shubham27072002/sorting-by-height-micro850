# I/O table

The Factory I/O driver for the Micro800 (by Ingear) builds its tag names from a
fixed prefix plus a number (`BOOL_IN_0`, `BOOL_OUT_0`, `INT_OUT_0`), so the raw
names cannot be changed. The meaning of each point is written in the **Alias**
column in CCW instead, and in the table below.

The Factory I/O side of the mapping is in
[`media/images/ccw/factory-io-driver-config.png`](../media/images/ccw/factory-io-driver-config.png).

## Inputs (Factory I/O → PLC)

| Tag | CCW alias | Factory I/O sensor | Type | Used for |
|---|---|---|---|---|
| `BOOL_IN_0`  | at_entry            | At entry            | Sensor | Counter up (+1), feeder logic (R25, R34) |
| `BOOL_IN_1`  | low_box             | Low box             | Sensor | Low beam: height check, box-passed pulse (R06, R07) |
| `BOOL_IN_2`  | high_box            | High box            | Sensor | High beam: tall-box detection (R06) |
| `BOOL_IN_3`  | at_turntable_entry  | At turntable entry  | Sensor | Entry conveyor and turntable loading (R18, R24) |
| `BOOL_IN_4`  | at_load_position    | At load position    | Sensor | Box on the turntable load position (R18, R20, R22) |
| `BOOL_IN_5`  | at_unload_position  | At unload position  | Sensor | Box ready to leave the turntable (R20, R21, R23, R26, R27) |
| `BOOL_IN_6`  | at_front            | At front            | Sensor | Box at the front of the turntable (R13, R19, R22) |
| `BOOL_IN_7`  | at_right_entry      | At right entry      | Sensor | Box entering the right exit conveyor (R23, R31) |
| `BOOL_IN_8`  | at_left_entry       | At left entry       | Sensor | Box entering the left exit conveyor (R23, R30) |
| `BOOL_IN_9`  | at_right_exit       | At right exit       | Sensor | Box leaving the right exit conveyor (R29) |
| `BOOL_IN_10` | at_left_exit        | At left exit        | Sensor | Box leaving the left exit conveyor (R28) |
| `BOOL_IN_11` | start               | Start               | Push button (NO) | Start command (R05) |
| `BOOL_IN_12` | reset               | Reset               | Push button (NO) | E-stop reset (R02) |
| `BOOL_IN_13` | stop                | Stop                | Push button (NC, TRUE = not pressed) | Run permissive (R04) |
| `BOOL_IN_14` | emergency_stop      | Emergency stop      | E-stop (NC, TRUE = released) | E-stop latch and run permissive (R02, R03, R04) |
| `BOOL_IN_15` | auto                | Auto                | Selector switch | Run permissive: Auto must be selected (R04) |
| `BOOL_IN_16` | FACTORY_I/O (Running) | FACTORY I/O (Running) | Factory I/O status | Run permissive and counter reset (R04, R33) |

## Outputs (PLC → Factory I/O)

| Tag | CCW alias | Factory I/O actuator | Driven by |
|---|---|---|---|
| `BOOL_OUT_0`  | feeder_conveyor | Feeder conveyor | `Sts_Running AND Cmd_Feeder` (R35) |
| `BOOL_OUT_1`  | entry_conveyor  | Entry conveyor  | `Sts_Running AND Cmd_Entry_Conv` (R36) |
| `BOOL_OUT_2`  | load            | Turntable load rollers | `Sts_Running AND Cmd_Turntable_Load` (R37) |
| `BOOL_OUT_3`  | unload          | Turntable unload rollers | `Sts_Running AND Cmd_Turntable_Unload` (R38) |
| `BOOL_OUT_4`  | turn            | Turntable turn  | `Sts_Running AND Cmd_Turn` (R39) |
| `BOOL_OUT_5`  | left_conveyor   | Left exit conveyor | `Sts_Running AND Cmd_Left_Conv` (R40) |
| `BOOL_OUT_6`  | right_conveyor  | Right exit conveyor | `Sts_Running AND Cmd_Right_Conv` (R41) |
| `BOOL_OUT_7`  | green_indicator | Stack light, green  | `Sts_Running` (R42) |
| `BOOL_OUT_8`  | yellow_indicator| Stack light, yellow | `NOT Sts_Running AND NOT Sts_EStop_Latched` (R43) |
| `BOOL_OUT_9`  | red_indicator   | Stack light, red    | `Sts_EStop_Latched` (R44) |
| `BOOL_OUT_10` | start_light     | Start button light  | `NOT Sts_Running AND NOT Sts_EStop_Latched` (R43) |
| `BOOL_OUT_11` | reset_light     | Reset button light  | `Sts_EStop_Latched` (R44) |
| `BOOL_OUT_12` | stop_light      | Stop button light   | `Sts_Running` (R42) |
| `INT_OUT_0`   | counter         | Counter display (INT) | `ANY_TO_INT(Cnt_Boxes_In_Process)` (R45) |

## Summary

| | Count |
|---|---|
| Digital inputs used | 17 (`BOOL_IN_0` to `BOOL_IN_16`) |
| Digital outputs used | 13 (`BOOL_OUT_0` to `BOOL_OUT_12`) |
| Analog / integer outputs | 1 (`INT_OUT_0`) |

## Notes

* Stop and E-stop are wired normally closed, so `TRUE` means healthy. A broken
  wire therefore stops the cell, the same as pressing the button.
* The `Factory I/O (Running)` input goes `FALSE` when the simulation is paused or
  reset, which stops the cell and zeroes the counter.
* Factory I/O reports the box-detecting sensors as `TRUE` while a box blocks the beam.
