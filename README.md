# HWBP-DEP-Bypass

[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-blue.svg)](https://www.microsoft.com/windows)
[![Language](https://img.shields.io/badge/Language-C-brightgreen.svg)](https://en.wikipedia.org/wiki/C_(programming_language))
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Research](https://img.shields.io/badge/Type-Security%20Research-red.svg)](https://github.com)

A proof-of-concept implementation demonstrating how to execute code from non-executable memory on Windows x64 systems by combining hardware breakpoints, vectored exception handling (VEH), and instruction emulation—bypassing DEP/NX protection without modifying memory permissions.

> **📖 Complete Technical Write-Up**: For a comprehensive deep-dive into this technique with detailed explanations, debugging walkthrough with screenshots, and security analysis, read the full blog post:  
> **[The Emulator's Gambit: Executing Code from Non-Executable Memory](https://redops.at/en/blog/the-emulators-gambit-executing-code-from-non-executable-memory)**

## ⚠️ Disclaimer

**This project is for educational and security research purposes only.**

This code demonstrates security concepts and should only be used in controlled environments for learning, testing, or legitimate security research. The author does not condone malicious use and accepts no responsibility for misuse of this code. Use at your own risk and only on systems you own or have explicit permission to test.

## 📖 Overview

Data Execution Prevention (DEP) and No-Execute (NX) are memory protection mechanisms that prevent code execution from pages marked as non-executable. This proof-of-concept demonstrates a technique to bypass these protections by exploiting the timing of hardware breakpoint checks in the CPU pipeline.

### How It Works

This technique bypasses DEP/NX by exploiting the timing of CPU hardware breakpoint checks, which occur *before* memory protection validation:

**1. Hardware Breakpoints Trigger First**
- CPU checks debug registers (DR0-DR7) before instruction fetch
- EXCEPTION_SINGLE_STEP fires before MMU examines page permissions
- NX bit is never checked

**2. VEH Captures Exceptions**
- Vectored Exception Handler gets first-chance notification
- Full access to CPU context (all registers, RIP, RSP, etc.)
- Can modify context and control execution flow

**3. Software Emulation**
- Read instruction bytes as data from non-executable memory
- Decode opcode and emulate behavior
- Update CPU context (increment RIP, adjust RSP for RET, etc.)
- Set next hardware breakpoint at new RIP

**The Result**: Each instruction triggers this cycle. Code executes from `.data` section (PAGE_READWRITE) without ever changing memory protection. DEP/NX remains active but is bypassed through software emulation.
