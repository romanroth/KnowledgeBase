# NSIS

Used to make Windows installers.

- **Download:** [https://nsis.sourceforge.io/Download](https://nsis.sourceforge.io/Download)
- **Documentation:** [https://nsis.sourceforge.io/Docs/](https://nsis.sourceforge.io/Docs/)

Create a `.nsi` script and right click on it to run the setup.

## Example

Comments are done with a `;`.

**Directory Structure**
- `Application/`
- `Redist/`
- `Runtime/`
- `Application.ico`
- `Application.nsi`
- `license.txt`

```
;Request application privileges for Windows Vista
RequestExecutionLevel admin

;--------------------------------
;Includes

!include "MUI2.nsh" ;ModernUI
!include "WordFunc.nsh"

;--------------------------------
;General

;Name and file
Name "Application"
OutFile "ApplicationSetup.exe"
Unicode True

;Default installation folder
InstallDir "$PROGRAMFILES\Application"

;Get installation folder from registry if available
InstallDirRegKey HKCU "Software\Application" ""  

;--------------------------------
;Interface Settings

!define MUI_ABORTWARNING
!define MUI_ICON "Application.ico"
!define MUI_UNICON "Application.ico"

!define MUI_FINISHPAGE_TEXT_REBOOTNOW "Reboot Now (recommended)"
!define MUI_FINISHPAGE_TEXT_REBOOTLATER "Reboot Later"
!define MUI_FINISHPAGE_REBOOTNOW_DEFAULT

;Only for Debugging (?)
!define MUI_FINISHPAGE_NOAUTOCLOSE true
!define MUI_UNFINISHPAGE_NOAUTOCLOSE true

;--------------------------------
;Pages

!insertmacro MUI_PAGE_WELCOME
!insertmacro MUI_PAGE_LICENSE "license.txt"
!insertmacro MUI_PAGE_DIRECTORY
!insertmacro MUI_PAGE_INSTFILES
!insertmacro MUI_PAGE_FINISH

!insertmacro MUI_UNPAGE_WELCOME
!insertmacro MUI_UNPAGE_CONFIRM
!insertmacro MUI_UNPAGE_INSTFILES

;--------------------------------
;Macros

!insertmacro MUI_LANGUAGE "English"

ShowInstDetails hide

;--------------------------------
;Functions

Function CreateDesktopShortCut
  CreateShortcut "$DESKTOP\Application.lnk" "$INSTDIR\Application\Application.exe" "" "$INSTDIR\Application\Application.ico" 0
FunctionEnd

;--------------------------------
;Installer Sections

Section "Application"
  SetRebootFlag true  

  SetDetailsPrint textonly
  DetailPrint "Installing Application..."
  SetDetailsPrint listonly

  ;Clear Installation Dir first to overwrite an old install
  RMDir /r "$INSTDIR"

  ;Install Application
  SetOutPath "$INSTDIR\Application"
  File "Application.ico"
  File /nonfatal /a /r "Application\"

  SetDetailsPrint textonly
  DetailPrint "Installing Runtime..."
  SetDetailsPrint listonly
  SetOutPath "$INSTDIR\Runtime"
  File /nonfatal /a /r "Runtime\"


  ;Install Prerequisits
  SetOutPath "$INSTDIR\Redist"
  File /nonfatal /a /r "Redist\"
  SetDetailsPrint textonly
  DetailPrint "Installing Visual C++ Redistributables..."
  SetDetailsPrint listonly
  ExecWait '"$INSTDIR\Redist\vcredist_vc2010_x86.exe" /Q'
  ExecWait '"$INSTDIR\Redist\vcredist_vc2013_x86.exe" /Q'
  ExecWait '"$INSTDIR\Redist\vcredist_vc2015_x86.exe" /install /silent /norestart'

  RMDir /r /REBOOTOK "$INSTDIR\Redist\"


  SetDetailsPrint textonly
  DetailPrint "Creating Desktop Shortcut..."
  SetDetailsPrint listonly
  call CreateDesktopShortCut

  ;Store installation folder
  WriteRegStr HKLM "Software\Application" "" $INSTDIR
  WriteRegStr HKLM "Software\Microsoft\Windows\CurrentVersion\Uninstall\Application" \
                 "DisplayName" "Application"
  WriteRegStr HKLM "Software\Microsoft\Windows\CurrentVersion\Uninstall\Application" \
                 "UninstallString" "$INSTDIR\Uninstall.exe"
  WriteRegStr HKLM "Software\Microsoft\Windows\CurrentVersion\Uninstall\Application" \
                 "DisplayIcon" "$INSTDIR\Application\Application.ico"

  ;Write Uninstaller
  SetDetailsPrint textonly
  DetailPrint "Creating Uninstaller..."
  SetDetailsPrint listonly
  WriteUninstaller "$INSTDIR\Uninstall.exe"
  SetDetailsPrint textonly

SectionEnd


;--------------------------------
;Uninstaller Section

Section "Uninstall"

  SetDetailsPrint textonly
  DetailPrint "Deleting Shortcut..."
  SetDetailsPrint listonly
  Delete "$DESKTOP\Application.lnk"

  SetDetailsPrint textonly
  DetailPrint "Deleting Application..."
  SetDetailsPrint listonly
  RMDir /r /REBOOTOK "$INSTDIR\Application"

  SetDetailsPrint textonly
  DetailPrint "Deleting Runtime..."
  SetDetailsPrint listonly
  RMDir /r /REBOOTOK "$INSTDIR\Runtime"

  Delete "$INSTDIR\Uninstall.exe"

  RMDir "$INSTDIR"

  DeleteRegKey /ifempty HKLM "Software\Application"
  DeleteRegKey HKLM "Software\Microsoft\Windows\CurrentVersion\Uninstall\Application"
  SetDetailsPrint textonly

SectionEnd
```
