# ivm-psc

PSC — Peripheral Sync Controller

## What it is

A single PCB that replaces three boards in the HYDRO ROV camera system: the PCI-to-RS-232 interface card, the eeWare synchronization trigger board, and the LM317 LED flash driver. It mounts directly on the Jetson AGX GPIO header (Samtec T1M-10-GF-DH on the Connecttech Rogue carrier) and is split into a fixed base board (100×100 mm) plus a variant-specific adapter board connected via a B2B connector.

## Problems solved

The old system had five failure modes at the 20 Hz operating rate required by ivm-backend:

1. The LM317 linear driver thermal-shuts-down at 20 Hz — efficiency is bounded by V_LED/V_supply, and for H300 (LED voltage above supply) a linear driver is simply impossible.
2. The cap bank was charged directly from the supply, producing large uncontrolled current spikes after each discharge.
3. The eeWare trigger MCU ran its own timer clock, independent of the Jetson. No feedback signal told the Jetson when a trigger actually fired or whether the cap bank was charged.
4. Flash pulse width was limited to integer milliseconds (0–5 ms) via RS-232 command.
5. RS-232 peripherals sharing a common return with the Jetson created ground loops causing corrupted serial frames from the IMU and altimeter.

## Repository layout

```
ivm-psc/
├── docs/        # Design documents, schematics, and board specs
├── reference/   # Datasheet PDFs and reference material
└── ltspice/     # LTspice simulation files
```
