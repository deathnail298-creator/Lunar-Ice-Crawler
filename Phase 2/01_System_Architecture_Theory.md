01 — System Architecture Theory

Moon Ice Crawler — Phase 2

1. Purpose of This Document

This document defines the theoretical system architecture for the Moon Ice Crawler.

Goals:

Describe the system as a set of subsystems with clear responsibilities.

Define interfaces between subsystems and to external infrastructure.

Capture state-level behavior (what the crawler does, in what order).

Lock invariants that must survive into Phase-3 if it ever exists.

This is not a build spec. There are no numbers, no hardware drawings, and no TRL claims. This is the minimum architecture needed so real engineers can later decide whether it deserves actual work.

2. System-Level Concept
2.1 Mission Role (Restated in Architecture Terms)

The Moon Ice Crawler is a slow, local infrastructure asset whose job is:

Move to pre-identified ice-bearing regolith patches.

Form a sealed contact zone between its heater head and the surface.

Melt and vaporize shallow ice in place using a conical heater.

Capture the resulting water vapor into an onboard containment volume.

Hand off collected water (dirty is fine) to upstream ISRU systems that will do the real processing.

It is not:

A refinery

A rover for exploration

A sample-return system

A “smart” autonomy platform

It is a dirty machine that heats, captures, and hands off.

2.2 High-Level Block View (Textual)

Think of the Crawler as:

A chassis with tracks/wheels that can crawl between patches.

A front-mounted heater head with a sealing skirt that drops onto the surface.

A closed vapor path from under the head into an internal capture volume.

A cold-side condensation and storage module to hold collected water.

A power + control spine that drives heater, pumps/valves (if any), and mobility.

In text-block form:

Regolith Patch
↕ (sealed contact via heater head + skirt)
Heater Head → Vapor Path → Condenser / Capture Volume → Storage / Transfer Interface
↕
Chassis + Mobility + Power + Control

3. Subsystem Breakdown
3.1 Chassis & Mobility Subsystem

Purpose

Physically carry all other subsystems.

Provide deliberate, low-speed repositioning between ice patches and transfer points.

Key Architectural Characteristics

Low-speed, high-torque locomotion; time is cheap.

Geometry sized to stay stable during head deployment (no tipping when heater is down).

Ground clearance sufficient for rocks and modest terrain irregularities.

All mass above a single rigid spine (consistent with your general “spine + modules” philosophy).

Responsibilities

Move from home base / staging point to target patch and back.

Maintain positional accuracy sufficient to seat the heater head correctly.

Survive repeated duty cycles over long periods; no expectation of speed or agility.

Interfaces

Mechanical interfaces to:

Heater head module (front)

Storage / transfer interface (rear or side)

Electrical/power bus from Power Subsystem.

Command/telemetry link from Control & Sensing Subsystem.

3.2 Heater Head & Contact Plate Subsystem

Purpose

Apply controlled heat into the regolith to melt/vaporize near-surface ice.

Provide the primary mechanical and thermal interface with the ground.

Key Architectural Characteristics

Conical heater geometry (your “conical heater system”) with a flat or slightly contoured contact plate.

No exposed moving parts in the thermal zone; if anything moves, it’s simple vertical actuation of the entire head.

Designed for repeatable seating: once the crawler is roughly positioned, the head drops, seals, and begins a heat cycle.

Responsibilities

Deliver heat into regolith within a bounded footprint (no deep drilling).

Maintain a temperature gradient that encourages ice melt and vaporization toward the captured region, not outward leaks.

Host structural features for the sealing skirt and vapor inlet(s).

Interfaces

Structural mount to chassis.

Thermal connection to power/heat source (whatever emerges from Phase-1 heater experiments).

Fluid interface to Vapor Capture & Ducting Subsystem.

Invariants

Head operates in discrete cycles (down → heat → up), not continuous plowing.

Heat-only; no mechanical excavation, drilling, or augering in this architecture.

3.3 Sealing Skirt & Contact Seal Subsystem

Purpose

Create a local, semi-closed cavity between heater head and regolith surface, so water vapor goes into the crawler instead of space.

Key Architectural Characteristics

Passive, compliant geometry (flaps, soft ring, or segmented edges) that can tolerate minor rocks and texture.

No requirement for perfect vacuum seal — “good enough” to direct most vapor into the capture path.

Materials selected for thermal survival near the heater but not carrying main heat flux.

Responsibilities

Conform to local terrain to minimize vapor loss.

Maintain sufficient mechanical engagement to prevent head slippage during heating.

Allow for vertical disengagement without snagging.

Interfaces

Mechanical attachment to heater head.

Implicit fluid interface with the regolith cavity and vapor inlets.

Invariants

Passive only. No active sealing rings, powered clamps, or motorized skirts in this architecture.

3.4 Vapor Capture & Ducting Subsystem

Purpose

Provide a preferential flow path for water vapor from the sealed regolith cavity into internal capture/condensation volumes.

Key Architectural Characteristics

Short, wide, low-impedance passages from the cavity to the capture volume.

Minimal geometry changes to reduce condensation in the wrong place.

Interior surfaces and routing designed so any condensate forming in the duct can drain into the capture volume, not pool in dead ends.

Responsibilities

Connect vapor inlets under the heater head to the Condenser & Capture Subsystem.

Maintain a one-way path (regolith → internal volume) during active heating.

Survive the temperature gradient between hot cavity and cooler interior.

Interfaces

Upstream: heater cavity / vapor ports.

Downstream: Condenser & Capture Subsystem.

Invariants

Passive flow; no reliance on high-speed pumps to “suck” vapor.

Any valves (if used) are simple, binary, and fail-safe (either “open” for heating or “closed” when not in use).

3.5 Condenser & Capture Subsystem

Purpose

Turn water vapor into stored liquid or solid water inside the crawler, in a form that downstream ISRU can claim later.

Key Architectural Characteristics

A colder-than-vapor surface or volume, relative to the heater cavity.

Geometry designed so condensate naturally collects in a single, serviceable reservoir or cartridge.

No attempt at purification — this is raw, dirty water consistent with the “dirty machine” role.

Responsibilities

Provide a condensation environment that encourages vapor-to-liquid/solid transition inside the crawler.

Store collected water in a way that supports later transfer (gravity dump, removable cartridge, or docking interface).

Avoid clogging, freezing-solid in the wrong places, or over-pressurization.

Interfaces

Inlet from Vapor Capture & Ducting.

Outlet to Storage & Transfer Interface Subsystem.

Structural + thermal connection to Power Subsystem (if using active refrigeration later — Phase-2 keeps it conceptual).

Invariants

Only one primary collection path; no complex multi-branch routing.

Failure mode is “less efficient collection” or “reduced capacity,” not “pressure bomb” or structural rupture.

3.6 Storage & Transfer Interface Subsystem

Purpose

Hold collected water until it can be handed off.

Provide a simple interface so other systems can retrieve water without redesigning the Crawler.

Key Architectural Characteristics

Single dominant storage volume (or a small number of repeated, identical cartridges).

Simple, dumb interface:

gravity dump port, and/or

latchable cartridge that another system can pull, and/or

mating port for a station-side pump.

Responsibilities

Track basic fill state (coarse — near empty / nominal / near full).

Prevent uncontrolled venting or leaks during transit.

Support repeated transfer cycles over the unit’s lifetime.

Interfaces

Internal plumbing from Condenser & Capture Subsystem.

External mechanical / fluid interface to base station or retrieval system.

Telemetry to Control Subsystem (fill state, valve status if any).

Invariants

The Crawler never tries to “process” or “refine” water. It just stores and hands off.

Any transfer operation is slow and controlled; no fast-dump assumptions.

3.7 Power Subsystem

Purpose

Provide electrical power for mobility, control, and heater operations within the defined envelope from Phase-1.

Define the baseline duty cycle feasible for long-term operation.

Key Architectural Characteristics

External recharge model (e.g., home base, charging pad, or tether) rather than full independence.

Battery and power bus sized for one or more heat cycles plus transit, then recharge.

No dynamic “boost” behavior; predictable power draw profiles.

Responsibilities

Distribute power to:

Heater Head

Control & Sensing

Mobility

Any small actuators (head lift, valves)

Enforce power prioritization: heater + safety + control get preference over mobility.

Interfaces

Physical and electrical interface to external recharge point.

Internal bus to all powered subsystems.

Status telemetry (SOC, faults) to Control Subsystem.

Invariants

The Crawler is designed to stop safely if power is low — no heroic last-minute burns.

3.8 Control & Sensing Subsystem

Purpose

Coordinate motion, head deployment, heating, capture, and transfer operations.

Provide enough sensing to not be stupid, but not enough to pretend it’s an exploration rover.

Key Architectural Characteristics

Simple, deterministic control logic (finite state machine) rather than complex autonomy.

Minimal sensor suite:

Position/attitude feedback sufficient for deployment

Contact/force sensing for head seating

Temperature monitoring (heater head, critical internal points)

Basic fill and pressure readings where necessary

Works with remote supervision (from a base or orbit) rather than requiring continuous human drive.

Responsibilities

Execute the state machine:

Idle / Transit

Pre-seat alignment

Head Deploy & Seal

Heat & Capture

Head Retract

Transit to Transfer Point

Hand-off / Dump

Fault / Safe

Enforce operational limits:

Temperature ceilings

Power ceilings

Pressure limits

Interfaces

Command & telemetry link to external operators / mission control.

Control signals to all powered subsystems.

Health/status reporting.

Invariants

System must be operable without AI.

Any AI or advanced planning that appears later is strictly an optional overlay that uses the same underlying state machine.

4. Internal Interfaces & Data Flows (High Level)
4.1 Mechanical Interfaces

Chassis ↔ Heater Head + Skirt: rigid mount plus vertical actuation.

Chassis ↔ Storage/Transfer: rigid mount, access for external mating.

Chassis ↔ Power: battery pack or power spine integrated into core structure.

4.2 Fluid/Material Interfaces

Regolith cavity → Vapor inlet ports → Ducting → Condenser → Storage.

Storage → External transfer interface (gravity dump, cartridge, or docking).

No regolith is supposed to enter the internal fluid path; any dust intrusion is considered a contamination/abrasion risk, not a feature.

4.3 Electrical & Control Interfaces

Power bus from Power Subsystem to all loads.

Control bus from Control & Sensing to:

Mobility actuators

Heater power control

Head actuator

Valves (if present)

Sensors (temp, pressure, contact, position, fill).

5. Operating States (Conceptual State Machine)

IDLE / SAFE

All actuators parked; heater off; head raised.

Waiting at base or in a safe loiter position.

TRANSIT

Slow movement to target patch or transfer point.

Heater disabled; head stowed.

PRE-SEAT ALIGNMENT

Short movements and/or minor adjustments to align heater head over target patch.

Check terrain slope, roughness.

HEAD DEPLOY & SEAL

Lower head + skirt onto surface.

Verify contact via force/position sensing.

Confirm readiness (power, temps, pressures sane).

HEAT & CAPTURE

Enable heater for a defined duty cycle.

Monitor thermal + pressure + power limits.

Vapor flows into condenser/capture system.

Abort if limits exceeded; transition to SAFE / FAULT.

HEAD RETRACT

Heater off; cool-down if needed.

Raise head to travel position.

TRANSIT TO TRANSFER POINT

Move to base or local transfer interface.

Monitor storage fill; prevent overfill.

HAND-OFF / DUMP

Execute transfer (dump, cartridge swap, or docking sequence).

Confirm storage reset to safe level.

FAULT / SAFE MODE

Any serious error transitions here.

Heaters off, head raised, mobility slowed or stopped.

Await higher-level decision (recover, retire, or ignore).

The implementation details of this state machine show up in later docs; Phase-2 only fixes the conceptual ladder.

6. Non-Claims & Deliberate Omissions

This architecture does not claim:

Specific production rates (kg/hour).

Specific depths of ice access.

Long-term structural survivability across full lunar thermal cycles.

Human-rating or crew interaction beyond safe standoff.

Any particular heater technology beyond “conical heater system” consistent with Phase-1.

Those are Phase-1 + Phase-3 questions.

7. Open Architecture Questions (Reserved for Later Phases)

These are acknowledged, not solved here:

Exact geometry and material stack of heater head and skirt.

Choice between purely passive condensation vs limited active cooling.

Best transfer mode (dump vs cartridge vs dock).

Optimal deployment topology (single crawler vs multiple units vs paired with a static hub).

Detailed interaction with specific ISRU chains (GEMS variants, others).

Phase-2’s job is to define the box; detailed answers belong to later work.
