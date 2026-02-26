# Repository Overview

This repository is a structured and centralized **collection of my personal
system configurations, workflow customizations, and environment-level
adjustments** for development tools and general software setups.

**The primary objective of this project is not instructional**. It is not
intended to replace official documentation or provide comprehensive step-by-step
guides. Instead, **it functions as a curated archive of ready-to-use
configuration files** that represent how I structure, optimize, and maintain my
working environment.

To maintain consistency across different tools and components, **each section of
this repository may follow a standardized documentation structure**. When
applicable, the following topics are used:

- `Overview`: A brief description of what the tool or tweak is, its main
  purpose, and why it is relevant. May include a link to the official
  documentation and minimal installation guidance when necessary.

- `Configurations`: Links to the configuration files provided in this
  repository, with a short explanation of their structure and what aspects of
  the tool they control.

- `Inspirations`: References to themes, repositories, or style guides that
  influenced the visual design or structural approach, when applicable.

- `Tips`: Practical notes and small technical clarifications that help avoid
  common mistakes or improve compatibility and usability.

- `Known Issues`: A short summary of limitations or edge cases, with links to
  official documentation or issue trackers when relevant.

The emphasis is on standardization, organization, and portability. **All files
are grouped into dedicated directories with clear naming conventions**, ensuring
that the repository remains maintainable and easy to navigate. The structure is
intentionally uniform to reduce friction for anyone who wants to reuse or adapt
the configurations.

If you find any part of this repository useful, feel free to copy, adapt, or
fork it according to your own requirements. **It should be viewed as a practical
reference implementation rather than a prescriptive standard**.

> [!IMPORTANT]

> Some configurations may assume familiarity with the associated tools, as the
> focus is on sharing finalized setups rather than explaining every individual
> configuration decision.

# Table of Contents

- [Browser Extensions](#browser-extensions)
- [Komorebi](#komorebi)
- [Style Overrides](#style-overrides)
- [VS Code](#vs-code)
- [Windows Debloat Install](#windows-debloat-install)
- [YASB - Yet Another Status Bar](#yasb---yet-another-status-bar)

<!-- --- --- Komorebi --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- --- -->

## Komorebi

### Overview

[**_`Komorebi`_**](https://github.com/LGUG2Z/komorebi) **is a tiling window
manager for Windows** that automatically organizes open application windows into
structured layouts. Instead of manually resizing and positioning windows, it
arranges them in predefined tiling patterns, improving productivity and workflow
efficiency. It is especially useful for users who prefer keyboard-driven
navigation and a more organized desktop experience.

To install Komorebi, **you can download it directly from the official repository
or install it using a package manager**. After installation, you may need to
configure it and set it to start with Windows. Since the steps can vary, it’s
important to follow the official
[**_`documentation`_**](https://komorebi-starlight.lgug2z.workers.dev/guides/installation/)
to ensure everything is set up correctly.

> [!WARNING]

> The documentation provides important recommendations, such as:
> [**_`enabling long path support`_**](http://komorebi-starlight.lgug2z.workers.dev/guides/installation/#suggested-system-settings)
> and
> [**_`disabling unnecessary system animations`_**](https://lgug2z.github.io/komorebi/installation.html#disabling-unnecessary-system-animations).

### Configurations

You can access my configuration file here:

- [**_`komorebi.json`_**](./komorebi/komorebi.json)
- [**_`whkdrc`_**](./komorebi/whkdrc)

Komorebi is configured primarily through the
[**_`komorebi.json`_**](https://komorebi-starlight.lgug2z.workers.dev/reference/komorebi-windows/)
file, where you define layouts, workspace behavior, window rules, and general
manager settings. This file controls how windows are arranged, how many
workspaces are available, and how specific applications should behave.

A
[**_`whkdrc`_**](https://lgug2z.github.io/komorebi/example-configurations.html#whkdrc)
file is used by [**_`WHKD`_**](https://github.com/LGUG2Z/whkd), a Windows hotkey
daemon, to map key combinations to commands. WHKD essentially runs
[**_`Komorebic`_**](https://komorebi-starlight.lgug2z.workers.dev/reference/komorebic-windows/)
in the background, interpreting the whkdrc bindings and executing actions. In
Komorebi, it manages shortcuts for controlling the window manager, such as
changing layouts, moving windows, switching workspaces, and resizing containers.

Here is a table showing the **keybindings** from my personal whkdrc
configuration:

| Shortcut                    | Action Description                     |
| --------------------------- | -------------------------------------- |
| alt + escape                | Close the current window               |
| alt + a                     | Move focus to the window on the left   |
| alt + s                     | Move focus to the window below         |
| alt + w                     | Move focus to the window above         |
| alt + d                     | Move focus to the window on the right  |
| win + a                     | Shift the current window left          |
| win + s                     | Shift the current window down          |
| win + w                     | Shift the current window up            |
| win + d                     | Shift the current window right         |
| alt + space                 | Swap or promote the current window     |
| alt + z                     | Toggle floating mode                   |
| alt + x                     | Toggle monocle mode (maximized window) |
| alt + r                     | Stack the window to the left           |
| alt + f                     | Stack the window downward              |
| alt + t                     | Stack the window upward                |
| alt + g                     | Stack the window to the right          |
| alt + c                     | Remove window from stack               |
| alt + q                     | Go to previous stacked window          |
| alt + e                     | Go to next stacked window              |
| alt + oem_plus (+)          | Widen window horizontally              |
| alt + oem_minus (-)         | Narrow window horizontally             |
| alt + shift + oem_plus (+)  | Increase window height                 |
| alt + shift + oem_minus (-) | Decrease window height                 |
| alt + 1                     | Switch to workspace 0                  |
| alt + 2                     | Switch to workspace 1                  |
| alt + 3                     | Switch to workspace 2                  |
| alt + 4                     | Switch to workspace 3                  |
| alt + 5                     | Switch to workspace 4                  |
| alt + 6                     | Switch to workspace 5                  |
| win + 1                     | Move window to workspace 0             |
| win + 2                     | Move window to workspace 1             |
| win + 3                     | Move window to workspace 2             |
| win + 4                     | Move window to workspace 3             |
| win + 5                     | Move window to workspace 4             |
| win + 6                     | Move window to workspace 5             |
| alt + j                     | Switch to the next layout              |
| alt + k                     | Switch to the previous layout          |

> [!WARNING]

> In my komorebi.json config, I use the
> [**_`JetBrains Mono`_**](https://www.jetbrains.com/lp/mono/) font. You can
> change it to any font you like.

### Tips

#### Windows Virtual Keys

In my whkdrc for Komorebi, **I use OEM keys (`oem_1`, `oem_plus`, `oem_102`,
etc.) for shortcuts**. They let me map layout-specific characters, ensuring my
keybindings work across different keyboard layouts.

On Windows, keys like `oem_1`, `oem_plus`, and `oem_102` refer to **OEM
(Original Equipment Manufacturer) keys**. These are layout-dependent keys,
usually punctuation or special character keys, whose output changes depending on
the keyboard language (for example ;, `` ` ``, `~`, `[`, `]`, etc.). The
numbering follows **Windows virtual-key codes**. The number does not represent
the printed symbol, but a layout-specific key position. Because of that, the
**mapped character can vary from country to country**.

To determine the correct OEM value, use the
[**_`kbdlayout`_**](https://kbdlayout.info/) website, which provides the
virtual-key code associated with each key in a specific keyboard layout.

For reference, consult the official Microsoft documentation for the
[**_`United States Keyboard Layout`_**](https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes)
to review the standard virtual-key definitions. If you are working with a
different keyboard layout, such as
the[**_`Brazilian Keyboard Layout (ABNT2)`_**](https://kbdlayout.info/KBDBR/virtualkeys),
you can use the kbdlayout website as previously described to look up the
corresponding virtual-key mappings and OEM values.

### Known Issues

Komorebi has a few known issues, such as occasional inconsistencies after waking
from sleep, minor crashes in edge cases, and problems with window placement when
using multiple workspaces. These issues are actively being addressed, and
updates often improve stability and performance.

> [!WARNING]

> Many common issues are already explained in the
> [**_`documentations`_**](https://komorebi-starlight.lgug2z.workers.dev/guides/known-issues/)
