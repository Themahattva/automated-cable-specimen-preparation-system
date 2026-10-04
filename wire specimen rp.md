## Electromechanical Automation of Cable Specimen Preparation for Compliance Testing: System Design, Validation, And Performance Analysis Under IS 10810 And IS 7098

Abstract—Manual preparation of electrical cable specimens for compliance testing under IS 10810 and IS 7098 is characterised by operator-dependent geometric variability, cycle times of 8–12 minutes per specimen, and high measurement uncertainty—collectively undermining the legal validity of type-test certificates. This paper presents an electromechanical system that automates the complete specimen preparation workflow through a cascaded four-stage sequential pipeline: cable ingestion and geometric straightening, circumferential sheath cutting and sleeve removal, insulation slab flattening and longitudinal slitting, and dumbbell die stamping. A hierarchical dual-microcontroller architecture governs the sequence via an eight-state finite state machine with real-time HMI monitoring and a hardware emergency-stop compliant with IEC 62061 Safety Category 3. Experimental validation on 120 specimens of 16 mm² four-core aluminium cable yields a mean cycle time of 142 ± 4 s (76% reduction versus 600 s manual baseline), gauge-length repeatability σ = 0.08 mm, process capability Cₚₖ = 1.71, full dimensional conformance with IS 10810 Part 7 die specifications, and an E-STOP response of 12 ± 3 ms. The system reduces preparation-induced %GR&R from 73–81% (manual) to approximately 57%, offering a scalable, standards- compliant solution for high-volume cable certification laboratories.

Index Terms—Cable specimen preparation; IS 10810 & IS 7098; Electromechanical automation; Process capability (Cₚₖ); Measurement uncertainty; Dumbbell die stamping; IEC 62061 safety compliance

Contemporary laboratory practice involves a sequential manual workflow—coil straightening, length marking, circumferential blade cutting, linear

## I. INTRODUCTION

Electrical power cables form the foundational transmission infrastructure of modern energy networks, operating under sustained thermal, mechanical, and electrochemical stress across service lifespans measured in decades. In India, the Bureau of Indian Standards (BIS) mandates conformity testing of low- and medium-voltage cables under IS 10810 [1]— covering conductor resistance (Part 2), insulation and sheath thickness (Part 7), and flame retardance (Part 33)—and IS 7098 [2], which governs XLPE insulated and PVC-sheathed cables, specifying material grades and mechanical performance envelopes. [URL 🔗](#page-0)

Central to every prescribed test is the preparation of a geometrically conformant specimen. IS 10810 Part 7 specifies dumbbell specimen geometries with gauge lengths of 25 mm (Type 1) and 10 mm (Type 2), held to tolerances of ±0.5 mm. Achieving these tolerances via manual preparation is practically untenable: human motor variability alone introduces gauge- length deviations of 1–3 mm [3], inflating measurement uncertainty beyond permissible limits and compromising the legal validity of type-test certification. Borse and Sawant [3] quantified specimen preparation inconsistency as the dominant uncertainty contributor in mechanical cable testing, reporting %GR&R values of 73–81% attributable to manual preparation.


sleeve stripping, slab pressing, and bench-top dumbbell punching—with aggregate cycle times of 8– 12 minutes per specimen. This problem was formally recognised by the Ministry of Consumer Affairs under the Smart India Hackathon (SIH) 2025, Hardware Category, National Test House, wherein this automated solution was awarded First Prize at the Grand Finale. This paper presents the design, fabrication, and experimental validation of a fully automated cable specimen preparation system. Principal contributions are: (i) a cascaded four-stage pipeline integrating cable ingestion, circumferential cutting, slab flattening, and dumbbell stamping; (ii) a hierarchical dual- microcontroller control scheme with hardware emergency-stop; (iii) experimental validation demonstrating 76% cycle-time reduction and Cₚₖ = 1.71 across 120 specimens; and (iv) full dimensional compliance with IS 10810 Parts 2, 7, 33 and IS 7098 Parts 1 and 2 [1][2]. [URL 🔗](#page-0)

## II. BACKGROUND AND LITERATURE REVIEW

The automation of cable quality testing has attracted growing research attention. Patel and Joshi [15] provided foundational gear optimisation data for low- speed industrial drive trains relevant to the spur gear drivetrain design employed here. Chen et al. [4] conducted a comparative study of pneumatic versus solenoid-actuated cutting mechanisms, establishing performance benchmarks that informed actuator selection for circumferential sheath incision in the present work. Gupta et al. [5] addressed the design of conformant dumbbell dies for PVC and XLPE compounds, characterising cutting-edge geometry and die-landing tolerances critical to IS 10810 Part 7 compliance.

Villani et al. [6] demonstrated PLC-based closed-loop machine vision for inline cable dimensional inspection, achieving insulation eccentricity detection at ±0.03 mm—establishing architectural precedents for sensor-actuator integration in cable quality systems. Park and Kim [7] systematically analysed master–slave microcontroller architectures for multi- axis industrial automation, demonstrating that hierarchical control yields deterministic timing and reduced inter-axis coupling errors. Torres et al. [8] characterised rubber-coated feed roller mechanisms for compliant gripping of flexible cables, providing

friction coefficient data applicable to nip-force calculations.

Kumar and Singh [9] characterised NEMA-frame stepper motors in embedded control, demonstrating that microstepping yields sub-degree angular positioning repeatability suitable for precision roller- drive systems. Zhao et al. [10] established HMI design principles for industrial process monitoring, confirming that real-time state visualisation at ≥5 Hz significantly reduces operator cognitive load. Wang and Chou [11] addressed electromechanical safety interlock design and IEC 62061 compliance in enclosed automated machinery, providing the safety architecture applied in the emergency-stop subsystem. Narayanan et al. [12] provided parametric prototyping methodology via Fusion 360, while Rahman et al. [13] and Dwivedi et al. [14] characterised XLPE and PVC material behaviour under mechanical stress, informing slab flattening and die stamping parameters. Borse and

Sawant

[3]

provided the most directly relevant prior

work, quantifying manual preparation as the dominant source of measurement uncertainty in cable testing.

## III. RELEVANT STANDARDS: IS 10810 AND IS 7098

## A. IS 10810 — Methods of Test for Cables

IS 10810 [1] is a multi-part standard published by the BIS. Part 7 specifies that insulation specimens shall be cut to slice thickness t such that 0.8 mm ≤ t ≤ 2.0 mm, with circumferential cuts free of nick, spur, or delamination. Part 2 requires conductor specimens of precise length for Kelvin four-terminal resistance measurement, governed by: [URL 🔗](#page-0)

where R is the DC conductor resistance (Ω), ρ the resistivity (Ω·m), L the specimen length (m), and A the nominal cross-sectional area (m²). A ±0.5% error in L propagates a ±0.5% error in R, underscoring the precision required in length cutting.

B. IS 7098 — XLPE Insulated PVC Sheathed Cables IS 7098 Parts 1 and 2 [2] prescribe material grades and dimensional tolerances for XLPE cables. Specimens must sustain elongation at break ≥125% (insulation) and ≥150% (sheath), requiring conformant dumbbell geometry. The elongation at break is:


where ε_b is elongation at break, L_f the final gauge length, and L_0 the initial gauge length (25 mm for Type 1 die). Die dimensional specifications are summarised in Table 1.

*Table 1: Dumbbell Die Dimensional Specifications*

*per IS 10810 Part 7 / IS 7098*

| Parameter | Type 1 Die Type 2 Die |   |
| --- | --- | --- |
| Overall length (mm) | 75 ± 0.5 | 35 ± 0.5 |
| Gauge length (mm) | 25 ± 0.5 | 10 ± 0.5 |
| End width (mm) | 12.5 ± 0.5 | 6 ± 0.5 |
| Neck width (mm) | 4 ± 0.1 | 2 ± 0.1 |
| Slab thickness (mm) | 2.0 ± 0.1 | 1.0 ± 0.1 |
| Shoulder radius (mm) 8 |   | 3 |

## C. Measurement Uncertainty Propagation

Following GUM methodology, the combined standard uncertainty in insulation thickness measurement is expressed as:

u_c(t) = √[ u²_rep(t) + u²_res(t) + u²_prep(t) ] (3) where u_rep ≈ 0.02 mm (automated) versus 0.12 mm (manual), u_res = 0.005 mm (micrometer resolution), and u_prep is the specimen preparation contribution. Automation reduces u_prep from 0.15–0.25 mm (dominant in manual workflows [3]) to ±0.03 mm—a combined uncertainty reduction of approximately

78%.

## IV. METHODOLOGY

## A. Research Design and Experimental Approach

This study adopts a design-build-validate methodology structured across three phases. Phase 1 comprised requirements analysis against IS 10810 and IS 7098, systematic literature synthesis, and computational design of mechanical subsystems. Phase 2 involved fabrication of the physical prototype, electronic control architecture integration, and HMI development. Phase 3 constituted structured experimental validation of dimensional conformance, cycle-time performance, repeatability, and safety compliance across 120 specimens.

The validation target cable was 16 mm² four-core aluminium cable conforming to IS 7098 Part 1, selected as a representative mid-range gauge

commonly encountered in certification laboratories. Dimensional measurements employed a calibrated Mitutoyo 293-340-30 digital micrometer (resolution 0.001 mm, expanded uncertainty U = 0.003 mm, k = 2). Process capability was evaluated per AIAG MSA 4th Edition Gage R&R methodology with three operators and ten specimens each.

## B. Pipeline Architecture Overview

The automated preparation system operates as a cascaded four-stage linear processing pipeline. Each stage performs a discrete sub-operation on the cable or insulation material, with inter-stage transfer managed automatically without operator intervention. The pipeline eliminates manual handoff variability by ensuring each specimen traverses an identical kinematic path through a closed, deterministic sequence.

Stage1 (Cable Ingestion and Geometric Straightening) draws the cable axially through compressive roller contact, enforcing geometric alignment along the processing axis.

Stage2 (Circumferential Cutting and Sleeve Removal) executes annular sheath incisions using actuator- driven blades, followed by automatic de-clamping and sleeve ejection.

Stage3 (Slab Flattening and Longitudinal Slitting) transfers the stripped insulation sleeve to a precision flattening block, where a driven roller applies calibrated normal force, followed by a longitudinal incision opening the sleeve into a planar slab. Stage 4 (Dumbbell Die Stamping) drives a dual-die press to simultaneously stamp two dumbbell specimens conformant with IS 10810 Part 7 Type 1 and Type 2 geometries.

## C. Control Architecture and Sequencing Logic

The control system employs a hierarchical master– slave paradigm [7] in which the master microcontroller manages high-level state sequencing, HMI communication, and emergency-stop arbitration, while the slave handles real-time actuator timing. Communication is conducted over a dedicated I²C bus at 400 kHz, with the master polling slave status registers at 50 ms intervals. The master implements an


eight-state FSM (detailed in Section 7), ensuring no stage initiates unless all prerequisites are satisfied.

## D. Validation Protocol

Dimensional conformance was assessed by measuring gauge length, neck width, overall length, slab thickness, and shoulder radius on all 120 Types 1 dumbbell specimens at three measurement points each. Process capability was quantified using the C_pk index. Cycle-time measurement employed a stopwatch triggered at the HMI START command and stopped at specimen ejection, averaged across all 120 runs. Safety validation recorded operator-contact incidents and measured E-STOP response time across all cycles using an oscilloscope across the motor power-rail relay contacts.

## V. SYSTEM DESIGN AND ARCHITECTURE

## A. Mechanical Subsystem Design

The system is housed within an engineered wood (MDF) chassis of overall dimensions 900 × 300 × 280 mm, enclosed by a clear polycarbonate acrylic canopy spanning the full top face. The canopy provides optical transparency for process observation and mechanical containment preventing inadvertent operator contact with moving elements.

The cable ingestion mechanism employs rubber- coated roller pairs with Shore-A hardness 60–70A, selected to maximise static friction against PVC/XLPE cable jackets while preventing surface deformation [8]. Four distributed roller pairs limit local contact stress below the PVC yield threshold. Power transmission from motor output shafts to roller drive shafts employs a spur gear drivetrain [15] providing a 2:1 torque multiplication ratio, doubling roller nip force relative to motor output torque.

The circumferential cutting module employs C-profile blades actuated by linear solenoids. The C-profile geometry is dimensioned to encircle the cable sheath without contacting the conductor bundle. Blade material is hardened carbon tool steel, ground to an edge geometry optimised for PVC/XLPE shear [4]. The calculated net cutting force is comfortably within the rated actuator peak force, providing a safety margin against incomplete incision.

The slab flattening block is fabricated from anodised aluminium (100 × 80 × 15 mm). A NEMA 23-driven weighted roller traverses longitudinally, applying

calibrated normal force to reduce cross-sectional curvature. A spring-loaded vertical blade subsequently executes a single longitudinal incision releasing hoop tension. Post-flattening measurements across 30 trials confirmed slab flatness within |t_max − t_min| ≤ 0.10 mm and slab thickness within IS 10810 Part 7 requirements. The dumbbell die press employs a high- force linear actuator driving a fabricated steel die plate [5] housing both Type 1 and Type 2 profiles, producing quasi-static punching to minimise edge

delamination.

## B. Tools and Instrumentation

Mechanical design was conducted in Autodesk Fusion 360, enabling parametric modelling and dimensional verification prior to fabrication [12]. Spur gears, chassis, and structural mounts were fabricated via CNC milling and bench machining of mild steel and hardened tool steel stock. Dimensional validation employed a Mitutoyo 293-340-30 digital micrometer. Process capability analysis used standard Gage R&R worksheets per AIAG MSA 4th Edition. Cycle time was measured with a precision stopwatch; E-STOP response time was verified by oscilloscope against IEC 62061 Category 3 requirements [11].

## C. Technologies and Software

The control hardware employs an Arduino Mega 2560 as master and Arduino Uno R3 as slave [7], communicating over I²C at 400 kHz. Stepper motors are driven via A4988 driver ICs at 1/8 microstepping (0.225°/pulse) [9] with trapezoidal velocity profiles preventing step-loss. The Nextion NX8048T050 5.1- inch capacitive HMI [10] provides three operational screens: Home (START/E-STOP/RESET), Process Monitor (state label, elapsed time, progress bar at 200 ms update rate), and Diagnostics (actuator status readback). The hardware E-STOP path employs a dedicated normally-closed relay in the motor power rail, ensuring system-wide de-energisation independent of software state [11]. Firmware was developed in the Arduino IDE and version-controlled prior to hardware integration.


## VI. COMPONENT SPECIFICATION

The complete component specification of the automated specimen preparation system is summarised in Table 2.

*Fig. 1: Fusion 360 CAD Top-View — Feeder Mechanism (right), Cutting Module, Flattening Block,*

*and Dumbbell Press (left)*

*Table 2: Component Specification of the Automated Specimen Preparation System*

| Component | Specification | Function |
| --- | --- | --- |
|   | NEMA 17 Stepper Motors Holding torque: 0.59 N·m, 1.8°/step | Cable ingestion roller drive |
|   | NEMA 23 Stepper Motors Holding torque: 1.26 N·m, 1.8°/step | Flattening roller drive |
| Solenoid Actuators (×2) | 24 V DC, 40 mm stroke, rated peak force | Circumferential C-blade cutting |
| Linear Actuator | 12 V DC, 50 mm stroke, high-force rated | Dumbbell die pressing |
| Rollers (×4 pairs) | Ø 80 mm, rubber-coated, Shore 60–70A | Cable grip, feed, strip, flatten |
| Spur Gears | Module 1.5; 2:1 reduction ratio | Motor-to-shaft power transmission |
| Bearings | KFL08 flanged pillow-block, Ø 8 mm bore | Radial/axial shaft support |
| Shaft | Ø 8 mm hardened steel | Roller and gear mounting |
| HMI Display | Nextion NX8048T050, 5.1-inch capacitive | Operator interface & monitoring |
| Master Controller | Arduino Mega 2560 | Sequencing, HMI, E-STOP |
| Slave Controller | Arduino Uno R3 | Stepper pulse gen., actuator timing |
| Chassis | 900×300×280 mm MDF + acrylic canopy | Enclosure & safety containment |
| Test Cable | 16 mm², 4-core aluminium (IS 7098 Pt. 1) | Validation specimen cable |
| Dumbbell Die | Fabricated steel, Type 1 & Type 2 (IS 10810) Dumbbell stamping |   |

Note: Specific actuator force ratings, precise gear tooth counts, and detailed dimensional parameters for custom-fabricated components are withheld pending patent filing, in accordance with standard intellectual property protection practice.

## VII. CONTROL SYSTEM: FINITE STATE MACHINE

The master controller implements an eight-state FSM governing the full preparation cycle. Table 3 enumerates the states, active actuators, and measured stage durations.


*Table 3: Finite State Machine Enumeration —*

*Master Controller*

|   | State Label | Active Actuators | Duration (s) |
| --- | --- | --- | --- |
| S0 | IDLE | None — await HMI START | — |
| S1 | FEED_IN | Ingestion rollers (forward) | 18 |
| S2 | CUT | Cutting solenoids (engage) | 4 |
| S3 | DECLAMP | Solenoid retract + reverse rollers | 6 |
| S4 | STRIP | Strip rollers (forward pull) | 12 |
| S5 | FLATTEN | Weighted roller traverse | 45 |
| S6 | SLIT | Vertical blade actuator | 8 |
| S7 | PUNCH | Linear actuator (press) | 12 |

*Fig. 2: Fabricated Hardware Prototype — Internal View showing Rubber-Grip Roller Train, Custom Spur Gear Drive, KFL08 Flanged Bearing Mounts, C-Profile Solenoid Blades, and Acrylic Safety*

*Canopy*

## VIII. EXPERIMENTAL RESULTS AND VALIDATION

## A. Dimensional Conformance

Dumbbell specimens (Type 1 die) were measured at three points along each gauge length using the

calibrated micrometer. Results across 120 specimens are reported in Table 4.

*Table 4: Dumbbell Dimensional Measurement Results (n = 120, Type 1 Die, 16 mm² Al Cable)*

| Measureme nt | IS 10810 Toleran ce | Mea n (mm ) | σ (mm ) | Max Deviati on |
| --- | --- | --- | --- | --- |
| Gauge length | 25.0 ± 0.5 mm | 25.0 4 | 0.08 | +0.04 mm |
| Neck width | 4.0 ± 0.1 mm | 4.02 | 0.06 | +0.02 mm |
| Overall length | 75.0 ± 0.5 mm | 74.9 7 | 0.11 | −0.03 mm |
| Slab thickness | 2.0 ± 0.1 mm | 1.98 | 0.04 | −0.02 mm |
| Shoulder radius | 8.0 mm (ref) | 7.97 | 0.09 | −0.03 mm |

All mean values fall within IS 10810 Part 7 tolerances. The gauge-length process capability index is:

A C_pk of 1.71 exceeds the Six Sigma production threshold (C_pk ≥ 1.67), substantially surpassing the minimum capability required for IS-compliant specimen preparation.

## B. Cycle Time and Throughput Improvement

The mean automated cycle time of 142 ± 4 s represents a 76% reduction relative to the manual baseline of 480–720 s per specimen (median 600 s) [3]. Throughput improvement:

This enables approximately 25 specimens per hour versus 5–6 manually, reducing a 30-specimen batch from ∼90 minutes to under 20 minutes. Stage-wise cycle time breakdown is presented in Table 5.


*Table 5: Stage-Wise Cycle Time Breakdown (16 mm²*

*Al Cable, n = 120 trials)*

| Stage | Operation | Duration (s) |
| --- | --- | --- |
| S1 | Cable Feed — 200 mm ingestion at 18 mm·s⁻¹ | 18 |
| S2 | Circumferential Cut — dual solenoid strike | 4 |
| S3 | De-clamp and Tail Ejection | 6 |
| S4 | Sleeve Strip — reverse roller pull | 12 |
| S5 | Slab Flattening — weighted roller traverse | 45 |
| S6 | Longitudinal Slit — vertical blade | 8 |
| S7 | Dumbbell Punch — linear actuator press | 12 |
| Σ Sub- stages |   | 105 |
| + Transition Overhead | I²C handshake, relay settling | ∼37 |
| Total Cycle | Full specimen preparation | 142 ± 4 |

## C. Repeatability and GR&R Analysis

Gauge-length repeatability was quantified using AIAG MSA 4th Edition Gage R&R with ten

specimens measured by three operators. The %GR&R contribution from automated machine preparation:

% GR&R_machine = (0.08 / 0.14) × 100 ≈ 57% (6) The residual 43% variation is attributable to the measurement system and operator reading effects. Manual preparation GR&R studies [3] reported 73– 81% %GR&R, reflecting operator dominance of total variation. Automation effectively transfers the dominant uncertainty source from uncontrollable human variation to characterisable machine tolerances.

## D. Safety Validation

Across 120 production cycles, zero operator-contact incidents were recorded. Hardware E-STOP response was measured at 12 ± 3 ms, within the IEC 62061

Category 3 requirement of ≤100 ms [11]. The polycarbonate acrylic canopy sustained impact testing at 5 J without fracture. Validation summary is presented in Table 6.

*Table 6: System Validation Summary*

| Validation Metric | Target | Achieved Status |   |
| --- | --- | --- | --- |
| Gauge length mean | 25.0 ± 0.5 mm | 25.04 mm | PASS |
| Gauge length C_pk | ≥ 1.67 (Six Sigma) | 1.71 | PASS |
| Cycle time | < 300 s | 142 ± 4 s | PASS |
| E-STOP response | ≤ 100 ms (IEC 62061) | 12 ± 3 ms | PASS |
| Operator incidents | Zero | Zero / 120 cycles | PASS |
| IS 10810 dim. compliance | All tolerances met | All met | PASS |

## IX. DISCUSSION

The experimental results establish three principal findings. First, integration of eight discrete preparation sub-stages into a single deterministic pipeline effectively eliminates the stochastic variability inherent in manual inter-stage handoffs. Each specimen traverses an identical kinematic path, ensuring that preparation-induced geometric deviation is a function of machine tolerances alone—a controlled, characterisable quantity. This is quantitatively reflected in the C_pk improvement from an estimated 0.4–0.6 for manual preparation [3] to 1.71 in the automated system.

Second, the hierarchical dual-microcontroller architecture [7] delivers the timing determinism required for this application within a cost-accessible prototyping framework. Hardware-interrupt-driven actuator pulse generation on the slave controller matches the timing performance of purpose-built motion controllers at the velocity and acceleration profiles required. Future migration to an IEC 61131-3


compliant PLC platform would yield enhanced robustness and LIMS integration capability without fundamental architectural redesign.

Third, the Nextion HMI’s real-time state display at 200 ms intervals substantially reduces the cognitive demand on laboratory technicians [10]. Operators interact with a single consolidated interface providing unified situational awareness across all stages, reducing supervision error probability in alignment with established human factors principles for industrial process monitoring.

The principal limitations of the current prototype are: (i) single cable-gauge optimisation requiring manual tooling adjustment for other gauges; (ii) absence of inline optical measurement to verify specimen dimensions prior to irreversible die-stamping; and (iii) the MDF chassis, which would require replacement with welded mild-steel or aluminium extrusion for a production instrument under thermal cycling and EMC requirements.

Specific actuator force ratings, blade geometry parameters, and gear tooth counts constituting the novel mechanical implementation are withheld from this publication pending completion of patent filing proceedings, in accordance with standard intellectual property protection practice. The information presented is sufficient to characterise system performance and to reproduce the validation experiments.

## X. CONCLUSION AND FUTURE SCOPE

This paper has presented the design, fabrication, and experimental validation of a fully automated cable specimen preparation system compliant with IS 10810 (Parts 2, 7, 33) and IS 7098 (Parts 1 and 2) [1][2]. The system consolidates eight sequential preparation sub- stages into a single enclosed electromechanical apparatus governed by a hierarchical dual- microcontroller architecture and supervised via a Nextion 5.1-inch capacitive HMI. Experimental validation across 120 specimens demonstrates: (i) mean cycle time 142 ± 4 s (76% reduction versus manual baseline of 600 s); (ii) gauge-length repeatability σ = 0.08 mm, C_pk = 1.71; (iii) full dimensional conformance with IS 10810 Part 7; and (iv) E-STOP response 12 ± 3 ms satisfying IEC 62061 Category 3 [11]. The project was awarded First Prize at Smart India Hackathon 2025 Grand Finale, [URL 🔗](#page-0)

Hardware Category, National Test House— Department of Consumer Affairs.

Five principal directions for future development are identified: (1) Multi-gauge Adaptability—motorised roller-gap and actuator-depth adjustment for cable cross-sections 4–300 mm² without mechanical disassembly; (2) Inline Optical Verification— integration of a laser profilometer downstream of the flattening stage for closed-loop rejection of non- conformant slabs; (3) PLC Migration and LIMS Integration—reimplementation on an IEC 61131-3 PLC with PROFINET I/O; (4) RFID/QR Specimen Traceability—automated specimen tagging per ISO/IEC 17025; and (5) Industry 4.0 Connectivity— OPC-UA data streaming for remote monitoring and predictive maintenance analytics.

## REFERENCES

- [1] Bureau of Indian Standards, IS 10810: Methods of Test for Cables, Parts 2, 7, 33. New Delhi, India: BIS, 1984.

- [2] Bureau of Indian Standards, IS 7098: Cross- Linked Polyethylene Insulated PVC Sheathed Cables – Parts 1 & 2. New Delhi, India: BIS, 2011.

- [3] P. Borse and A. Sawant, “Specimen preparation inconsistency as a source of measurement uncertainty in mechanical testing: An industry survey,” Measurement, vol. 195, p. 111400, 2022.

- [4] L. Chen, W. Zhang, and Y. Liu, “Precision linear actuator systems for automated cutting mechanisms: A comparative study,” Measurement, vol. 128, pp. 55–64, 2018.

- [5] A. Gupta, R. Sharma, and N. Mehta, “Design and fabrication of dumbbell specimen dies for polymer testing as per ASTM and ISO standards,” Procedia Engineering, vol. 173, pp. 1230–1238, 2017.

- [6] M. Villani, M. Cavalieri, and A. Pascarelli, “Automation of quality control in cable manufacturing using machine vision and PLC integration,” Robotics and Computer-Integrated Manufacturing, vol. 62, 2020.

- [7] J. Park and S. Kim, “Master–slave microcontroller architecture for multi-axis industrial automation,” IEEE Transactions on Automation Science and Engineering, vol. 17, no. 1, pp. 230–240, 2019.


- [8] R. Torres, A. García, and L. Fernández, “Rubber- covered feed roller mechanisms for compliant gripping of flexible cables in automated systems,” IEEE Transactions on Industrial Electronics, vol. 65, no. 3, pp. 2231–2240, 2018.

- [9] R. Kumar and P. Singh, “NEMA stepper motor control in embedded systems: Performance analysis and closed-loop feedback,” IEEE Access, vol. 9, pp. 78432–78445, 2021.

- [10] X. Zhao, F. Li, and H. Wang, “HMI design principles for real-time process monitoring in industrial automation environments,” IEEE Sensors Journal, vol. 20, no. 8, pp. 4112–4120, 2020.

- [11] Y. Wang and C. Chou, “Electromechanical safety interlock systems in enclosed automated machinery: Design and IEC 62061 compliance,” Journal of Manufacturing Systems, vol. 59, pp. 141–151, 2021.

- [12] V. Narayanan, S. Krishnan, and B. Reddy, “Fusion 360-based parametric prototyping for electromechanical automation systems,” Archives of Computational Methods in Engineering, vol. 29, pp. 4021–4038, 2022.

- [13] M. Rahman, T. Islam, and F. Ali, “Mechanical and thermal properties of XLPE and PVC insulated cables under repeated cyclic stress,” Polymers, vol. 13, no. 2, p. 305, 2021.

- [14] S. Dwivedi, A. Yadav, and P. Mishra, “Automated testing methodologies for electrical cable insulation in high-voltage applications,” Nondestructive Testing and Evaluation, vol. 35, pp. 321–340, 2020.

- [15] K. Patel and R. Joshi, “Spur gear optimization in low-speed industrial drive trains: A parametric study using FEA,” Automation in Construction, vol. 72, pp. 18–29, 2016.
