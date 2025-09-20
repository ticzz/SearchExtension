# SearchExt Online - Installation Guide  
(German version down below .. [ticzz/SearchExtension_Online](https://github.com/ticzz/SearchExtension?tab=readme-ov-file#searchext-online---installationsanleitung)

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

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# SearchExt Online - Installationsanleitung

## Was ist das?
Eine einfache Lösung, um das eingestellte "SearchExt" Programm zu ersetzen. Fügt einen Kontextmenü-Eintrag hinzu, mit dem Sie Dateierweiterungen online nachschlagen können.

## Dateien:

### 1. SearchExtOnline.reg (Einfache Version)
- Fügt einen einzelnen Kontextmenü-Eintrag "Dateierweiterung online suchen..." hinzu
- Sucht auf file-extensions.org
- Öffnet im Standard-Browser

### 2. SearchExtOnline_Advanced.reg (Erweiterte Version)
- Fügt ein Untermenü mit mehreren Suchoptionen hinzu:
  - File-Extensions.org
  - FileInfo.com  
  - Google Suche
  - Alle gleichzeitig öffnen

### 3. RemoveSearchExtOnline.reg (Deinstallation)
- Entfernt alle SearchExt Kontextmenü-Einträge

## Installation:

1. **Als Administrator ausführen** - Rechtsklick auf die gewünschte .reg-Datei
2. "Als Administrator ausführen" wählen
3. Bestätigen Sie die Registrierungsänderung

## Verwendung:

- Rechtsklick auf eine beliebige Datei
- "Dateierweiterung online suchen..." wählen (einfache Version)
- Oder "Dateierweiterung suchen" → Suchmaschine auswählen (erweiterte Version)

## Deinstallation:

- RemoveSearchExtOnline.reg als Administrator ausführen

## Anpassungen:

Sie können die URLs in den .reg-Dateien bearbeiten, um andere Suchmaschinen zu verwenden:

- Öffnen Sie die .reg-Datei in einem Texteditor
- Ändern Sie die URL nach "Start-Process"
- Speichern und erneut importieren

## Beispiel für andere Suchmaschinen:

```powershell
# FILExt.com
Start-Process ('https://filext.com/file-extension/' + $ext)

# WhatIs.com
Start-Process ('https://whatis.techtarget.com/fileformat/' + $ext)

# DuckDuckGo
Start-Process ('https://duckduckgo.com/?q=' + $ext + '+file+extension')
```

## Fehlerbehebung:

- **Kein Kontextmenü-Eintrag sichtbar**: Registry-Datei als Administrator importieren
- **Browser öffnet nicht**: PowerShell Execution Policy prüfen (siehe unten)
- **Falsche Suche**: Datei hat möglicherweise keine Erweiterung

### PowerShell Execution Policy Probleme:

Falls die Kontextmenü-Einträge nicht funktionieren, liegt es möglicherweise an der PowerShell Execution Policy. 

**Lösung 1 - Temporäre Änderung (nur für diese Ausführung):**
Die Registry-Einträge verwenden bereits `-NoProfile` und `-Command`, was die meisten Execution Policy Beschränkungen umgeht.

**Lösung 2 - Falls immer noch Probleme bestehen:**
Ändern Sie die Registry-Einträge, um die Execution Policy temporär zu setzen:

```reg
@="powershell.exe -NoProfile -ExecutionPolicy Bypass -Command \"$ext=[System.IO.Path]::GetExtension('%1').TrimStart('.'); if($ext -ne '') { Start-Process ('https://www.file-extensions.org/search?search=' + $ext) }\""
```

**Lösung 3 - Systemweite temporäre Änderung:**
```powershell
# Aktuelle Policy anzeigen
Get-ExecutionPolicy

# Temporär auf Bypass setzen (nur für aktuelle PowerShell-Sitzung)
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process

# Oder systemweit temporär (als Administrator)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**Wichtig:** Der Parameter `-ExecutionPolicy Bypass` in den Registry-Einträgen umgeht die Execution Policy nur für diese eine Befehlsausführung, ohne die Systemeinstellungen dauerhaft zu ändern.

## Sicherheitshinweis:

Diese Registry-Einträge führen PowerShell-Befehle aus. Der Code ist harmlos und führt nur Browser-Aufrufe durch, aber seien Sie bei Registry-Änderungen grundsätzlich vorsichtig.



