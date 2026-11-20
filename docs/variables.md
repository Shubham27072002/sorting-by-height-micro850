# Variables

All variables below are local to the `Sort_By_Height` program, except the raw I/O
tags, which are global (see [io-table.md](io-table.md)).
Screenshots: [local variables](../media/images/ccw/variables-local.png),
[global I/O variables](../media/images/ccw/variables-global-io.png),
[array data type](../media/images/ccw/array-data-type.png).

## Naming convention

| Prefix | Meaning | Example |
|---|---|---|
| `Sts_` | State or status. Something that *is* true. | `Sts_Running` |
| `Cmd_` | Command. What the logic *wants* an output to do. Gated by `Sts_Running` in the output section. | `Cmd_Left_Conv` |
| `Pls_` | One-scan pulse. TRUE for exactly one scan. | `Pls_Box_Passed` |
| `Cnt_` | Counter value. | `Cnt_Boxes_In_Process` |
| `Class_` | Box height class value or queue. | `Class_Queue` |
| `Ft_` / `Rt_` | Falling-edge / rising-edge trigger instance. | `Ft_Left_Exit` |
| `FFL_` / `FFU_` | FIFO load / unload instance. | `FFL_Queue_Load` |
| `CTUD_` | Up/down counter instance. | `CTUD_Boxes_In_Process` |
| `UPPER_CASE` | Constant. | `CLASS_SHORT` |

## Constants and data

| Name | Type | Initial | Description |
|---|---|---|---|
| `CLASS_SHORT` | INT | 1 | Class value for a short box. |
| `CLASS_TALL`  | INT | 2 | Class value for a tall box. |
| `Class_New`   | INT |   | Class of the box that has just passed the height sensors. |
| `Class_Active`| INT |   | Class of the box currently at the front of the turntable (read from the queue). |
| `Class_Queue` | `Class_Queue_Array` | | FIFO storage: classes of boxes that are measured but not yet sorted. |
| `Class_Queue_Ctrl` | FF_LF_CON | Length 10 | FIFO control structure. `Length` must equal the array size. |
| `Cnt_Boxes_In_Process` | DINT | 0 | Boxes between the entry sensor and an exit sensor. |

**User-defined array type**

| Name | Element type | Dimension | Notes |
|---|---|---|---|
| `Class_Queue_Array` | INT | 0..9 (10 elements) | Queue depth. `Class_Queue_Ctrl.Length` is set to 10 to match. A mismatch gives FIFO error 6. |

## Status (`Sts_`)

| Name | Type | Description |
|---|---|---|
| `Sts_Running` | BOOL | Cell is running. Sealed in by Start, dropped when the run permissive is lost (R05). |
| `Sts_Run_Permit` | BOOL | All run conditions are met (R04). |
| `Sts_EStop_Latched` | BOOL | E-stop has been pressed and not yet reset (R02, R03). |
| `Sts_Tall_Box_Seen` | BOOL | The current box has blocked both height beams (R06). |
| `Sts_Class_Valid` | BOOL | `Class_Active` holds a valid class for the box on the turntable (R15). |
| `Sts_Active_Short` | BOOL | Box on the turntable is short (R16). |
| `Sts_Active_Tall` | BOOL | Box on the turntable is tall (R17). |
| `Sts_Turntable_Loading` | BOOL | A box is currently being loaded onto the turntable (R18, R19). |
| `Sts_Queue_Full` | BOOL | FIFO queue is full (from `FFL.Full`). |
| `Sts_Queue_Empty` | BOOL | FIFO queue is empty (from `FFU.Empty`). |
| `Sts_Queue_Read_Done` | BOOL | FIFO read finished (from `FFU.Done`). |
| `Sts_Queue_Read_Error` | BOOL | FIFO read error (from `FFU.Error`). |
| `Sts_Counter_Reset` | BOOL | Counter reset: first scan, or simulation not running (R33). |

## Commands (`Cmd_`)

| Name | Type | Description | Output |
|---|---|---|---|
| `Cmd_Feeder` | BOOL | Run the feeder. | `BOOL_OUT_0` |
| `Cmd_Entry_Conv` | BOOL | Run the entry conveyor. | `BOOL_OUT_1` |
| `Cmd_Turntable_Load` | BOOL | Turntable rollers in the load direction. | `BOOL_OUT_2` |
| `Cmd_Turntable_Unload` | BOOL | Turntable rollers in the unload direction. | `BOOL_OUT_3` |
| `Cmd_Turn` | BOOL | Turn the turntable. | `BOOL_OUT_4` |
| `Cmd_Left_Conv` | BOOL | Run the left exit conveyor. | `BOOL_OUT_5` |
| `Cmd_Right_Conv` | BOOL | Run the right exit conveyor. | `BOOL_OUT_6` |

## Pulses (`Pls_`)

| Name | Type | Description |
|---|---|---|
| `Pls_Box_Passed` | BOOL | Box has passed the low beam (falling edge, R07). |
| `Pls_Queue_Load` | BOOL | Push a class into the queue (R11). |
| `Pls_Queue_Read` | BOOL | Pop a class from the queue (R13). |
| `Pls_Left_Exit` | BOOL | Box has left the left exit sensor (R28). |
| `Pls_Right_Exit` | BOOL | Box has left the right exit sensor (R29). |
| `Pls_Box_Exited` | BOOL | Box has left either exit (R32). |

## Function block instances

| Name | Type | Used in | Purpose |
|---|---|---|---|
| `FT_Low_Beam` | F_TRIG | R07 | Falling edge of the low beam. |
| `Rt_At_Front` | R_TRIG | R13 | Rising edge: box arrives at the front. |
| `Rt_Turn_Start` | R_TRIG | R22 | Rising edge: start the turn. |
| `Ft_Unload_Right` | F_TRIG | R23 | Box leaves the turntable to the right. |
| `Ft_Unload_Left` | F_TRIG | R23 | Box leaves the turntable to the left. |
| `Ft_Left_Exit` | F_TRIG | R28 | Box leaves the left exit sensor. |
| `Ft_Right_Exit` | F_TRIG | R29 | Box leaves the right exit sensor. |
| `FFL_Queue_Load` | FFL | R12 | FIFO load. |
| `FFU_Queue_Read` | FFU | R14 | FIFO unload. |
| `CTUD_Boxes_In_Process` | CTUD | R34 | Up/down counter. |

Total: 43 local variables.
