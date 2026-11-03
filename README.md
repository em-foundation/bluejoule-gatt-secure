# BlueJoule-GATT-Secure

Experimental release candidate for a BLE secure connection benchmark. This repository starts on RC-26.12 and is not a formal release. No fresh captures or scores are claimed yet.

## Workload

A peripheral advertises the BlueJoule-GATT service, connects to the secure reference central, performs LE Secure Connections / Just Works pairing without bonding, establishes encryption, completes targeted discovery and the Command write (0x04) / Status read (0x84), then disconnects and returns toward idle. Characteristic data requires encryption. Pairing repeats rather than reusing stored bonds. Advertising before connection is outside the intended scored transaction window.

The secure central is a distinct fixture from ordinary GATT. Before measurements, verify pairing, encryption, successful data exchange and disconnect. Final scan boundaries and scoring settings must be validated using the selected EM•Scope version; no unsupported scan/recover commands are prescribed here.

## Layout

- central/: secure nRF52 DK central source and build configuration
- peripheral/: copied GATT build/load/size tooling for nRF52832, nRF54L15 and CC2340R5; security config is always merged last
- captures/: six platform/implementation folders, each with a 3V0-J placeholder (3.0 V, Joulescope)

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

This is initially a deliberate copy of the ordinary GATT machinery. Consolidation will follow concrete reuse requirements. Improvements and additional targets are welcome on RC-26.11; preserve workload equivalence and distinguish contributor evidence from independently recaptured results.
