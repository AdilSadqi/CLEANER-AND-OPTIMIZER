@echo off
:: =============================================
::     Junk and Temp Files Cleaner and Optimizer
::     Created by asadqi.com
:: =============================================
title Junk and Temp Files Cleaner and Optimizer
color 1F
mode con: cols=60 lines=25

:: Check & Request Admin Privileges
net session >nul 2>&1
if %errorLevel% neq 0 (
    echo Requesting Administrator Privileges...
    powershell -Command "Start-Process '%~f0' -Verb RunAs"
    exit
)

:menu
cls
echo ==========================================
echo     TEMP AND JUNK FILES CLEANER AND OPTIMIZER
echo         Created by asadqi.com
echo ==========================================
echo.
echo  1) Clear Temporary Files
echo  2) Clear Windows Temp
echo  3) Clear Prefetch Files
echo  4) Empty Recycle Bin
echo  5) Disable Unnecessary Startup Apps
echo  6) Defragment Hard Drive (HDD Only)
echo  7) Flush DNS Cache
echo  8) Full Cleanup and Optimization
echo  9) Exit
echo.

set /p choice= Enter your choice (1-9): 

if "%choice%"=="1" goto clear_temp
if "%choice%"=="2" goto clear_win_temp
if "%choice%"=="3" goto clear_prefetch
if "%choice%"=="4" goto clear_bin
if "%choice%"=="5" goto disable_startup
if "%choice%"=="6" goto defrag_drive
if "%choice%"=="7" goto flush_dns
if "%choice%"=="8" goto full_cleanup
if "%choice%"=="9" exit

echo Invalid option! Please choose 1-9.
pause
goto menu

:clear_temp
cls
echo Cleaning User Temp Files...
del /s /f /q "%temp%\*.*" >nul 2>&1
rd /s /q "%temp%" >nul 2>&1
mkdir "%temp%" >nul 2>&1
echo Done!
pause
goto menu

:clear_win_temp
cls
echo Cleaning Windows Temp Files...
del /s /f /q "C:\Windows\Temp\*.*" >nul 2>&1
rd /s /q "C:\Windows\Temp" >nul 2>&1
mkdir "C:\Windows\Temp" >nul 2>&1
echo Done!
pause
goto menu

:clear_prefetch
cls
echo Cleaning Prefetch Files...
del /s /f /q "C:\Windows\Prefetch\*.*" >nul 2>&1
echo Done!
pause
goto menu

:clear_bin
cls
echo Emptying Recycle Bin...
rd /s /q C:\$Recycle.Bin >nul 2>&1
echo Done!
pause
goto menu

:disable_startup
cls
echo Disabling Unnecessary Startup Apps...
wmic startup get caption,command > startup_backup.txt
echo Disabling common bloatware startup apps...
wmic startup where "Caption like '%%Adobe%%' or Caption like '%%Updater%%'" call disable >nul 2>&1
echo Done!
pause
goto menu

:defrag_drive
cls
echo Defragmenting Hard Drive...
defrag C: /O
echo Done!
pause
goto menu

:flush_dns
cls
echo Flushing DNS Cache...
ipconfig /flushdns
echo Done!
pause
goto menu

:full_cleanup
cls
echo Performing Full Cleanup and Optimization...
call :clear_temp
call :clear_win_temp
call :clear_prefetch
call :clear_bin
call :disable_startup
call :defrag_drive
call :flush_dns
echo All done!
pause
goto menu
