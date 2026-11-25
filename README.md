# LineageOS 24.0 UNOFFICIAL — Galaxy M32 4G (SM-M325F, India)

Unofficial Android 17 test build for **SM-M325F (`m32dd`) only**. Do not flash on the EU SM-M325FV. Recovery may accept either ZIP because they share the `m32` device assertion. That is not a compatibility check.

## Downloads

[GitHub Releases](../../releases) · [Flash instructions](INSTALL.md)

2026-11-24 · Build incremental `1795557904` · Security patch `2026-11-01`

Back up your data off-device before unlocking or formatting. Never relock the bootloader while running custom firmware. This is a testing release, not recommended if you rely on this phone for calls or dual-SIM service.

## What's working

Confirmed by testing on SM-M325F: system boots, audio playback works, AudioFX no longer crashes, the voice recorder works, and both front and rear cameras record video. SIM slot 2 registered on LTE and was used for call testing; this does not confirm mobile data or reliable call quality.

## Known issues

- **SIM slot 1:** Reported not working. SIM detection, network registration, calls, and mobile data still need separate testing. Dual SIM is not confirmed working.
- **Call echo:** The caller may hear their own voice echoed back. The affected audio routes and cause are not yet confirmed.
- **KernelSU:** Not included as a supported feature; aftermarket compatibility has not been tested.

Other features are not covered by this test report. When reporting a problem, include your exact model, build incremental, and reproduction steps. Remove phone numbers, SIM identifiers, and other private data before posting logs publicly.

## Credits

Device trees: [samsung-mt6768-aosp](https://github.com/samsung-mt6768-aosp). TWRP: Mujammil. Thanks to Loki for testing the India build.
