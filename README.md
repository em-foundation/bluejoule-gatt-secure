# BlueJoule-GATT-Secure

Experimental release candidate for a BLE secure connection benchmark. Current results are from the RC-26.12 development branch.

## Current RC scores

RC-26.12 capture results at 3.0 V, projected at one transaction per 10 seconds.

| Capture | EM•eralds |
| --- | ---: |
| [nrf-52832-emscript/3V0-J](captures/nrf-52832-emscript/3V0-J/ABOUT.md) | 12.48 |
| [nrf-52832-zephyr/3V0-J](captures/nrf-52832-zephyr/3V0-J/ABOUT.md) | 9.82 |
| [nrf-54l15-emscript/3V0-J](captures/nrf-54l15-emscript/3V0-J/ABOUT.md) | 22.48 |
| [nrf-54l15-zephyr/3V0-J](captures/nrf-54l15-zephyr/3V0-J/ABOUT.md) | 20.43 |
| [ti-cc2340r5-emscript/3V0-J](captures/ti-cc2340r5-emscript/3V0-J/ABOUT.md) | 4.49 |
| [ti-cc2340r5-zephyr/3V0-J](captures/ti-cc2340r5-zephyr/3V0-J/ABOUT.md) | 1.47 |
| [ti-cc2755p10-zephyr/3V0-J](captures/ti-cc2755p10-zephyr/3V0-J/ABOUT.md) | 1.37 |

## Workload

A peripheral advertises the BlueJoule-GATT service, connects to the secure reference central, performs LE Secure Connections / Just Works pairing without bonding, establishes encryption, completes targeted discovery and the Command write (0x04) / Status read (0x84), then disconnects and returns toward idle. Characteristic data requires encryption. Pairing repeats rather than reusing stored bonds. Advertising before connection is outside the intended scored transaction window.

The secure central is a distinct fixture from ordinary GATT. Before measurements, verify pairing, encryption, successful data exchange and disconnect. Final scan boundaries and scoring settings must be validated using the selected EM•Scope version; no unsupported scan/recover commands are prescribed here.

## Layout

- central/: secure nRF52 DK central source and build configuration
- peripheral/: copied GATT build/load/size tooling for nRF52832, nRF54L15, CC2340R5, and CC2755P10. The build merges the security configuration last
- captures/: platform and implementation folders with 3.0 V Joulescope captures

## Build and load

From peripheral/ after copying/editing environments.example.json to environments.local.json:

```
python build.py platforms/nrf-52832
python build.py platforms/nrf-54l15
python build.py platforms/ti-cc2340r5
python load.py platforms/nrf-52832 --probe SERIAL
python load.py platforms/nrf-54l15 --probe SERIAL --recover
python size.py platforms/ti-cc2340r5
```

Recovery erases user nonvolatile memory. Use --hex PATH to load an explicit compatible image and --dry-run to inspect load/build commands without running them. See peripheral/README.md and platform notes for complete options. Local TI secure build output is kept separate from ordinary GATT output.

EM•Script builds remain in Emporium; this repository can retain the corresponding published HEX, build/source provenance and fresh captures. No EM•Script build environment is copied here.

## Collecting the six baselines

For each device and each implementation, retain the exact loaded firmware and source/tool revisions, confirm the secure transaction, and collect fresh raw data into captures/<platform>-<implementation>/3V0-J. Then run the agreed EM•Scope analysis/pack workflow and inspect generated metadata and results. Existing EMR 11–13 engineering captures remain historical evidence, not automatically scored or copied into these placeholders. PEDS activity/platform support for secure report generation must be checked before publishing results.

This is initially a deliberate copy of the ordinary GATT machinery. Consolidation will follow concrete reuse requirements. Improvements and additional targets are welcome on RC-26.12; preserve workload equivalence and distinguish contributor evidence from independently recaptured results.
