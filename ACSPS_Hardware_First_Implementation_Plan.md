# Automated Cable Specimen Preparation System

## Hardware-First Implementation Plan

## Goal

Build a demo-ready **hardware-integrated automated cable specimen
preparation machine** that converts raw PVC/XLPE/HDPE cable into
test-ready specimens with minimum operator intervention.

The primary system must physically automate:

`FEED → STRAIGHTEN → MEASURE → CLAMP → CUT → STRIP/SHAPE → VERIFY → EJECT`

PLC and industrial hardware are the primary implementation. Software is
limited to machine configuration, monitoring and traceability.

------------------------------------------------------------------------

## 1. Implementation Priorities

1.  Build the mechanical cable-feeding and straightening assembly.
2.  Integrate controlled cable clamping.
3.  Install non-contact cable diameter measurement.
4.  Integrate servo/linear cutting and stripping hardware.
5.  Integrate dumbbell specimen tooling.
6.  Implement PLC-based deterministic sequencing.
7.  Add HMI method selection and machine monitoring.
8.  Add automatic specimen ejection and waste collection.
9.  Implement safety guarding and independent safety circuits.
10. Validate specimen repeatability against the applicable IS methods.
11. Add batch logging and traceability.
12. Optimize cycle time after dimensional repeatability is established.

------------------------------------------------------------------------

## 2. Prototype Architecture

``` text
                 RAW CABLE
                     │
                     ↓
              ┌─────────────┐
              │ FEED ROLLERS│
              └──────┬──────┘
                     ↓
              ┌─────────────┐
              │ STRAIGHTENER│
              └──────┬──────┘
                     ↓
              ┌─────────────┐
              │ DIAMETER    │
              │ SENSOR      │
              └──────┬──────┘
                     ↓
              ┌─────────────┐
              │ CLAMP       │
              └──────┬──────┘
                     ↓
          ┌──────────┴──────────┐
          ↓                     ↓
     CUT / STRIP            DUMBBELL TOOL
          │                     │
          └──────────┬──────────┘
                     ↓
                 PLC CONTROL
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
      VERIFICATION          SAFETY LOGIC
          │
          ↓
      AUTO EJECTION
          │
     ┌────┴────┐
     ↓         ↓
 SPECIMEN    WASTE
```

------------------------------------------------------------------------

## 3. Phase 1 --- Mechanical Prototype

Build a compact machine frame containing:

-   Cable supply/reel interface
-   Motor-driven feed rollers
-   Straightening roller assembly
-   Adjustable cable guides
-   Clamping station
-   Cutting/stripping station
-   Dumbbell cutting station
-   Specimen ejector
-   Waste collection bin
-   Guarded machine enclosure

The prototype should support representative PVC, XLPE and HDPE cable
samples and allow controlled changes in cable diameter so the automatic
adjustment function can be demonstrated.

------------------------------------------------------------------------

## 4. Phase 2 --- Cable Feeding and Straightening

### Feed System

Install:

-   Geared motor or servo motor
-   Feed rollers
-   Adjustable roller pressure
-   Encoder
-   Cable guide rails

Implement:

``` text
Feed Command
     ↓
Motor / Servo
     ↓
Feed Rollers
     ↓
Encoder Feedback
     ↓
Required Cable Position
```

### Straightening System

Use counter-rotating or adjustable roller sets to remove cable curvature
before specimen preparation.

Demonstrate:

-   Consistent cable alignment
-   Reduced curvature
-   Repeatable entry into the cutting station

------------------------------------------------------------------------

## 5. Phase 3 --- Diameter Sensor Integration

Install a non-contact laser/optical diameter sensor before the cutting
station.

Implement:

``` text
Cable
  ↓
Diameter Sensor
  ↓
Measured Diameter
  ↓
PLC
  ↓
Lookup / Setpoint Logic
  ↓
Clamp Force + Blade Depth
```

The first implementation should use deterministic lookup tables rather
than machine learning.

The lookup table can be indexed by:

-   Selected IS test method
-   Measured cable diameter
-   Cable construction/material where required

Exact standard-derived values must be validated against the current
applicable BIS documents.

------------------------------------------------------------------------

## 6. Phase 4 --- Automated Clamping

Install pneumatic or servo-actuated clamps with force feedback.

### Sequence

``` text
Cable Positioned
      ↓
Diameter Confirmed
      ↓
PLC Calculates Clamp Setting
      ↓
Clamp Closes
      ↓
Load Cell Confirms Force
      ↓
Cutting Enabled
```

The machine must prevent cutting if the cable is not securely
positioned.

Clamp-force limits should be established experimentally during
commissioning to avoid deformation of the cable specimen.

------------------------------------------------------------------------

## 7. Phase 5 --- Precision Cutting and Stripping

### Circumferential Cut

Implement:

``` text
Position Cable
     ↓
Clamp
     ↓
Move Cutting Head
     ↓
Set Blade Depth
     ↓
Circumferential Cut
     ↓
Cut Completion Confirmation
```

### Linear Stripping

Implement controlled stripping after the layer has been scored/cut.

``` text
Score
 ↓
Engage Strip Tool
 ↓
Linear Motion
 ↓
Remove Selected Layer
 ↓
Confirm Completion
```

### Dumbbell Cutting

Install the required dumbbell die as a controlled tooling station.

``` text
Specimen Positioned
       ↓
Clamp / Hold
       ↓
Dumbbell Die Engaged
       ↓
Cut Complete
       ↓
Specimen Released
```

The die should be replaceable so that the machine can be configured for
the applicable test specimen profile.

------------------------------------------------------------------------

## 8. Phase 6 --- PLC Automation

Implement deterministic PLC sequences for:

### Feeding

`Start → Feed → Encoder Check → Target Position → Stop`

### Straightening

`Cable Detected → Straightening Rollers → Alignment Confirmed`

### Diameter Adaptation

`Diameter Sensor → PLC → Parameter Lookup → Machine Setpoints`

### Clamping

`Clamp Command → Force Feedback → Stable Hold`

### Cutting

`Position → Blade Depth → Cut → Position Verification`

### Stripping

`Strip Command → Linear Motion → Completion Check`

### Dumbbell Shaping

`Position → Die Actuation → Completion Check`

### Ejection

`Preparation Complete → Verification → Eject / Reject`

------------------------------------------------------------------------

## 9. Phase 7 --- HMI Implementation

### Home / Machine Status

Display:

-   Machine state
-   Active method
-   Cycle count
-   Current cable diameter
-   Current operation
-   Alarm state
-   Safety/E-stop state

### Test Method Selection

Provide selectable programs for:

-   IS 10810 Part 2
-   IS 10810 Part 7
-   IS 10810 Part 33
-   IS 7098 Part 1
-   IS 7098 Part 2

### Live Machine Monitor

Display:

-   Diameter
-   Clamp force
-   Feed position
-   Blade position
-   Active step
-   Cycle status
-   Faults

### Diagnostics

Display:

-   Motor/drive faults
-   Sensor status
-   Encoder status
-   Blade cycle count
-   Calibration reminders

### Batch Records

Store:

-   Batch/lot identifier
-   Selected test method
-   Cable diameter
-   Machine settings
-   Cycle result
-   Reject/flag status
-   Timestamp

------------------------------------------------------------------------

## 10. Phase 8 --- Verification and Quality Control

If an inline measurement system is fitted, implement:

``` text
Specimen Prepared
       ↓
Post-Cut Measurement
       ↓
PLC Comparison
       ↓
Within Tolerance?
    ┌──┴──┐
   YES    NO
    ↓      ↓
  PASS    FLAG
    ↓      ↓
 EJECT    REJECT
```

The initial prototype should prioritize measurement repeatability and
process stability before claiming production-level accuracy.

------------------------------------------------------------------------

## 11. Phase 9 --- Automatic Ejection and Waste Management

Install separate physical paths for:

### Good Specimen

`Verified → Ejector → Specimen Tray`

### Failed Specimen

`Out of Tolerance → Reject → Reject Tray`

### Waste

`Offcut / Stripped Material → Waste Chute → Waste Bin`

The operator should not need to reach into the cutting chamber during
normal operation.

------------------------------------------------------------------------

## 12. Phase 10 --- Safety Hardware

Implement safety before production-speed testing.

Required:

-   Enclosed cutting zone
-   Interlocked access door
-   Emergency stop
-   Safety relay
-   Guarded cutting tools
-   Guarded moving rollers
-   Safe motor shutdown
-   Light curtain where the loading arrangement requires it
-   Controlled restart procedure

Safety logic should be independent from normal process sequencing.

------------------------------------------------------------------------

## 13. Phase 11 --- Commissioning Procedure

### Stage A --- Dry Run

Test without cable:

-   PLC sequence
-   Servo motion
-   Clamp motion
-   Blade movement
-   Ejector movement
-   HMI commands
-   Safety circuits

### Stage B --- Low-Speed Cable Test

Test with representative cable:

-   Feed accuracy
-   Straightening
-   Clamp stability
-   Diameter sensing
-   Blade positioning

### Stage C --- Specimen Preparation

Perform repeated specimens for each supported method.

Measure:

-   Specimen dimensions
-   Cut consistency
-   Strip quality
-   Dumbbell profile
-   Reject frequency
-   Cycle time

### Stage D --- Repeatability Validation

Run repeated samples under controlled conditions and compare measured
results with the required tolerances of the applicable IS method.

------------------------------------------------------------------------

## 14. Hardware Automation Matrix

  --------------------------------------------------------------------------
  Stage             Input                Controller        Hardware Action
  ----------------- -------------------- ----------------- -----------------
  Cable loading     Cable presence       PLC               Enable feed

  Feeding           Encoder              PLC               Position cable

  Straightening     Position/alignment   PLC               Adjust/drive
                                                           rollers

  Diameter          Diameter sensor      PLC               Set clamp/blade
  adaptation                                               parameters

  Clamping          Load cell            PLC               Confirm secure
                                                           hold

  Circumferential   Position + encoder   PLC               Execute
  cut                                                      controlled cut

  Stripping         Actuator feedback    PLC               Execute linear
                                                           stripping

  Dumbbell cutting  Tool position        PLC               Actuate die

  Verification      Inline gauge, if     PLC               Pass/flag
                    fitted                                 

  Ejection          Cycle result         PLC               Route
                                                           specimen/reject

  Safety event      Door/E-stop          Safety relay      Safe stop
  --------------------------------------------------------------------------

------------------------------------------------------------------------

## 15. Suggested Prototype Hardware

### Mechanical

-   Machine frame
-   Feed rollers
-   Straightening rollers
-   Adjustable cable guides
-   Pneumatic/servo clamps
-   Cutting head
-   Stripping tool
-   Replaceable dumbbell die
-   Ejector
-   Waste chute/bin
-   Safety enclosure

### Sensors

-   Laser/optical diameter sensor
-   Clamp-force load cells
-   Rotary encoder
-   Linear position sensors
-   Cable-presence sensor
-   Optional inline dimensional gauge

### Control

-   Industrial PLC
-   HMI touchscreen
-   Servo drives
-   Motor drivers
-   Safety relay
-   Industrial power supply

### Actuation

-   Feed motor
-   Straightening motor
-   Clamp actuator
-   Blade actuator
-   Stripping actuator
-   Dumbbell press actuator
-   Specimen ejector

------------------------------------------------------------------------

## 16. Software Layer --- Secondary

Software should support the machine rather than replace the hardware
process.

Primary software functions:

-   PLC sequencing
-   HMI configuration
-   Method/program selection
-   Alarm management
-   Parameter storage
-   Batch logging
-   CSV-capable traceability records

No AI/ML model is required for the primary specimen-preparation control
loop.

------------------------------------------------------------------------

## 17. Demonstration Scenario

### Scenario: Automatic Preparation of a Cable Specimen

``` text
RAW CABLE
   ↓
AUTOMATIC FEED
   ↓
STRAIGHTEN
   ↓
DIAMETER MEASUREMENT
   ↓
AUTO-ADJUST CLAMP + BLADE
   ↓
CLAMP
   ↓
CIRCUMFERENTIAL CUT
   ↓
LINEAR STRIPPING
   ↓
DUMBBELL SHAPING IF REQUIRED
   ↓
DIMENSION VERIFICATION
   ↓
PASS / FLAG
   ↓
AUTOMATIC EJECTION
   ↓
TEST-READY SPECIMEN
```

The demonstration should show that the operator selects the required
test method and loads the cable, while the machine performs the
preparation sequence automatically.

------------------------------------------------------------------------

## 18. Final System Philosophy

``` text
                 HARDWARE FIRST
                      │
              ┌───────┴───────┐
              ↓               ↓
           MEASURE          ACTUATE
              │               │
        Sensors           Motors / Servo
        Encoders          Pneumatics
              │               │
              └───────┬───────┘
                      ↓
                    PLC
                      ↓
              PRECISION CUT
                      ↓
               VERIFY SAMPLE
                      ↓
             ┌────────┴────────┐
             ↓                 ↓
          ACCEPT             REJECT
             ↓                 ↓
        AUTO EJECT          WASTE
             ↓
       TEST-READY SAMPLE
```

**Core proposition:** Physically automate the cable specimen-preparation
process so that feeding, straightening, dimensional adaptation,
clamping, cutting, stripping, shaping, verification and ejection are
performed as one coordinated machine operation. PLC-based deterministic
control and sensor feedback provide the repeatability required for
reliable testing, while HMI and traceability functions support the
operator without becoming the primary solution.
