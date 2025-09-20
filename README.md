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
