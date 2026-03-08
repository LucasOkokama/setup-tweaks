## Installation Guide

### Terminal Setup

1. Install **Nerd Fonts** to ensure proper rendering of icons and extended
   glyphs used by the terminal prompt:

   ```bash
   winget install -e --id DEVCOM.JetBrainsMonoNerdFont
   ```

   Alternatively, the font can be downloaded manually from the official
   [**`Nerd Fonts`**](https://www.nerdfonts.com/font-downloads#:~:text=JetBrains)
   website

1. Open the Windows Terminal `settings.json` configuration file and apply the
   following changes:
   - Insert the contents of [**`pwsh-general.json`**](./pwsh-general.json) at
     the `root level` of the settings file.

   - Insert the contents of [**`pwsh-defaults.json`**](./pwsh-defaults.json)
     inside the **`defaults`** section.

   - Insert the contents of [**`pwsh-scheme.json`**](./pwsh-scheme.json) inside
     the **`schemes`** array.

1. In the `list` array, modify the PowerShell profile entry by adding the
   `-NoLogo` argument to the `commandline` field. This **removes the default
   startup banner** when opening a new terminal session.

   It should look similar to the following:

   ```json
   "commandline": "%SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe -NoLogo",
   ```

<br>
<br>

### Prompt Customization

1.  Install Oh My Posh using the following command:

    ```powershell
    winget install JanDeDobbeleer.OhMyPosh -s winget
    ```

1.  From PowerShell, open your **PowerShell profile file** in notepad, which is
    executed automatically every time a new shell session starts:

    ```powershell
    notepad $PROFILE
    ```

    If the **profile file does not exist**, create it first and then run the
    previous command again:

    ```powershell
    New-Item -Path $PROFILE -Type File -Force
    ```

    On some systems, **PowerShell may block script execution by default**. If
    you encounter a permissions error, enable script execution with:

    ```powershell
      Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
    ```

1.  Inside your PowerShell **`.ps1` profile file**, add the following line to
    initialize `Oh My Posh` when the shell starts:

    ```powershell
    oh-my-posh init pwsh --config "$env:USERPROFILE\.config\oh-my-posh\theme.json" | Invoke-Expression
    ```

1.  Create a `theme.json` file in the following directory and paste the contents
    of [**`ohmyposh-theme.json`**](./ohmyposh-theme.json) into it:

    ```powershell
    C:\Users\<Your_Username>\.config\oh-my-posh\theme.json
    ```

1.  After editing, reload the profile:

    ```powershell
    . $PROFILE
    ```

<br>
<br>

### Terminal Icons

1. Open PowerShell as administratos and install Terminal Fonts:

   ```powershell
   Install-Module -Name Terminal-Icons -Repository PSGallery
   ```

1. From PowerShell, open your **PowerShell profile file** in notepad, which is
   executed automatically every time a new shell session starts:

   ```powershell
   notepad $PROFILE
   ```

   If the **profile file does not exist**, create it first and then run the
   previous command again:

   ```powershell
   New-Item -Path $PROFILE -Type File -Force
   ```

   On some systems, **PowerShell may block script execution by default**. If you
   encounter a permissions error, enable script execution with:

   ```powershell
     Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
   ```

1. Inside your PowerShell **`.ps1` profile file**, add the following line to
   initialize `Terminal Icons` when the shell starts:

   ```powershell
   Import-Module -Name Terminal-Icons
   ```

1. After editing, reload the profile:

   ```powershell
   . $PROFILE
   ```

<br>
<br>

### FastFetch

1.  Install Oh My Posh using the following command:

    ```powershell
    winget install fastfetch
    ```

2.  Create the `config.jsonc` file at the following location:

    ```
    C:\Users\<Your_Username>\.config\fastfetch\config.jsonc
    ```

3.  Open the `config.jsonc` file and paste the configuration from
    [**`fastfetch-config.jsonc`**](./fastfetch-config.jsonc).

<!-- prettier-ignore-start -->
> [!NOTE]
> For a complete overview of how FastFetch configuration works, you can visit the
[**FastFetch Configuration Wiki**](https://github.com/fastfetch-cli/fastfetch/wiki/Configuration).
This page explains the structure of the configuration file and how to customize
modules, formatting, and other settings.
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
> [!NOTE]
> You can check the [**`all.jsonc`**](https://github.com/fastfetch-cli/fastfetch/blob/dev/presets/all.jsonc) file, which contains all available configuration options. Alternatively, you can run the command below to see what each configuration module would output:
```
fastfetch -c all.jsonc
```
<!-- prettier-ignore-end -->
