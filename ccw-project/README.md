# CCW project

Connected Components Workbench **23.0** solution for a Micro850 controller.

```
ccw-project/
└── sorting_boxes_advance/
    ├── sorting_boxes_advance.ccwsln   <- open this file in CCW
    └── Controller/                    <- controller, program and data types
```

## Open it

1. Install Connected Components Workbench (Standard or Developer edition).
2. Open `sorting_boxes_advance/sorting_boxes_advance.ccwsln`.
3. In the **Controller Organizer**, expand **Tasks > MainTask > Program1 > Sort_By_Height** to see the ladder.

## Run it with Factory I/O (no hardware)

1. Open the scene in [`../factory-io`](../factory-io) and select the **Allen-Bradley Micro800** driver.
2. In CCW, start the **Micro800 Simulator**, then build and download the program to it.
3. Connect the Factory I/O driver to the simulator, run the scene and press **Start** on the control panel.
   Auto must be selected.

If CCW reports a connection path problem, set the path to your own simulator in
the CCW **Connection Browser** (Connect button). The path is stored in `DevicePref.xml`, which is not part of this repository.

## Controller

| Item | Value |
|---|---|
| Controller | Micro850 |
| Program | `Sort_By_Height` (ladder, 45 rungs) |
| Data type | `Class_Queue_Array`: INT, 0..9 |
| Function blocks used | F_TRIG, R_TRIG, FFL, FFU, CTUD |
