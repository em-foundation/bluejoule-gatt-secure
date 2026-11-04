# TI LP-EM-CC2755P10

Build with the existing TI Zephyr environment documented in the [CC2340R5 setup](../ti-cc2340r5/README.md). The CC2755P10 baseline uses TI SimpleLink Zephyr `v3.7.0-ti-9.14.02_ea` and Zephyr SDK 0.16.8. Use a short Windows build root.

From the peripheral directory:

```text
python build.py ti-cc2755p10
python load.py ti-cc2755p10 --dry-run
```

For loading, set `load.ti-cc2755p10.executable` to UniFlash DSLite and `load.ti-cc2755p10.config` to the CC2755P10 target configuration from your working UniFlash session. Use the same probe and power settings as that session. The loader uses the existing TI program and separate image verification commands. After inspecting the dry run, omit `--dry-run` to load. Use `--hex PATH` to select an existing compatible image.

## Secure baseline

The build merges `prj_secure.conf` and uses the dedicated Secure GATT central. The 25 s Joulescope capture at 3.0 V contains 23 complete transactions. Use `emscope scan --gap 15 --min-duration 75 --sleep-window 100 --event-window 650`. For this recording, set the saved analysis span width to 24703500 samples to exclude the unfinished final transaction. The 10 s projection is 1.37 EM•eralds, with 6551.6 µJ per event and 21.2 µW sleep power.

The linker map resolves `ECDH_computeSharedSecret` to `ECDHLPF3SW.c.obj`. TI's standalone SDK 9.20.00.81 selects `ECDHXXF3HSM` in its CC2755P10 `mac_sensor_ble_basic` SysConfig example, with secure driver and HSM runtime libraries. The Zephyr HAL source list and board config select software ECDH. Hardware ECDH integration in this Zephyr fork remains a question for TI.
