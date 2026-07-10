# ArduPilot SITL Flight Test Guides

This guide provides structured **Beginner**, **Intermediate**, and **Advanced** ArduPilot SITL flight test exercises for Windows + WSL2 Ubuntu users running ArduCopter SITL through MAVProxy.

The goal is to build skill progressively:

```text
Beginner      = basic takeoff, mode changes, landing, RTL
Intermediate = Guided movement, square maneuvers, Mission Planner click-to-move
Advanced      = multi-drone SITL, leader-follower concepts, failsafe testing
```

---

# 1. Required Setup

Before starting these flight tests, make sure SITL is already installed.

Start SITL:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

Optional with console:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map --console
```

If the floating console does not accept typing, type commands in the main PowerShell/WSL terminal.

---

# 2. Prompt Reminder

## Ubuntu/Linux Shell

Example:

```bash
neko@Ranger:~/ardupilot/ArduCopter$
```

Use this for:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

## MAVProxy Prompt

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

---

# 3. Safety Rules for SITL Practice

Even though SITL is simulation, practice real drone discipline:

```text
1. Send one command at a time.
2. Wait for mode confirmation.
3. Do not spam commands.
4. Always know how to LAND and RTL.
5. Use 20 m movement tests before 50 m movement tests.
6. Reset SITL if behavior becomes confusing.
```

Emergency commands:

```text
mode land
```

```text
mode rtl
```

Exit SITL:

```text
exit
```

or press:

```text
Ctrl + C
```

---

# BEGINNER FLIGHT TEST GUIDE

## Beginner Goal

Learn the basic SITL workflow:

```text
Start SITL
Change flight mode
Arm
Takeoff
Loiter
RTL
Land
Exit
```

---

## Beginner Test 1 — Start SITL

From Ubuntu/WSL shell:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

Wait until MAVProxy shows:

```text
STABILIZE>
```

Success condition:

```text
SITL starts without errors.
Map opens.
MAVProxy prompt appears.
```

---

## Beginner Test 2 — Change to GUIDED Mode

At the MAVProxy prompt:

```text
mode guided
```

Expected confirmation:

```text
GUIDED>
```

or:

```text
Mode GUIDED
```

Success condition:

```text
Vehicle is in GUIDED mode.
```

---

## Beginner Test 3 — Arm Motors

At the MAVProxy prompt:

```text
arm throttle
```

Expected confirmation:

```text
ARMED
```

or:

```text
AP: Arming motors
```

Success condition:

```text
Vehicle arms successfully.
```

---

## Beginner Test 4 — Takeoff to 20 m

```text
takeoff 20
```

Expected confirmation:

```text
Take Off started
Got COMMAND_ACK: NAV_TAKEOFF: ACCEPTED
```

Wait until altitude is near 20 m.

To check status:

```text
status
```

Useful altitude reference:

```text
GLOBAL_POSITION_INT relative_alt
```

Altitude interpretation:

```text
relative_alt : 20000 = 20 m
relative_alt : 30000 = 30 m
```

Success condition:

```text
Vehicle climbs and stabilizes around target altitude.
```

---

## Beginner Test 5 — Change to LOITER

```text
mode loiter
```

Expected confirmation:

```text
LOITER>
```

or:

```text
Mode LOITER
```

Success condition:

```text
Vehicle holds position.
```

---

## Beginner Test 6 — Return to Launch

```text
mode rtl
```

Expected confirmation:

```text
RTL>
```

or:

```text
Mode RTL
```

Success condition:

```text
Vehicle returns toward home.
```

---

## Beginner Test 7 — Land

If RTL does not land automatically or you want to force landing:

```text
mode land
```

Wait until:

```text
DISARMED
```

Success condition:

```text
Vehicle lands and disarms.
```

---

## Beginner Test 8 — Exit SITL

```text
exit
```

or press:

```text
Ctrl + C
```

Success condition:

```text
SITL closes and returns to Ubuntu shell.
```

---

## Beginner Complete Sequence

```text
mode guided
arm throttle
takeoff 20
mode loiter
mode rtl
mode land
exit
```

Do not paste everything at once. Enter one command, wait for confirmation, then continue.

---

# INTERMEDIATE FLIGHT TEST GUIDE

## Intermediate Goal

Learn Guided movement using MAVProxy message commands and Mission Planner click-to-move.

Intermediate tests include:

```text
Guided forward movement
Square maneuver
Diagonal movement
Altitude change
Mission Planner Fly To Here
Battery simulation settings
```

---

## Intermediate Test 1 — Start and Takeoff

Start SITL:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map
```

At MAVProxy:

```text
mode guided
arm throttle
takeoff 20
```

Wait until altitude is near 20 m.

---

## Intermediate Test 2 — Load Message Module

At MAVProxy:

```text
module load message
```

This is required before using:

```text
message SET_POSITION_TARGET_LOCAL_NED ...
```

If it says already loaded, that is okay.

---

## Intermediate Test 3 — Move Forward 20 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 20 0 0 0 0 0 0 0 0 0 0
```

Success condition:

```text
Vehicle moves forward relative to its current heading.
```

---

## Intermediate Test 4 — Move Right 20 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 20 0 0 0 0 0 0 0 0 0
```

Success condition:

```text
Vehicle moves right relative to its current heading.
```

---

## Intermediate Test 5 — Move Backward 20 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 -20 0 0 0 0 0 0 0 0 0 0
```

Success condition:

```text
Vehicle moves backward.
```

---

## Intermediate Test 6 — Move Left 20 m

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 -20 0 0 0 0 0 0 0 0 0
```

Success condition:

```text
Vehicle moves left and returns near the original square starting area.
```

---

## Intermediate Test 7 — 20 m Square Maneuver

Use this first before attempting a 50 m square.

```text
module load message
```

Forward 20 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 20 0 0 0 0 0 0 0 0 0 0
```

Right 20 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 20 0 0 0 0 0 0 0 0 0
```

Backward 20 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 -20 0 0 0 0 0 0 0 0 0 0
```

Left 20 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 -20 0 0 0 0 0 0 0 0 0
```

Land:

```text
mode land
```

---

## Intermediate Test 8 — 50 m Square Maneuver

Only do this after the 20 m square works.

Forward 50 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 50 0 0 0 0 0 0 0 0 0 0
```

Right 50 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 50 0 0 0 0 0 0 0 0 0
```

Backward 50 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 -50 0 0 0 0 0 0 0 0 0 0
```

Left 50 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 0 -50 0 0 0 0 0 0 0 0 0
```

---

## Intermediate Test 9 — Diagonal Movement

Forward 50 m and right 50 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 50 50 0 0 0 0 0 0 0 0 0
```

Success condition:

```text
Vehicle moves diagonally forward-right.
```

---

## Intermediate Test 10 — Climb While Moving Forward

Climb 10 m while moving forward 30 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 30 0 -10 0 0 0 0 0 0 0 0
```

Important:

```text
Negative Z = up
Positive Z = down
```

Success condition:

```text
Vehicle moves forward while increasing altitude.
```

---

## Intermediate Command Meaning

Command format:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 x y z 0 0 0 0 0 0 0 0
```

In `BODY_OFFSET_NED` frame:

```text
x = forward / backward
y = right / left
z = down / up
```

Examples:

```text
50 0 0     = forward 50 m
-50 0 0    = backward 50 m
0 50 0     = right 50 m
0 -50 0    = left 50 m
0 0 -10    = climb 10 m
0 0 10     = descend 10 m
```

---

## Intermediate Test 11 — Mission Planner Click-to-Move

Steps:

```text
1. Start SITL.
2. Connect Mission Planner using UDP.
3. Take off in GUIDED mode.
4. Go to Mission Planner > Flight Data.
5. Right-click on the map.
6. Select Fly To Here.
7. Enter altitude if requested.
8. Confirm.
```

Success condition:

```text
Vehicle moves to the selected point on the map.
```

Use Mission Planner click-to-move for quick visual testing.

Use MAVProxy message commands for repeatable scripted tests.

---

## Intermediate Test 12 — Simulated Unlimited Battery

For SITL only:

```text
param set BATT_FS_LOW_ACT 0
param set BATT_FS_CRT_ACT 0
param set FS_BATT_ENABLE 0
param set BATT_CAPACITY 99999
param set BATT_LOW_VOLT 0
param set BATT_CRT_VOLT 0
```

Do not use these settings on a real drone.

---

## Intermediate Test 13 — Return Battery Settings to Default-Like Values

```text
param set BATT_FS_LOW_ACT 2
param set BATT_FS_CRT_ACT 1
param set FS_BATT_ENABLE 1
param set BATT_CAPACITY 3300
param set BATT_LOW_VOLT 10.5
param set BATT_CRT_VOLT 10.2
```

---

## Intermediate Test 14 — Reset SITL Parameters

Exit SITL, then run from Ubuntu shell:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map --wipe
```

Use `--wipe` when you want a clean parameter reset.

---

# ADVANCED FLIGHT TEST GUIDE

## Advanced Goal

Prepare for swarm simulation and custom swarm application development.

Advanced tests include:

```text
Two-drone SITL
Vehicle selection
All-vehicle commands
Leader and follower setup
Fixed offset concept
Failsafe behavior
Mission repeatability
```

---

## Advanced Test 1 — Start Two SITL Drones

From Ubuntu shell:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --count 2 --auto-sysid --auto-offset-line 10,0 --map
```

Meaning:

```text
--count 2               start two simulated drones
--auto-sysid            assign unique MAV_SYSID values
--auto-offset-line 10,0 place drones 10 m apart
```

Success condition:

```text
Two SITL vehicles start successfully.
Map shows two vehicles.
MAVProxy can switch between vehicles.
```

---

## Advanced Test 2 — Select Vehicle 1

At MAVProxy:

```text
vehicle 1
```

Then:

```text
mode guided
arm throttle
takeoff 20
```

Success condition:

```text
Vehicle 1 takes off.
```

---

## Advanced Test 3 — Select Vehicle 2

```text
vehicle 2
```

Then:

```text
mode guided
arm throttle
takeoff 20
```

Success condition:

```text
Vehicle 2 takes off.
```

---

## Advanced Test 4 — Command All Vehicles to RTL

```text
alllinks mode rtl
```

Success condition:

```text
Both vehicles return to launch.
```

---

## Advanced Test 5 — Command All Vehicles to Land

```text
alllinks mode land
```

Success condition:

```text
Both vehicles land and disarm.
```

---

## Advanced Test 6 — Two-Drone Basic Pattern

Start two drones:

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --count 2 --auto-sysid --auto-offset-line 10,0 --map
```

Vehicle 1:

```text
vehicle 1
mode guided
arm throttle
takeoff 20
```

Vehicle 2:

```text
vehicle 2
mode guided
arm throttle
takeoff 20
```

Return both:

```text
alllinks mode rtl
```

Land both if needed:

```text
alllinks mode land
```

---

## Advanced Test 7 — Leader-Follower Concept

For early swarm simulation:

```text
Vehicle 1 = Leader
Vehicle 2 = Follower
```

Basic concept:

```text
Leader follows mission or Guided movement.
Follower maintains offset from leader.
```

Example offset:

```text
Follower target = Leader position + fixed offset
```

For a simple formation:

```text
Leader:    center/front
Follower:  10 m behind or 10 m side offset
```

Recommended starting offset:

```text
10 m minimum
```

Do not start with close offsets during early testing.

---

## Advanced Test 8 — Manual Leader Movement

Select Vehicle 1:

```text
vehicle 1
mode guided
```

Load message module:

```text
module load message
```

Move leader forward 20 m:

```text
message SET_POSITION_TARGET_LOCAL_NED 0 0 0 9 3576 20 0 0 0 0 0 0 0 0 0 0
```

Observe Vehicle 1 movement.

Vehicle 2 will not automatically follow yet unless you create follow logic or use a supported follow/swarm setup.

This test proves the leader can be commanded separately.

---

## Advanced Test 9 — Multi-Vehicle Emergency Practice

While both vehicles are flying:

```text
alllinks mode loiter
```

Then:

```text
alllinks mode rtl
```

Then:

```text
alllinks mode land
```

Success condition:

```text
You can command all vehicles safely.
```

---

## Advanced Test 10 — Simulated Failsafe Practice

Practice the correct reaction sequence:

```text
1. Vehicle behaves unexpectedly.
2. Send mode loiter.
3. If still unsafe, send mode rtl.
4. If immediate landing is needed, send mode land.
```

For single vehicle:

```text
mode loiter
mode rtl
mode land
```

For all vehicles:

```text
alllinks mode loiter
alllinks mode rtl
alllinks mode land
```

---

## Advanced Test 11 — Mission Planner with Two Vehicles

Connect Mission Planner using UDP after starting two-drone SITL.

Recommended observation tasks:

```text
1. Confirm both vehicles appear.
2. Confirm each vehicle has a unique SYSID.
3. Observe altitude and mode.
4. Test RTL and LAND from MAVProxy.
5. Use Mission Planner mainly for monitoring.
```

For early swarm work, use Mission Planner as a monitor, not the main swarm brain.

---

## Advanced Test 12 — Prepare for Custom Swarm App

After two-drone SITL is stable, the next development step is a custom app using:

```text
Python
MAVSDK
MAVLink
MAVProxy modules
```

Recommended architecture:

```text
Custom Swarm App
        |
MAVLink / UDP
        |
ArduPilot SITL drones
        |
Mission Planner for monitoring
```

Basic swarm logic:

```text
1. Connect to all drones.
2. Identify each MAV_SYSID.
3. Assign leader and follower.
4. Read leader position.
5. Compute follower target position.
6. Send follower Guided movement command.
7. Monitor battery, GPS, mode, and failsafe state.
8. Send RTL/LAND if safety condition is triggered.
```

---

# 4. Suggested Learning Path

## Phase 1 — Beginner

```text
Single drone
Takeoff
Loiter
RTL
Land
Status check
```

## Phase 2 — Intermediate

```text
Guided movement
20 m square
50 m square
Mission Planner Fly To Here
Battery simulation
Parameter reset
```

## Phase 3 — Advanced

```text
Two-drone SITL
Vehicle switching
All-vehicle commands
Leader/follower concept
Failsafe practice
Custom swarm app preparation
```

---

# 5. Quick Reference Tables

## Flight Mode Commands

| Command | Meaning |
|---|---|
| `mode guided` | Allows guided commands and takeoff |
| `mode loiter` | Holds position |
| `mode rtl` | Return to launch |
| `mode land` | Land immediately |

## Basic Flight Commands

| Command | Meaning |
|---|---|
| `arm throttle` | Arms the vehicle |
| `takeoff 20` | Take off to 20 m |
| `status` | Show vehicle status |
| `exit` | Exit MAVProxy/SITL |

## Guided Movement Examples

| Movement | Command Coordinate |
|---|---|
| Forward 50 m | `50 0 0` |
| Backward 50 m | `-50 0 0` |
| Right 50 m | `0 50 0` |
| Left 50 m | `0 -50 0` |
| Climb 10 m | `0 0 -10` |
| Descend 10 m | `0 0 10` |

## Multi-Drone Commands

| Command | Meaning |
|---|---|
| `vehicle 1` | Select vehicle 1 |
| `vehicle 2` | Select vehicle 2 |
| `alllinks mode rtl` | Command all vehicles to RTL |
| `alllinks mode land` | Command all vehicles to land |

---

# 6. Recommended Documentation Checklist

For each test, record:

```text
Date:
SITL command used:
Vehicle count:
Mode sequence:
Altitude:
Movement distance:
Observed behavior:
Errors:
Fix applied:
Result:
```

Example:

```text
Date: 2026-07-10
Test: 20 m square maneuver
Vehicle count: 1
Mode: GUIDED
Altitude: 20 m
Movement: 20 m forward, right, backward, left
Result: Successful
Notes: Waited 5 seconds between commands
```

---

# 7. Final Recommendation

Do not rush into swarm algorithms immediately.

Follow this order:

```text
1. Beginner single-drone flight control
2. Intermediate Guided movement
3. Advanced two-drone SITL
4. Leader-follower simulation
5. Custom Python/MAVSDK swarm app
6. Physical two-drone testing
```

This creates a safer and more reliable research workflow for drone swarm development.
