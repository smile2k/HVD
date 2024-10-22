
# Step 1: Fix content in 2 files
- File 1: Make_Installer_Manually - Repeat.bat
```c
:: Get current date : 20240730
set "currentdate=%datetime:~0,4%%datetime:~4,2%%datetime:~6,2%"

:: Get current time : 140315
set "currenttime=%time:~0,2%%time:~3,2%%time:~6,2%"

:: Combine date & time:
set curDateTime=%currentdate%_%currenttime%
:: Set 'Params'
set version_info=3.0.0.0
set revision=v3.0.0.0
set OutputDirPath=.\
set OutputFileName=KY_RepeatabilityTool_%revision%.exe

:: Run 'NSIS Script'
CALL "C:\Program Files (x86)\NSIS\makensisw.exe" ".\RepeatabilityTool_3.0.0.0_install_x64_2024.04.26.nsi"
```


- File 2: RepeatabilityTool_3.0.0.0_install_x64_2024.04.26.nsi

# Step 2: 
- 



