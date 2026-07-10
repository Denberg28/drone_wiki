# Beginner-Friendly Betaflight Drone Setup Guide

> A complete beginner guide for setting up your first Betaflight FPV drone safely.

---

## ⚠️ READ THIS FIRST — SAFETY WARNING

Building and configuring an FPV drone can be dangerous if done carelessly.

A drone can:

- Cut skin badly with spinning propellers
- Burn electronics if wiring is wrong
- Catch fire if the battery is damaged or shorted
- Fly away if failsafe is not configured
- Injure people, animals, or damage property

### 🚨 Golden Rule

> **REMOVE ALL PROPELLERS whenever the drone is connected to USB, battery, Betaflight Configurator, BLHeli, or while doing motor tests.**

Never trust that the motors will not spin.

---

## Table of Contents

1. [Who This Guide Is For](#who-this-guide-is-for)
2. [Required Parts](#required-parts)
3. [Required Tools](#required-tools)
4. [Basic Drone Parts Explained](#basic-drone-parts-explained)
5. [Before You Start](#before-you-start)
6. [Wiring Safety Checklist](#wiring-safety-checklist)
7. [Install Betaflight Configurator](#install-betaflight-configurator)
8. [Connect the Flight Controller](#connect-the-flight-controller)
9. [Backup Before Changing Anything](#backup-before-changing-anything)
10. [Firmware Flashing](#firmware-flashing)
11. [Board Orientation](#board-orientation)
12. [Receiver Setup](#receiver-setup)
13. [Motor Setup](#motor-setup)
14. [ESC Setup](#esc-setup)
15. [Modes Setup](#modes-setup)
16. [Failsafe Setup](#failsafe-setup)
17. [OSD Setup](#osd-setup)
18. [VTX Setup](#vtx-setup)
19. [Battery and Voltage Setup](#battery-and-voltage-setup)
20. [First Arm Test](#first-arm-test)
21. [First Hover Test](#first-hover-test)
22. [Beginner PID and Filter Advice](#beginner-pid-and-filter-advice)
23. [Pre-Flight Checklist](#pre-flight-checklist)
24. [Common Problems and Fixes](#common-problems-and-fixes)
25. [Beginner Safety Rules](#beginner-safety-rules)
26. [Final Notes](#final-notes)

---

## Who This Guide Is For

This guide is for beginners setting up their first Betaflight drone, especially small FPV quadcopters such as:

- 3-inch FPV drones
- 5-inch freestyle drones
- Cinewhoops
- Toothpick drones
- DIY racing/freestyle quads

This guide assumes you are using:

- A Betaflight-compatible flight controller
- 4-in-1 ESC or individual ESCs
- FPV receiver
- Radio transmitter
- LiPo battery
- Betaflight Configurator

---

## Required Parts

A basic Betaflight FPV drone usually needs:

| Part | Purpose |
|---|---|
| Frame | Holds all drone parts |
| Flight Controller | Main brain of the drone |
| ESC / 4-in-1 ESC | Controls the motors |
| Motors | Produce thrust |
| Propellers | Generate lift |
| Receiver | Receives signal from your radio |
| Radio Transmitter | Controller used by the pilot |
| FPV Camera | Sends video feed |
| VTX | Sends video signal to goggles/monitor |
| Antenna | Transmits video signal |
| LiPo Battery | Powers the drone |
| Buzzer | Helps find the drone after crash |
| Capacitor | Helps reduce voltage spikes |

---

## Required Tools

Recommended tools:

- Soldering iron
- Solder wire
- Flux
- Wire cutter
- Wire stripper
- Multimeter
- Smoke stopper
- Small screwdrivers
- Heat shrink tube
- Zip ties
- Electrical tape
- Computer/laptop
- USB cable for flight controller

---

## Basic Drone Parts Explained

### Flight Controller

The flight controller, or FC, is the brain of the drone. It reads gyro movement and sends commands to the ESCs.

### ESC

The ESC controls motor speed. A 4-in-1 ESC controls all four motors from one board.

### Receiver

The receiver connects your radio transmitter to the drone.

Common receiver systems:

- ELRS
- Crossfire
- FrSky
- FlySky
- Spektrum

### VTX

The video transmitter sends camera video to your FPV goggles or monitor.

### LiPo Battery

The battery provides power. Common drone batteries:

- 1S
- 2S
- 3S
- 4S
- 6S

---

## Before You Start

### ⚠️ Important Beginner Advice

Do not rush.

Most beginner mistakes happen because of:

- Wrong wiring
- Wrong battery voltage
- Solder bridges
- Forgetting to remove propellers
- Wrong motor order
- Wrong propeller direction
- No failsafe setup

---

## Wiring Safety Checklist

Before plugging in a battery:

- [ ] Propellers removed
- [ ] No loose wires
- [ ] No solder bridges
- [ ] Battery positive goes to positive pad
- [ ] Battery negative goes to negative pad
- [ ] Capacitor installed correctly
- [ ] Receiver wiring checked
- [ ] Camera wiring checked
- [ ] VTX wiring checked
- [ ] Motor wires not touching frame
- [ ] Antenna connected to VTX
- [ ] Use smoke stopper for first power-up

---

## 🚨 Battery Warning

LiPo batteries are powerful and dangerous if abused.

Never:

- Short the battery terminals
- Charge unattended
- Charge a damaged battery
- Over-discharge below safe voltage
- Puncture or crush the battery
- Use swollen batteries
- Store fully charged for many days

Recommended:

- Use a proper LiPo charger
- Store batteries at storage voltage
- Use a LiPo safe bag
- Check cell voltage before and after flying

---

## Install Betaflight Configurator

1. Download Betaflight Configurator from the official Betaflight source.
2. Install it on your computer.
3. Connect the flight controller using USB.
4. Open Betaflight Configurator.
5. Select the correct COM port.
6. Click **Connect**.

---

## Alternate, access via Betaflight Web Application

1. Go to https://app.betaflight.com/
2. Guide: https://betaflight.com/docs/wiki/app

---

## Connect the Flight Controller

When you connect the flight controller:

- Do not connect the battery yet
- Remove all propellers
- Use a good USB cable
- Some USB cables are charge-only and will not work

If Betaflight does not detect the board:

- Try another USB cable
- Try another USB port
- Install drivers if needed
- Use boot button only when flashing firmware

---

## Backup Before Changing Anything

Before changing settings:

1. Open Betaflight Configurator.
2. Go to **CLI** tab.
3. Type:

```bash
diff all
```

4. Press Enter.
5. Save the output as:

```text
betaflight_backup_original.txt
```

### Why Backup?

If something goes wrong, you can restore your original settings.

---

## Firmware Flashing

### ⚠️ Warning

Do not flash random firmware. Flashing the wrong target can cause problems.

Before flashing:

- Confirm your exact flight controller model
- Confirm correct Betaflight target
- Save a backup
- Use stable release unless you know why you need newer firmware

Connecting usb cable to flight controller (DFU Mode)
1. Hold flight controlle boot button
2. Connect USB cable
3. Release boot button
4. Confirm at the top right already in DFU mode
   (if unable to enter in DFU mode)
1. Use ImpulseRC driver fixer to enable DFU mode

Basic steps:

1. Go to **Firmware Flasher**.
2. Select your flight controller target.
3. Select firmware version.
4. Radio protocol
   - CRSF (elrs)
   - IBUS (flysky)
5. Enable **Full chip erase** only if needed.
6. Click **Load Firmware Online**.
7. Click **Flash Firmware**.
8. Wait until finished.

After flashing:

- Reconnect the board
- Apply custom defaults if prompted
- Restore only settings that match your build

---

## Board Orientation

Go to **Setup** tab.

1. Calibrate accelerometer (place drone in a level surface)

Move your drone by hand and check the 3D model.

The model should move the same way as the real drone:

- Tilt forward = model tilts forward
- Tilt left = model tilts left
- Rotate clockwise = model rotates clockwise

### If movement is wrong:

Go to:

```text
Configuration > Board and Sensor Alignment
```

Adjust yaw, pitch, or roll alignment until correct.

### ⚠️ Warning

Incorrect board orientation can cause instant flip on takeoff.

---

## Receiver Setup

Go to **Receiver** tab.

Turn on your radio transmitter.

Check if stick movement appears in Betaflight.

Expected channel behavior:

| Stick | Betaflight Channel |
|---|---|
| Roll | Roll |
| Pitch | Pitch |
| Throttle | Throttle |
| Yaw | Yaw |

### Channel Values

Recommended values:

| Stick Position | Value |
|---|---|
| Low | Around 1000 |
| Center | Around 1500 |
| High | Around 2000 |

### If channels are wrong:

Change channel map.

Common channel maps:

```text
AETR1234
TAER1234
```

### Receiver Protocol

Common receiver protocols:

| Receiver Type | Common Protocol |
|---|---|
| ELRS | CRSF |
| Crossfire | CRSF |
| FrSky SBUS | SBUS |
| FlySky iBUS | IBUS |

---

## 🚨 Receiver Warning

Do not continue until receiver movement is correct.

If receiver channels are wrong, the drone may:

- Arm unexpectedly
- Flip on takeoff
- Ignore your controls
- Fail to disarm quickly

---

## Motor Setup

Go to **Motors** tab.

### 🚨 DANGER

Before opening the Motors tab:

> **REMOVE ALL PROPELLERS.**

Betaflight will also warn you.

### Motor Order

Use the motor test sliders to check motor order.

Typical Betaflight motor order:

```text
Motor 1: Rear right
Motor 2: Front right
Motor 3: Rear left
Motor 4: Front left
```

Check Betaflight diagram inside the Motors tab because layouts may vary depending on mixer/version.

### If motor order is wrong:

Use one of these methods:

- Motor remapping in Betaflight
- ESC configurator motor order feature
- Resource remapping in CLI

For beginners, use Betaflight motor reordering tools if available.

---

## Motor Direction

Correct motor direction is critical.

Common prop direction options:

### Props In

```text
Front motors spin inward toward the camera line.
Rear motors spin inward toward the center line.
```

### Props Out

```text
Front motors spin outward away from the camera line.
Rear motors spin outward away from the center line.
```

Many pilots use **props out** for freestyle because it can reduce debris hitting the camera, but either can work if configured correctly.

### ⚠️ Warning

Motor direction must match propeller direction and Betaflight configuration.

Wrong motor direction can cause:

- Instant flip
- No takeoff
- Unstable flight
- Crash

---

## ESC Setup

Depending on your ESC firmware, you may use:

- BLHeli_S
- BLHeli_32
- Bluejay
- AM32

Common ESC tasks:

- Check motor direction
- Reverse motors if needed
- Confirm ESC protocol
- Confirm bidirectional DShot if used

Recommended beginner ESC protocol:

```text
DShot300 or DShot600
```

For many modern builds:

```text
DShot600
```

is safe and reliable.

---

## Props Installation

### 🚨 Very Important

Install propellers only after:

- Receiver works correctly
- Motor order is correct
- Motor direction is correct
- Failsafe is configured
- Arm/disarm switch works
- You are ready for first hover test

### Propeller Rule

The raised side of the propeller usually faces upward.

Propellers must push air downward.

If the drone flips or does not lift:

- Remove props
- Recheck motor direction
- Recheck prop direction
- Recheck motor order

---

## Modes Setup

Go to **Modes** tab.

Recommended beginner modes:

| Mode | Purpose |
|---|---|
| ARM | Arms and disarms the drone |
| ANGLE | Self-level beginner mode |
| HORIZON | Self-level with more freedom |
| BEEPER | Activates buzzer |
| FLIP OVER AFTER CRASH | Turtle mode |
| AIRMODE | Better control at low throttle |

### Recommended Beginner Switch Setup

| Switch | Function |
|---|---|
| Switch 1 | ARM / DISARM |
| Switch 2 | ANGLE / ACRO |
| Switch 3 | BEEPER |
| Switch 4 | TURTLE MODE |

### Beginner Recommendation

Use **ANGLE mode** for your very first hover test.

Use **ACRO mode** only after practicing in simulator.

---

## 🚨 Arm Switch Warning

Use a real physical switch for arming.

Do not use stick arming for your first drone.

You must be able to disarm quickly after a crash.

---

## Failsafe Setup

Failsafe tells the drone what to do when radio signal is lost.

Go to:

```text
Failsafe Tab
```

Recommended beginner setting:

```text
Drop
```

This means the drone stops motors and falls if signal is lost.

### ⚠️ Warning

A drone without proper failsafe can fly away.

Before first flight:

- Arm drone without props
- Turn off transmitter
- Confirm drone disarms/failsafes
- Turn transmitter back on
- Confirm control returns only after safe reset

---

## OSD Setup

OSD means On-Screen Display.

Recommended beginner OSD items:

- Battery voltage
- Average cell voltage
- Timer
- RSSI or LQ
- Warnings
- Flight mode
- Craft name
- Throttle position

Important OSD warning messages:

| Warning | Meaning |
|---|---|
| RXLOSS | Receiver signal lost |
| FAILSAFE | Failsafe activated |
| LOW BATTERY | Battery voltage low |
| RUNAWAY | Drone detected dangerous behavior |
| THROTTLE | Throttle not low |
| MSP | Connected to USB/configurator |

### Reminder: Update font in font manager
---

## VTX Setup

The VTX sends video to your goggles or monitor.

### ⚠️ VTX Warning

Never power a VTX without antenna unless the manual says it is safe.

A VTX without antenna can overheat or burn out.

### Basic VTX Settings

Set:

- Band
- Channel
- Power level
- Pit mode if needed

For bench testing, use low power such as:

```text
25 mW
```

Higher power creates more heat.

---

## Battery and Voltage Setup

Go to **Power & Battery** tab.

Check:

- Voltage scale
- Cell count detection
- Warning voltage
- Minimum voltage

Typical LiPo voltage guide:

| Cell State | Voltage Per Cell |
|---|---|
| Full | 4.20 V |
| Storage | 3.80 V |
| Land Soon | 3.50 V |
| Minimum Under Load | Around 3.3 V |

### Example Battery Voltages

| Battery | Full Voltage | Storage Voltage |
|---|---:|---:|
| 3S | 12.6 V | 11.4 V |
| 4S | 16.8 V | 15.2 V |
| 6S | 25.2 V | 22.8 V |

### ⚠️ Battery Warning

Do not keep flying until the battery is empty.

Land early.

Over-discharging LiPo batteries damages them and can make them unsafe.

---

## First Arm Test

### Before Arm Test

- [ ] Propellers removed
- [ ] Drone on flat surface
- [ ] Battery connected
- [ ] Radio on
- [ ] Betaflight disconnected
- [ ] Arm switch assigned
- [ ] Throttle stick fully low

Arm the drone.

Expected result:

- Motors spin slowly
- No sudden high throttle
- No twitching violently
- Disarm switch stops motors immediately

If something feels wrong:

> Disarm immediately.

---

## First Hover Test

### Location

Use a safe open area.

Avoid:

- People
- Pets
- Cars
- Glass
- Roads
- Power lines
- Crowded places

### First Hover Procedure

1. Fully charge battery.
2. Install propellers correctly.
3. Place drone on level ground.
4. Stand behind the drone.
5. Turn on radio.
6. Plug in battery.
7. Wait for startup tones.
8. Arm.
9. Slowly raise throttle.
10. Hover only 20–50 cm high.
11. Land after a few seconds.
12. Disarm.

### What to Check

During first hover, check:

- Does it lift smoothly?
- Does it drift heavily?
- Does it shake?
- Does it flip?
- Does it respond correctly?
- Does disarm work instantly?

---

## 🚨 If Drone Flips on Takeoff

Immediately disarm.

Remove battery.

Remove propellers.

Check:

- Motor order
- Motor direction
- Propeller direction
- Board orientation
- Mixer type
- Receiver channel mapping

Do not keep trying with props installed.

---

## Beginner PID and Filter Advice

For your first build:

> Do not randomly change PID and filter settings.

Betaflight defaults are usually good enough for first hover.

Only tune after:

- Drone flies correctly
- No mechanical problems
- No loose screws
- No bent props
- No damaged motors
- No noisy gyro issue

Common causes of bad flight:

- Loose flight controller
- Soft frame
- Bent prop
- Damaged motor
- Loose motor screw
- Bad solder joint
- Wrong filtering
- Wrong firmware target

---

## Pre-Flight Checklist

Before every flight:

- [ ] Frame screws tight
- [ ] Motor screws tight
- [ ] Props not cracked
- [ ] Props installed correctly
- [ ] Battery charged
- [ ] Battery strap tight
- [ ] Antenna connected
- [ ] Receiver connected
- [ ] Radio battery charged
- [ ] Goggles/monitor ready
- [ ] Arm switch works
- [ ] Disarm switch works
- [ ] Failsafe tested
- [ ] Flying area clear

---

## Post-Flight Checklist

After flying:

- [ ] Disarm
- [ ] Unplug battery
- [ ] Check battery voltage
- [ ] Let motors cool
- [ ] Check frame damage
- [ ] Check prop damage
- [ ] Check antenna
- [ ] Record any issue
- [ ] Recharge or storage charge battery

---

## Common Problems and Fixes

### Drone Does Not Connect to Betaflight

Possible causes:

- Bad USB cable
- Missing drivers
- Wrong COM port
- Damaged USB port
- Board stuck in bootloader

Try:

- Different USB cable
- Different USB port
- Restart Betaflight Configurator
- Install drivers
- Press boot button only for flashing

---

### Receiver Not Moving in Receiver Tab

Check:

- Receiver powered
- Receiver bound to radio
- Correct UART selected
- Correct serial RX enabled
- Correct receiver protocol selected
- Correct wiring: TX to RX, RX to TX for UART-based receivers

---

### Motors Do Not Spin

Check:

- Battery connected
- ESC powered
- Motor wires soldered
- ESC signal cable connected
- Correct ESC protocol
- Motor idle value
- Arming disabled flags

---

### Drone Will Not Arm

Check the Setup tab or CLI for arming disable flags.

Common arming disable reasons:

| Flag | Meaning |
|---|---|
| MSP | Connected to Betaflight |
| THROTTLE | Throttle not low |
| RXLOSS | No receiver signal |
| FAILSAFE | Failsafe active |
| CLI | In CLI mode |
| NOGYRO | Gyro not detected |
| ACC_CALIB | Accelerometer needs calibration |

In CLI, type:

```bash
status
```

Look for:

```text
Arming disable flags
```

---

### Drone Shakes Badly

Possible causes:

- Bent propeller
- Loose screw
- Damaged motor
- Loose flight controller
- Bad gyro noise
- Bad tune
- Wrong filter settings

Start with mechanical checks before changing software.

---

### Video Has Lines or Noise

Try:

- Add capacitor
- Check ground wiring
- Use proper VTX wiring
- Reduce VTX power while testing
- Keep antenna connected
- Separate video wires from power wires

---

### Drone Flies Away

Possible causes:

- Wrong failsafe
- Bad receiver signal
- Wrong channel mapping
- Wrong GPS rescue setup
- Pilot disorientation

Beginner advice:

> Do not enable advanced rescue features until basic manual control is reliable.

---

## Beginner Safety Rules

Follow these rules:

1. Remove props during setup.
2. Never arm indoors with props.
3. Always test failsafe.
4. Always use an arm switch.
5. Keep fingers away from props.
6. Do not fly near people.
7. Do not fly over roads.
8. Do not fly with damaged batteries.
9. Do not fly beyond your skill level.
10. Practice in a simulator first.

---

## Recommended First Flight Plan

For your first day:

1. Configure Betaflight.
2. Test receiver.
3. Test motor order.
4. Test motor direction.
5. Configure failsafe.
6. Configure modes.
7. Do prop-off arm test.
8. Install props.
9. Do short hover.
10. Land and inspect.

Do not immediately do:

- Full throttle punch-outs
- Long-range flight
- Acro tricks
- GPS rescue testing
- Flying over people
- Flying in windy conditions

---

## Beginner Simulator Recommendation

Before flying acro mode, practice using FPV simulators such as:

- Liftoff
- VelociDrone
- Uncrashed
- DRL Simulator
- Tryp FPV

Practice:

- Hovering
- Turning
- Landing
- Throttle control
- Recovery from mistakes
- Disarming quickly

---

## Suggested Beginner Radio Switch Layout

Example:

```text
SA Switch: ARM / DISARM
SB Switch: ANGLE / ACRO
SC Switch: BEEPER
SD Switch: TURTLE MODE
```

Keep arming on a switch that is hard to hit accidentally.

---

## Simple Setup Checklist

Use this checklist before first flight:

```text
[ ] Props removed during setup
[ ] Betaflight backup saved
[ ] Correct firmware target confirmed
[ ] Board orientation correct
[ ] Receiver channels correct
[ ] Arm switch working
[ ] Flight mode switch working
[ ] Motor order correct
[ ] Motor direction correct
[ ] Props installed correctly
[ ] Failsafe tested
[ ] OSD warnings enabled
[ ] Battery voltage correct
[ ] VTX antenna installed
[ ] First hover location safe
```

---

## Emergency Procedure

If the drone behaves unexpectedly:

1. Disarm immediately.
2. Do not try to save a dangerous takeoff.
3. Wait for props to stop.
4. Unplug battery.
5. Inspect drone.
6. Remove props before troubleshooting.
7. Check Betaflight settings again.

---

## Final Notes

A first Betaflight drone setup can feel confusing, but take it step by step.

The most important beginner priorities are:

- Safety
- Correct wiring
- Correct receiver setup
- Correct motor order
- Correct motor direction
- Correct propeller direction
- Working failsafe
- Reliable disarm switch

Do not rush to fly.

A properly configured beginner drone is much safer, easier to control, and more enjoyable to learn.

---

## Disclaimer

This guide is for educational use only.

Always follow:

- Local drone laws
- School or club safety rules
- Manufacturer instructions
- Battery safety guidelines
- Field safety rules

You are responsible for safe operation of your drone.
