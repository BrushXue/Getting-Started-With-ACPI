# Fixing Power Management

**Intel CPUs only**

CPU naming is fairly easy to figure out as well, open your decompiled DSDT and search for `Processor`. This should give you a result like this:

![](https://i.imgur.com/U3xffjU.png)

As we can see, the first processor in our list is `PR00`. This is what we'll be applying the `plugin-type=1` property too. Now grab [SSDT-PLUG](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/SSDT-PLUG.dsl) and replace the default `CPU0` with our `PR00`. Note that there are 2 mentions of `CPU0` in the SSDT.

There are also some edge cases with `Processor`, specifically on HEDT series like X79, X99 and X299. This edge case is that the ACPI path is much longer and not so obvious:

![](https://i.imgur.com/HzOmbx2.png)

If we then search for instances of `CP00` we find that it's ACPI path is `SB.SCK0.CP00`:

![](https://i.imgur.com/CtL6Csn.png)

So for this X299 board, we'd change `\_PR.CPU0` with `\_SB.SCK0.CP00` and `External (_PR_.CPU0, ProcessorObj)` with `External (_SB_.SCK0.CP00, ProcessorObj)`