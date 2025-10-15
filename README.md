# HWBP-DEP-Bypass

[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-blue.svg)](https://www.microsoft.com/windows)
[![Language](https://img.shields.io/badge/Language-C-brightgreen.svg)](https://en.wikipedia.org/wiki/C_(programming_language))
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Research](https://img.shields.io/badge/Type-Security%20Research-red.svg)](https://github.com)

A proof-of-concept implementation demonstrating how to execute code from non-executable memory on Windows x64 systems by combining hardware breakpoints, vectored exception handling (VEH), and instruction emulation—bypassing DEP/NX protection without modifying memory permissions.

> **📖 Complete Technical Write-Up**: For a comprehensive deep-dive into this technique with detailed explanations, debugging walkthrough with screenshots, and security analysis, read the full blog post:  
> **[Executing Code from Non-Executable Memory: A Hardware Breakpoint Approach](https://yourblog.com/hwbp-dep-bypass)**

## ⚠️ Disclaimer

**This project is for educational and security research purposes only.**

This code demonstrates security concepts and should only be used in controlled environments for learning, testing, or legitimate security research. The author does not condone malicious use and accepts no responsibility for misuse of this code. Use at your own risk and only on systems you own or have explicit permission to test.

## 📖 Overview

Data Execution Prevention (DEP) and No-Execute (NX) are memory protection mechanisms that prevent code execution from pages marked as non-executable. This proof-of-concept demonstrates a technique to bypass these protections by exploiting the timing of hardware breakpoint checks in the CPU pipeline.

### How It Works

The technique combines three Windows and CPU mechanisms to execute code from non-executable memory:
