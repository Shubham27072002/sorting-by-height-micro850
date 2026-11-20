# Factory I/O scene

`Sorting_by_Height_Advanced_modified.factoryio` is the stock **Sorting by Height (Advanced)** scene
from Factory I/O (v2.5.10, Ultimate Edition) with two small changes:

* the **At entry** sensor (`BOOL_IN_0`) is moved slightly back, and
* the **At turntable** sensor is moved slightly forward.

These are the changes made to the stock layout. The sensor and actuator names are unchanged, so the PLC program works with the stock driver mapping.

## Driver settings

1. Open the scene in Factory I/O.
2. Go to **Driver** and select **Allen-Bradley Micro800**.
3. Set the host to the IP address of the PC that runs the Micro800 simulator (shown in CCW),
   then press **Connect**.

The resulting tag mapping is shown in
[`../media/images/ccw/factory-io-driver-config.png`](../media/images/ccw/factory-io-driver-config.png)
and listed in [`../docs/io-table.md`](../docs/io-table.md).
