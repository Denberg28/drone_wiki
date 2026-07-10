# ArduPilot SITL Setup Guide for Windows 11 using WSL2 Ubuntu

This guide documents the installation and basic use of **ArduPilot SITL** on Windows using **WSL2 Ubuntu**, with MAVProxy command practice, Guided mode movement, square maneuver testing, Mission Planner click-to-move, and simulated battery settings.

---

## 1. System Overview

Recommended setup:

```text
Windows 11
├── PowerShell
├── Mission Planner
└── WSL2 Ubuntu
    ├── ArduPilot source code
    ├── ArduCopter SITL
    └── MAVProxy
```

Use:

- **PowerShell** to open WSL.
- **Ubuntu/WSL shell** for Linux commands such as `cd`, `git`, and `sim_vehicle.py`.
- **MAVProxy prompt** for drone commands such as `mode guided`, `arm throttle`, and `takeoff 20`.

---

## 2. Install WSL

Open **PowerShell as Administrator**.

```powershell
wsl --install
```

Restart the computer if required.

---

## 3. Open Ubuntu from PowerShell

Run inside PowerShell:

```powershell
wsl
```

If WSL opens correctly, the prompt should look similar to this:

```bash
neko@Ranger:~$
```

---

## 4. Check Installed WSL Distributions

From PowerShell:

```powershell
wsl -l -v
```

Expected example:

```text
NAME      STATE           VERSION
Ubuntu    Running         2
```

If Ubuntu already exists, do not reinstall it. Just run:

```powershell
wsl
```

---

## 5. Update Ubuntu

Inside Ubuntu/WSL:

```bash
sudo apt update
sudo apt upgrade -y
```

---

## 6. Install Basic Packages

Inside Ubuntu/WSL:

```bash
sudo apt install git python3 python3-pip python3-dev python3-setuptools python3-wheel -y
```

---

## 7. Clone ArduPilot

Inside Ubuntu/WSL:

```bash
cd ~
git clone --recurse-submodules https://github.com/ArduPilot/ardupilot.git
cd ardupilot
```

If the `ardupilot` folder already exists, use the existing folder instead of cloning again:

```bash
cd ~/ardupilot
git pull
git submodule update --init --recursive
```

Recommended location:

```text
/home/<username>/ardupilot
```

Avoid installing it inside:

```text
/mnt/c/Users/...
```

The Linux filesystem is better for ArduPilot build and SITL performance.

---

## 8. Install ArduPilot Prerequisites

Inside the `~/ardupilot` folder:

```bash
Tools/environment_install/install-prereqs-ubuntu.sh -y
```

Reload Ubuntu profile:

```bash
. ~/.profile
```

Close WSL:

```bash
exit
```

---

## 9. Restart WSL and Start SITL

From PowerShell:

```powershell
wsl
```

Inside Ubuntu/WSL:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map --console
```

Alternative if the floating console does not accept typing:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

In many WSL setups, the floating MAVProxy console may not accept keyboard input. In that case, type commands in the main PowerShell/WSL terminal where the MAVProxy prompt appears.

---

## 10. MAVProxy Prompt vs Ubuntu Shell

### Ubuntu/Linux shell

Example:

```bash
neko@Ranger:~/ardupilot/ArduCopter$
```

Use this for:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
git pull
```

### MAVProxy prompt

Example:

```text
STABILIZE>
GUIDED>
LOITER>
RTL>
LAND>
```

Use this for:

```text
mode guided
arm throttle
takeoff 20
mode land
```

Do **not** type Linux commands inside MAVProxy.

Wrong:

```text
GUIDED> cd ~/ardupilot/ArduCopter
```

Correct:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

---

## 11. Basic SITL Flight Test

When SITL is running, the MAVProxy prompt usually starts in:

```text
STABILIZE>
```

Change to Guided mode:

```text
mode guided
```

Arm the simulated drone:

```text
arm throttle
```

Take off to 20 meters:

```text
takeoff 20
```

Expected response:

```text
Take Off started
Got COMMAND_ACK: NAV_TAKEOFF: ACCEPTED
```

Land:

```text
mode land
```

Wait until:

```text
DISARMED
```

---

## 12. Confirm Current Mode

Do not repeatedly send the same command just to confirm.

Standard habit:

```text
Send command once.
Wait 1–3 seconds.
Confirm mode changed.
Then send the next command.
```

Confirm mode using:

- The MAVProxy prompt: `GUIDED>`, `LOITER>`, `RTL>`, `LAND>`
- Status text such as `Mode GUIDED`
- Mission Planner HUD if connected
- MAVProxy command:

```text
status
```

---

## 13. Load MAVProxy Message Module

Before sending `SET_POSITION_TARGET_LOCAL_NED`, load the message module:

```text
module load message
```

If it is already loaded, that is okay.

---

## 14. Guided Square Maneuver

Make sure the vehicle is already in Guided mode, armed, and flying.

```text
mode guided
arm throttle
takeoff 20
```

Wait until altitude is stable.

Load message module:

```text
module load message
```

Send each command one at a time. Wait for the vehicle to reach each corner before sending the next command.

### Forward 50 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 50 0 0 0 0 0 0 0 0 0 0
```

### Right 50 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 50 0 0 0 0 0 0 0 0 0
```

### Backward 50 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 -50 0 0 0 0 0 0 0 0 0 0
```

### Left 50 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 -50 0 0 0 0 0 0 0 0 0
```

After completing the square:

```text
mode land
```

---

## 15. Meaning of the Guided Movement Command

Example:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 -50 0 0 0 0 0 0 0 0 0
```

Meaning:

```text
Move 50 meters left from current position.
Keep the same altitude.
Use vehicle body direction.
```

Important coordinate portion:

```text
x y z
0 -50 0
```

In `BODY_OFFSET_NED` frame:

```text
x = forward / backward
y = right / left
z = down / up
```

Therefore:

```text
50 0 0     = forward 50 m
-50 0 0    = backward 50 m
0 50 0     = right 50 m
0 -50 0    = left 50 m
0 0 -10    = climb 10 m
0 0 10     = descend 10 m
```

Important NED rule:

```text
Negative Z = up
Positive Z = down
```

---

## 16. Custom Guided Movement Examples

### Forward 20 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 20 0 0 0 0 0 0 0 0 0 0
```

### Right 10 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 10 0 0 0 0 0 0 0 0 0
```

### Left 30 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 -30 0 0 0 0 0 0 0 0 0
```

### Forward 50 m and right 50 m diagonally

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 50 50 0 0 0 0 0 0 0 0 0
```

### Climb 10 m while moving forward 30 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 30 0 -10 0 0 0 0 0 0 0 0
```

Reminder:

```text
In NED, negative Z means up and positive Z means down.
```

---

## 17. Mission Planner Click-to-Move Option

You can also click on the Mission Planner map and make the drone move there.

Steps:

1. Start SITL.
2. Connect Mission Planner using UDP.
3. Take off in Guided mode.
4. In Mission Planner, go to **Flight Data**.
5. Right-click on the map where you want the drone to go.
6. Select **Fly To Here**.
7. Enter altitude if asked.
8. Confirm.

The drone should move to the selected map location.

### Scripted Movement vs Click-to-Move

Scripted MAVProxy movement is better for:

```text
Repeatable testing
Algorithm development
Swarm logic
Square maneuvers
Documentation
Automation
```

Mission Planner click-to-move is better for:

```text
Quick manual testing
Visual movement
Simple Guided movement practice
```

---

## 18. Simulated Unlimited Battery Settings

For SITL practice only, you can disable simulated battery failsafe and increase capacity.

At the MAVProxy prompt:

```text
param set BATT_FS_LOW_ACT 0
param set BATT_FS_CRT_ACT 0
param set FS_BATT_ENABLE 0
param set BATT_CAPACITY 99999
param set BATT_LOW_VOLT 0
param set BATT_CRT_VOLT 0
```

This is useful for long simulation practice.

Do **not** use these settings on a real drone.

---

## 19. Return Battery Settings to Default-Like Values

At the MAVProxy prompt:

```text
param set BATT_FS_LOW_ACT 2
param set BATT_FS_CRT_ACT 1
param set FS_BATT_ENABLE 1
param set BATT_CAPACITY 3300
param set BATT_LOW_VOLT 10.5
param set BATT_CRT_VOLT 10.2
```

Meaning:

```text
BATT_FS_LOW_ACT 2   = RTL on low battery
BATT_FS_CRT_ACT 1   = Land on critical battery
FS_BATT_ENABLE 1    = enable battery failsafe
BATT_CAPACITY 3300  = simulated 3300 mAh pack
```

---

## 20. Increase Simulated Battery Capacity Only

If you only want longer simulated endurance:

```text
param set BATT_CAPACITY 99999
```

---

## 21. Reset SITL Runtime State

To reset the current SITL session:

```text
mode land
```

Wait for:

```text
DISARMED
```

Then exit:

```text
exit
```

Restart SITL:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

---

## 22. Reset SITL Parameters to Default

Runtime restart does not always reset saved parameters. To reset parameters fully, use `--wipe`.

Exit SITL first, then in the Ubuntu shell:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map --wipe
```

Use `--wipe` when you want a clean parameter reset.

---

## 23. Common Errors and Fixes

### Error: `No such file or directory`

Example:

```text
-bash: ../Tools/autotest/sim_vehicle.py: No such file or directory
```

Cause: You are in the wrong folder, often:

```text
/mnt/c/Users/Orangee/Desktop
```

Fix:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

### Error: `Unknown command SET_POSITION_TARGET_LOCAL_NED`

Cause: You forgot the `message` keyword.

Wrong:

```text
SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 50 0 0 0 0 0 0 0 0 0 0
```

Correct:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 50 0 0 0 0 0 0 0 0 0 0
```

Also make sure this was loaded first:

```text
module load message
```

### Floating console does not accept typing

Use the main PowerShell/WSL terminal instead. The real MAVProxy prompt is usually visible there.

### Port already in use

Inside Ubuntu/WSL:

```bash
pkill -f sim_vehicle.py
pkill -f arducopter
pkill -f mavproxy.py
```

Then restart SITL:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

---

## 24. Recommended Practice Sequence

```text
1. Start SITL.
2. Change to Guided mode.
3. Arm throttle.
4. Take off to 20 m.
5. Load message module.
6. Move forward 20 m.
7. Move right 20 m.
8. Move backward 20 m.
9. Move left 20 m.
10. Land.
11. Exit SITL.
12. Restart cleanly if needed.
```

Begin with 20 m movements before attempting 50 m square maneuvers.

---

## 25. Quick Command Reference

### Start SITL

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

### Start SITL with console

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map --console
```

### Start SITL with parameter wipe

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map --wipe
```

### Basic flight

```text
mode guided
arm throttle
takeoff 20
mode land
```

### Load message module

```text
module load message
```

### Forward 20 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 20 0 0 0 0 0 0 0 0 0 0
```

### Land and exit

```text
mode land
exit
```

---

## 26. Notes for Real Drone Use

The battery failsafe disabling commands are for SITL only:

```text
param set BATT_FS_LOW_ACT 0
param set BATT_FS_CRT_ACT 0
param set FS_BATT_ENABLE 0
```

Do **not** disable battery failsafes on real aircraft unless there is a controlled engineering reason and a separate safety process.

For real drones, keep battery failsafe enabled and tested.

---

## 27. Next Step

After single-drone SITL works reliably, proceed to:

```text
2-drone SITL
Leader + follower
Fixed offset
Simple waypoint mission
```

This is the recommended first step toward swarm simulation and custom swarm application development.
