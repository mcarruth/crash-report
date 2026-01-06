# CrashRpt - Crash Reporting Library for Windows

> **⚠️ Historical Archive Notice**
> This library was originally developed in 2003 and last updated in 2012. It is preserved here for historical purposes and legacy application support. For modern crash reporting solutions, consider contemporary alternatives like [Sentry](https://sentry.io), [Crashlytics](https://firebase.google.com/products/crashlytics), or [Backtrace](https://backtrace.io).

## Overview

CrashRpt is a lightweight C++ crash reporting framework for Windows applications that automatically intercepts unhandled exceptions, generates comprehensive debug reports, and optionally sends them to developers via email or HTTP.

Originally published on [CodeProject](https://www.codeproject.com/Articles/54666/Add-Crash-Reporting-to-Your-Applications-with-the) and used by thousands of Windows applications in the 2000s-2010s era.

## Features

### Automatic Crash Capture
- Intercepts unhandled C++ exceptions, SEH exceptions, and critical CRT errors
- Handles stack overflow crashes in a separate thread for robustness
- Captures crashes in all application threads

### Comprehensive Debug Information
- **Minidump generation** - Full crash dump with call stacks, local variables, CPU registers, and loaded modules
- **XML crash log** - Structured report with exception details, OS version, processor info, and module list
- **Screenshot capture** - Optional desktop/window screenshots at crash time
- **Custom file attachment** - Include application logs, configuration files, or other relevant data

### User-Friendly Error Dialogs
- Customizable crash dialog with privacy controls
- Allows users to review crash contents before sending
- Supports user comments and email address collection
- Multi-language support (20+ languages included)

### Flexible Delivery Options
- **Email** - SMTP with authentication support (including Gmail, Yahoo, etc.)
- **HTTP/HTTPS** - POST crash reports to your web server
- **Local storage** - Save crash reports to disk for manual collection

### Developer Tools
- **CrashRptProbe** - API for parsing and analyzing crash reports
- **Crash deduplication** - XML format enables grouping similar crashes
- **Symbol support** - Full integration with PDB debug symbols

## Platform Support

- **Windows XP and later** (including Windows 7, 8, 10)
- **Visual Studio 2005-2010** (VS .NET 2003 support removed in v1.3.1)
- **32-bit and 64-bit** applications
- **MFC, WTL, and native Win32** applications

## Quick Start

### 1. Installation

Include the CrashRpt library in your Visual Studio project:

```cpp
#include "CrashRpt.h"
#pragma comment(lib, "CrashRpt.lib")
```

### 2. Basic Usage

Initialize crash reporting at application startup:

```cpp
CR_INSTALL_INFO info;
memset(&info, 0, sizeof(CR_INSTALL_INFO));
info.cb = sizeof(CR_INSTALL_INFO);
info.pszAppName = _T("MyApp");
info.pszAppVersion = _T("1.0.0");
info.pszEmailSubject = _T("MyApp Crash Report");
info.pszEmailTo = _T("crashes@mycompany.com");
info.pszUrl = _T("http://myserver.com/crashrpt.php");
info.uPriorities[CR_HTTP] = 3;  // Try HTTP first
info.uPriorities[CR_SMTP] = 2;  // Then SMTP
info.uPriorities[CR_SMAPI] = 1; // Then Simple MAPI

int nResult = crInstall(&info);
if(nResult != 0) {
    // Handle installation failure
}
```

### 3. Cleanup

Uninstall before application exit:

```cpp
crUninstall();
```

### 4. Testing

Trigger a test crash to verify integration:

```cpp
crEmulateCrash(CR_SEH_EXCEPTION);
```

## Building from Source

### Requirements
- CMake 2.8 or later
- Visual Studio 2005-2010
- WTL (Windows Template Library) 8.0 or later
- Windows SDK

### Build Steps

```bash
# Generate Visual Studio solution
cmake -G "Visual Studio 10 2010" .

# Or use the provided solution file
# Open CrashRpt_vs2010.sln in Visual Studio

# Build all configurations
clean.bat
```

## Project Structure

```
crash-report/
├── bin/              # Compiled binaries
├── demos/            # Sample applications
├── docs/             # Full documentation (HTML help)
├── include/          # Public header files
│   ├── CrashRpt.h        # Main API
│   └── CrashRptProbe.h   # Report analysis API
├── lang_files/       # Localization resources (20+ languages)
├── lib/              # Library files
├── processing/       # Server-side crash processing tools
├── reporting/        # Core crash reporting code
├── tests/            # Unit tests
├── thirdparty/       # Dependencies (dbghelp, zlib, etc.)
└── CMakeLists.txt    # CMake build configuration
```

## Documentation

Full documentation is available in the `docs/` directory. Open `docs/index.html` in your browser for:
- Detailed API reference
- Integration guides
- Server-side processing examples
- Troubleshooting tips

## Version History

**v1.3.1** (September 2012) - Final release
- Stack overflow handling improvements
- SMTP authentication support
- File attachment management via context menu
- User email address persistence
- Removed VS .NET 2003 support

See [Changelog.txt](Changelog.txt) for complete version history.

## License

BSD 3-Clause License

Copyright (c) 2003-2012, Michael Carruth and Contributors.
All rights reserved.

See [License.txt](License.txt) for full license text.

## Historical Context

CrashRpt was created in 2003 during the Windows XP era when most desktop applications had no built-in crash reporting. It became widely adopted in the Win32 development community and was featured on CodeProject, SourceForge, and later Google Code.

The library pioneered several concepts that are now standard in crash reporting:
- Minidump-based debugging for release builds
- User consent and privacy controls
- Automated crash deduplication
- Multi-channel delivery (email, HTTP, local)

While modern alternatives exist with cloud services and cross-platform support, CrashRpt remains relevant for:
- Legacy Windows applications still in production
- Embedded systems requiring offline crash collection
- Applications with strict data privacy requirements (on-premise reporting)
- Historical reference for crash reporting techniques

## Acknowledgments

Thanks to all contributors who helped develop and maintain this library over the years. Special recognition to the CodeProject community for feedback and bug reports.

## See Also

- [Original CodeProject Article](https://www.codeproject.com/Articles/54666/Add-Crash-Reporting-to-Your-Applications-with-the)
- [Windows Debugging Tools](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/)
- [Minidump Format Documentation](https://docs.microsoft.com/en-us/windows/win32/api/minidumpapiset/)

---

*Archived on GitHub: January 2026*
