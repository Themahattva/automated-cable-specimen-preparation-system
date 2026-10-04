# Automated Cable Specimen Preparation System (ACSPS)

## Hardware-First Technical Specifications

**Project:** Automated Cable Specimen Preparation System for IS 10810
and IS 7098 Compliance\
**Problem Statement ID:** 26030\
**Design priority:** Hardware integration, automatic specimen
preparation, deterministic industrial control and physical safety.
Software is secondary.

------------------------------------------------------------------------

## 1. Objective

Eliminate the manual and semi-automated preparation steps currently
required before cable testing by developing a single automated machine
that:

`FEED → STRAIGHTEN → MEASURE → CLAMP → CUT/STRIP → SHAPE → EJECT`

The machine is intended to prepare repeatable cable specimens for
testing associated with:

-   IS 10810 Part 2
-   IS 10810 Part 7
-   IS 10810 Part 33
-   IS 7098 Part 1
-   IS 7098 Part 2

The machine must accommodate cable specimens involving PVC, XLPE and
HDPE insulation/sheath materials.

------------------------------------------------------------------------

## 2. Problems Targeted

-   Manual cable cutting
-   Manual straightening
-   Operator-dependent specimen positioning
-   Separate electrically/pneumatically operated slicing operations
-   Separate dumbbell cutting operation
-   Repeated machine setup and calibration
-   Variation caused by cable diameter
-   Inconsistent blade depth
-   Inconsistent clamp force
-   Human error during specimen preparation
-   Non-repeatable specimen dimensions
-   Excess preparation time and operator effort
-   Cutting-zone safety exposure
-   Lack of integrated specimen traceability

------------------------------------------------------------------------

## 3. Hardware-First Architecture

``` text
RAW CABLE / REEL
      ↓
MOTOR-DRIVEN FEED ROLLERS
      ↓
STRAIGHTENING ROLLERS
      ↓
DIAMETER MEASUREMENT
      ↓
ADJUSTABLE CLAMPING
      ↓
PLC / INDUSTRIAL CONTROLLER
      ↓
PRECISION CUTTING & STRIPPING
      ├── Circumferential Slice
      ├── Linear Stripping
      └── Dumbbell Specimen Cutting
      ↓
DIMENSION / CUT VERIFICATION
      ↓
AUTOMATIC SPECIMEN EJECTION
      ↓
SPECIMEN COLLECTION
      ↓
WASTE COLLECTION
```

------------------------------------------------------------------------

## 4. Cable Feeding and Straightening Hardware

### Motor-Driven Feed Rollers

Purpose:

-   Pull cable from the supply reel/spool.
-   Maintain controlled feed speed.
-   Position the cable at the cutting station.
-   Provide repeatable specimen length.

Representative hardware:

-   Geared DC motor or servo motor
-   Hardened feed rollers
-   Adjustable roller pressure
-   Encoder for feed-length feedback

### Straightening Roller Assembly

Purpose:

-   Remove curvature introduced by storage on a reel.
-   Present the cable in a controlled and repeatable condition before
    cutting.

Representative hardware:

-   Counter-rotating roller sets
-   Adjustable roller spacing
-   Linear guides
-   Mechanical alignment supports

------------------------------------------------------------------------

## 5. Diameter Measurement Hardware

Use a non-contact diameter sensor before the cutting operation.

### Recommended Technology

-   Laser diameter sensor
-   Optical micrometer
-   Equivalent non-contact measurement system

### Measurement Role

``` text
Cable
  ↓
Diameter Sensor
  ↓
Measured Diameter
  ↓
PLC
  ↓
Clamp Force + Blade Depth Setpoint
```

The sensor provides the dimensional input required for diameter-adaptive
machine settings.

------------------------------------------------------------------------

## 6. Automated Clamping System

The clamping mechanism must hold the cable securely without damaging or
excessively deforming the specimen.

### Hardware

-   Pneumatic or servo-actuated jaw clamps
-   Adjustable jaw geometry
-   Load cells for clamp-force feedback
-   Mechanical guides
-   Position sensors

### Control

``` text
Diameter Measurement
        ↓
PLC
        ↓
Required Clamp Force
        ↓
Clamp Actuator
        ↓
Stable Cable Position
```

The clamp force should be tuned during commissioning using actual cable
constructions.

------------------------------------------------------------------------

## 7. Precision Cutting and Stripping Module

The central hardware module combines the preparation operations that are
currently distributed across multiple machines.

### 7.1 Circumferential Cutting

Used to create controlled circumferential cuts through the required
insulation/sheath layer.

Hardware:

-   Precision circular/segmented blade arrangement
-   Servo or linear actuator
-   Programmable blade-depth mechanism
-   Encoder-based position feedback
-   Replaceable cutting tooling

### 7.2 Linear Stripping

Used to remove the selected insulation/sheath section after controlled
scoring.

Hardware:

-   Linear stripping blade/tool
-   Servo/linear actuator
-   Controlled pull/strip mechanism
-   Specimen support

### 7.3 Dumbbell Specimen Cutting

The machine should integrate a replaceable dumbbell cutting die/tool for
specimen shaping where required.

Hardware:

-   Standard-profile dumbbell die
-   Servo/pneumatic press mechanism
-   Die-position sensor
-   Guarded cutting chamber

Exact specimen geometry must be configured from the applicable current
IS method rather than assumed during machine commissioning.

------------------------------------------------------------------------

## 8. Closed-Loop Motion Control

The machine should use encoder feedback for:

-   Feed length
-   Cutting-head position
-   Blade-axis position
-   Clamp position
-   Specimen positioning

Representative motion hardware:

-   Servo motors
-   Servo drives
-   Linear guide rails
-   Ball-screw or equivalent linear actuator
-   Rotary/linear encoders

------------------------------------------------------------------------

## 9. PLC and Industrial Control Hardware

### PLC

The PLC is the primary deterministic controller.

Responsibilities:

-   Read diameter sensor
-   Read clamp-force feedback
-   Control feed rollers
-   Control straightening sequence
-   Control clamps
-   Position cutting tools
-   Execute method-specific cut sequences
-   Verify cycle completion
-   Control specimen ejection
-   Generate alarms
-   Interface with HMI
-   Record batch information

Representative class:

-   Siemens S7-1200 class or equivalent industrial PLC

### HMI

The HMI provides:

-   Test method selection
-   Machine status
-   Active program
-   Diameter reading
-   Clamp-force status
-   Blade position
-   Cycle count
-   Alarm display
-   Maintenance status
-   Batch/lot information

------------------------------------------------------------------------

## 10. Method-Driven Hardware Configuration

The operator selects the required test method from the HMI.

``` text
IS TEST METHOD
      ↓
PLC PROGRAM SELECTION
      ↓
LOAD METHOD PARAMETERS
      ↓
READ CABLE DIAMETER
      ↓
CALCULATE MACHINE SETPOINTS
      ↓
CLAMP + CUT + STRIP + SHAPE
```

The program library should contain separate parameter sets for:

-   IS 10810 Part 2
-   IS 10810 Part 7
-   IS 10810 Part 33
-   IS 7098 Part 1
-   IS 7098 Part 2

The exact standard-specific dimensions, tolerances, cut lengths and
specimen profiles must be verified against the current BIS documents
before production release.

------------------------------------------------------------------------

## 11. Sensor and Feedback Hardware

  -----------------------------------------------------------------------
  Sensor                  Measures                Hardware Decision
  ----------------------- ----------------------- -----------------------
  Diameter sensor         Overall cable diameter  Adjust blade/clamp
                                                  settings

  Clamp load cell         Clamping force          Prevent excessive
                                                  compression

  Feed encoder            Cable travel            Confirm specimen length

  Blade encoder           Tool position           Confirm blade
                                                  depth/position

  Linear position sensor  Actuator position       Confirm motion

  Optional inline gauge   Post-cut dimension      Pass/flag specimen
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 12. Automatic Specimen Ejection and Waste Handling

After successful preparation:

``` text
CUT COMPLETE
    ↓
DIMENSION CHECK
    ↓
PASS?
 ┌──┴──┐
YES    NO
 ↓      ↓
EJECT  FLAG / REJECT
 ↓
SPECIMEN COLLECTION
```

Separate waste handling should collect:

-   Removed insulation/sheath
-   Cable offcuts
-   Failed specimens
-   Cutting debris

This prevents manual removal from the cutting area during normal
operation.

------------------------------------------------------------------------

## 13. Safety Hardware

Safety must be implemented as a dedicated hardware layer.

Required provisions:

-   Fully enclosed cutting chamber
-   Interlocked access doors
-   Emergency-stop circuit
-   Category-rated safety relay
-   Light curtain/operator loading protection where required
-   Guarded moving components
-   Motor/drive protection
-   Safe restart procedure

The safety circuit should remain independent of normal PLC sequencing so
that a software fault cannot disable the safety function.

------------------------------------------------------------------------

## 14. Hardware Automation Matrix

  ------------------------------------------------------------------------
  Problem            Hardware Input    Controller        Physical Response
  ------------------ ----------------- ----------------- -----------------
  Cable curvature    Straightening     PLC               Automatic
                     mechanism                           straightening

  Variable diameter  Diameter sensor   PLC               Adjust
                                                         clamp/blade
                                                         settings

  Cable movement     Feed encoder      PLC               Correct feed
                                                         position

  Excess clamp force Load cell         PLC               Reduce/limit
                                                         clamp force

  Incorrect blade    Blade encoder     PLC               Stop/adjust
  position                                               cutting axis

  Incorrect specimen Feed/linear       PLC               Correct feed
  length             encoder                             position

  Cutting fault      Position/status   PLC               Stop cycle +
                     sensors                             alarm

  Out-of-tolerance   Inline gauge, if  PLC               Reject/flag
  specimen           fitted                              specimen

  Operator access    Door              Safety relay      Immediate safe
                     interlock/light                     stop
                     curtain                             
  ------------------------------------------------------------------------

------------------------------------------------------------------------

## 15. Hardware Technology Stack

**Mechanical:** Feed rollers, straightening rollers, clamps, linear
guides, cutting tools, dumbbell die, ejection mechanism\
**Motion:** Servo motors, servo drives, encoders, linear actuators\
**Sensing:** Laser/optical diameter sensor, load cells, position
sensors, encoders, optional inline gauge\
**Control:** Industrial PLC, safety relay, HMI\
**Actuation:** Feed motor, clamp actuator, blade actuator, stripping
actuator, dumbbell press, ejector\
**Safety:** Guards, interlocks, emergency stop, light curtains, safety
relay\
**Traceability:** HMI/local controller batch logging and CSV-capable
records

------------------------------------------------------------------------

## 16. Final Design Principle

``` text
             HARDWARE FIRST
                    │
       ┌────────────┴────────────┐
       ↓                         ↓
   MEASURE                    CONTROL
       │                         │
 Diameter / Force          PLC / Servo
 Position / Length         Pneumatic
       │                         │
       └────────────┬────────────┘
                    ↓
          PRECISION SPECIMEN
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
       VERIFY              EJECT
          │                   │
          └─────────┬─────────┘
                    ↓
             TEST-READY SAMPLE
```

**Core proposition:** Replace operator-dependent preparation with a
single mechanically integrated, sensor-guided and PLC-controlled
specimen-preparation machine that produces consistent test specimens
while improving preparation speed, repeatability and operator safety.
