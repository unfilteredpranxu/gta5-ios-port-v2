# GTA V2 iOS port — private diagnostic workspace

Keep this repository **private**. The uploaded migration archive contains game-engine assets.

## Run the build

1. Keep these three files in the repository root, committed to `main`:
   - `gta5-v2-package.part00`
   - `gta5-v2-package.part01`
   - `gta5-v2-package.part02`
2. Open **Actions → Reconstruct and Build V2 IPA**.
3. Choose **Run workflow** on `main`. Leave the extra-memory option enabled initially.
4. The workflow verifies/reconstructs the archive, checks V2 input hashes and scripts, then attempts a macOS/Xcode build.
5. On failure, download **V2-build-diagnostics**. On success, download **GTA5Port-ipa**.

The workflow removes a WABT command-line option unsupported by the pinned tool build. It also skips only the optional missing-Scaleform workaround when V2's generated C no longer matches that workaround's old pattern; the rest of preparation still runs and can report further blockers.

## Important limitations

- This is a **diagnostic migration candidate**, not a verified playable port. Successful compilation is not proof that the game boots or runs reliably.
- The bundled V2 inputs include the engine, manifest, shader index and shader packs, but not all 5,349 game-data files. Runtime play still requires the complete game-data mirror locally or a reachable asset server.
- The last diagnostics passed the asset/hash checks but stopped in `prepare_missing_scaleform.py` before Xcode compilation. The current workflow addresses that specific blocker; later compiler and device issues remain unverified.
- Some raw socket/poll syscall imports are still marked unported in the generated import report. Network-dependent paths may not work until those host calls are implemented and tested.
- Device validation must still cover launch, rendering, controls, memory pressure, streaming, save/load and sustained frame pacing on the target iPad.

Do not describe the build as playable until it has passed both the native build and real-device checks.
