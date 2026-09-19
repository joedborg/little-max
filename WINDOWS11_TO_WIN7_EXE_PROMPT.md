Build me a Windows executable on this Windows 11 machine that will run on a separate Windows 7 machine with no Python/Node/runtime installed.

Goal

- Build a native Windows serial probe executable for FTDI serial devices.
- Output should be one .exe plus one small .cmd launcher.
- Target compatibility: Windows 7 (prefer broad compatibility, including older systems where possible).
- No dependencies required on target machine.

What to generate

1. A C source file implementing:

- Open COM port passed as arg (example COM3, COM12).
- Configure 8N1 serial.
- Baud arg (default 230400), reps arg (default 20).
- Wake sequence using DTR/RTS toggles.
- Send these frames with inter-byte delay 10ms:
  - 02 D4 7F 03 00
  - 02 D4 82 03
  - 02 37 D0 03
  - 02 2F D7 03
- After each frame, wait 100ms, read response bytes, print TX/RX in hex.
- Repeat for reps cycles.
- Clear RX buffer before each TX.
- Exit with nonzero code on failure.

2. A build .bat script that:

- Tries MSVC cl first, then gcc if cl not available.
- Produces xp_probe_native.exe.
- Uses release flags and static runtime where practical.
- Prints clear success/failure messages.

3. A run .cmd launcher that:

- Usage: run_native_probe.cmd COM3 [baud] [reps]
- Calls xp_probe_native.exe with defaults when omitted.

4. A short README with exact build and run commands.

Compatibility requirements

- Prefer APIs available on Windows 7.
- Avoid dependencies on modern redistributables when possible.
- If using MSVC, use flags that minimize runtime dependency issues.
- If using gcc, produce a static binary if possible.

After generating files

- Build the executable.
- Show me the exact build command used and whether it succeeded.
- Show the exact files to copy to the Windows 7 target.
- Provide a quick smoke-test command for the target machine.

If build fails, fix errors and retry automatically until success or a concrete blocker is found.
