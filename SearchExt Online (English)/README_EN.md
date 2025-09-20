# SearchExt Online - Installation Guide

## What is this?
A simple solution to replace the discontinued "SearchExt" program. Adds a context menu entry that allows you to look up file extensions online.

## Files:

### 1. SearchExtOnline_EN.reg (Simple Version)
- Adds a single context menu entry "Search file extension online..."
- Searches on file-extensions.org
- Opens in default browser

### 2. SearchExtOnline_Advanced_EN.reg (Advanced Version)
- Adds a submenu with multiple search options:
  - File-Extensions.org
  - FileInfo.com  
  - Google Search
  - Open all simultaneously

### 3. RemoveSearchExtOnline_EN.reg (Uninstallation)
- Removes all SearchExt context menu entries

## Installation:

1. **Run as Administrator** - Right-click on the desired .reg file
2. Select "Run as administrator"
3. Confirm the registry change

## Usage:

- Right-click on any file
- Select "Search file extension online..." (simple version)
- Or "Search file extension" → Select search engine (advanced version)

## Uninstallation:

- Run RemoveSearchExtOnline_EN.reg as Administrator

## Customization:

You can edit the URLs in the .reg files to use other search engines:

- Open the .reg file in a text editor
- Change the URL after "Start-Process"
- Save and import again

## Examples for other search engines:

```powershell
# FILExt.com
Start-Process ('https://filext.com/file-extension/' + $ext)

# WhatIs.com
Start-Process ('https://whatis.techtarget.com/fileformat/' + $ext)

# DuckDuckGo
Start-Process ('https://duckduckgo.com/?q=' + $ext + '+file+extension')
```

## Troubleshooting:

- **No context menu entry visible**: Import registry file as Administrator
- **Browser doesn't open**: Check PowerShell Execution Policy (see below)
- **Wrong search**: File might not have an extension

### PowerShell Execution Policy Issues:

If the context menu entries don't work, it might be due to PowerShell Execution Policy. 

**Solution 1 - Temporary change (for this execution only):**
The registry entries already use `-NoProfile` and `-Command`, which bypasses most Execution Policy restrictions.

**Solution 2 - If problems persist:**
Modify the registry entries to temporarily set the Execution Policy:

```reg
@="powershell.exe -NoProfile -ExecutionPolicy Bypass -Command \"$ext=[System.IO.Path]::GetExtension('%1').TrimStart('.'); if($ext -ne '') { Start-Process ('https://www.file-extensions.org/search?search=' + $ext) }\""
```

**Solution 3 - System-wide temporary change:**
```powershell
# Show current policy
Get-ExecutionPolicy

# Set to Bypass temporarily (current PowerShell session only)
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process

# Or system-wide temporarily (as Administrator)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**Important:** The `-ExecutionPolicy Bypass` parameter in the registry entries bypasses the Execution Policy only for this single command execution, without permanently changing system settings.

## Security Notice:

These registry entries execute PowerShell commands. The code is harmless and only performs browser calls, but be generally careful with registry changes.