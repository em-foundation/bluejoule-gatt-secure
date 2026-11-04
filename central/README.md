# Secure GATT reference central

Source copied from bluejoule-gatt-dev/central. The build always merges prj.conf with prj_secure.conf. Use the nRF52 DK target nrf52dk/nrf52832 in the prepared NCS 3.0.2 environment:

```
python -m west build --no-sysbuild -p always -b nrf52dk/nrf52832 -d build .
```

Run with the vendor environment's Python and tool paths. The image is build/zephyr/zephyr.hex. Program a separate central board with Nordic tools. This secure central must replace the ordinary GATT central during secure measurements. Pairing uses LE Secure Connections / Just Works, no bonding, followed by encrypted GATT. Keep its image and source revision fixed across peripheral comparisons. Configuration uses 7.5 ms connection interval, zero latency, write 0x04/read 0x84. Functional hardware verification of this copied fixture is pending.
