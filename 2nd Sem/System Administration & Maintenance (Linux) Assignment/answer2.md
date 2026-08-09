## 2. Steps to Assemble a PC

### Phase 0 — Preparation and Safety

1. **Gather all components:** Motherboard (with matching CPU socket and chipset), CPU, CPU cooler, RAM (matching the board's DDR generation), storage (M.2 SSD and/or 2.5"/3.5" drives), GPU (if discrete graphics are needed), PSU, PC case, and peripherals.
2. **Verify compatibility in advance:** Confirm CPU ↔ motherboard socket (e.g., Intel LGA 1851 or AMD AM5), RAM generation (DDR4 vs DDR5), PSU wattage vs total system draw, and case form factor (ATX / Micro-ATX / Mini-ITX). Run the parts list through the motherboard manufacturer's **QVL (Qualified Vendor List)** or a compatibility checker.
3. **Gather tools:** Phillips-head screwdriver (magnetic tip recommended), anti-static wrist strap, thermal paste (if the cooler doesn't have pre-applied paste), zip ties, and isopropyl alcohol for cleaning.
4. **Safety precautions:**
   - Work on a **hard, non-static surface**; wear the **anti-static wrist strap** or touch the PSU metal case frequently to discharge static.
   - **Never force** any component — if it does not seat easily, check alignment.
   - Handle the CPU and motherboard by the edges only; avoid touching gold pins/contacts.
5. **Prepare the OS installer:** Download the OS (e.g., Windows 11 or a Linux distribution) and write it to a USB drive beforehand, and pre-download chipset/GPU/LAN drivers.

### Phase 1 — Motherboard Bench Build (outside the case)

**Step 1: Install the CPU**
- Open the **CPU socket retention arm** and the protective cover.
- Align the CPU using the **gold triangle** (corner marker) with the matching triangle on the socket.
- For **LGA (Intel)** sockets, drop the CPU in place without touching the pins; for **PGA (AMD AM5)**, align the pins with the socket holes.
- Lower the retention arm and lock it. **Do not apply force.**

**Step 2: Install the M.2 SSD (optional but recommended)**
- Locate the **M.2 slot** on the motherboard.
- Insert the M.2 drive at a **~30° angle** into the slot, then press it flat and secure it with the retention screw.
- Do this now, outside the case, because the small screw is easy to lose inside a chassis.

**Step 3: Install the RAM**
- Open the DIMM slot clips.
- Insert the modules using the **notch** as a guide, pressing down firmly until the clips **click** and lock.
- **Channel pairing matters:** for two modules in a four-slot board, use the slots recommended in the manual (usually slots 2 and 4 from the CPU) to enable **dual-channel** operation. Never mix different RAM speeds/timings if avoidable — the system will run at the slowest module's settings.

**Step 4: Install the CPU Cooler**
- If using a stock cooler, thermal paste is usually **pre-applied**; if not, apply a **pea-sized drop** of thermal paste to the center of the CPU (a small pea-sized dot is the correct amount — too much causes spillage, too little causes hotspots).
- Mount the cooler using the correct backplate/bracket for the socket, then **tighten screws in a cross pattern** (X-pattern) to ensure even pressure.
- Connect the cooler fan cable to the **CPU_FAN header** on the motherboard.

**Step 5: Bench Test (optional but highly recommended)**
- Place the motherboard on the box it came in (acts as an anti-static surface).
- Connect the **24-pin ATX** and **8-pin CPU EPS** power cables, plus a display.
- Short the two **power-switch front-panel pins** with a screwdriver (or use the motherboard power button if present) to test that the system **POSTs** before installing it in the case. This catches Dead-On-Arrival (DOA) parts early.

### Phase 2 — Case Preparation and Installation

**Step 6: Prepare the Case**
- Remove both side panels.
- Verify **standoffs** are installed in the correct positions matching the motherboard form factor. **This is critical** — a motherboard screwed directly onto the case (without standoffs) will short-circuit and can be destroyed.
- Install the **I/O shield** (if separate) into the rear opening, pushing it in from inside the case until it clicks.

**Step 7: Install the PSU**
- Mount the PSU in its bay (bottom-mounted on modern cases).
- **Fan orientation:** face the fan down toward the case's bottom ventilation if the case has a bottom vent; face it up only if the case has no bottom vent.
- Secure with the four screws from the outside of the case.
- Route the 24-pin, CPU EPS, PCIe, SATA, and Molex cables to their approximate destinations before tightening cable management.

**Step 8: Install the Motherboard into the Case**
- Lower the motherboard into the case, aligning the rear I/O ports with the I/O shield cutouts.
- Ensure the mounting holes line up with the standoffs.
- Screw the motherboard in using the supplied motherboard screws (usually 6–9), tightening gently to avoid cracking the board.

**Step 9: Install Storage Drives (SATA HDD/SSD)**
- Mount 3.5" HDDs or 2.5" SSDs into the case's drive bays using the drive trays.
- Connect a **SATA data cable** from the drive to a motherboard SATA port, and a **SATA power cable** from the PSU.
- (M.2 drives were already installed in Step 2.)

**Step 10: Install the GPU (and other PCIe cards)**
- Remove the appropriate **expansion slot covers** (brackets) at the rear of the case for the number of slots the GPU occupies.
- Align the GPU's PCIe connector with the **PCIe x16 slot**, press down until it clicks, and secure it to the case with the slot screws.
- Connect the **PCIe power cables** (6-pin / 8-pin / 12VHPWR as required) from the PSU.
- Install any other expansion cards (Wi-Fi, sound, capture cards) in the same manner.

### Phase 3 — Wiring and Cable Management

**Step 11: Connect Power Cables** (in this order)
1. **24-pin ATX** → motherboard main power connector.
2. **8-pin (or 4+4) CPU EPS** → CPU power connector near the socket (some high-end boards need two).
3. **PCIe cables** → GPU.
4. **SATA power** → drives.
5. **Molex** → optional fans/accessories.

**Step 12: Connect Front-Panel Headers and Fans**
- **Front-panel headers:** power button, reset button, power LED, HDD LED, front USB 2.0/3.0, and audio — connect each to the labeled motherboard headers using the case manual.
- **Case fans:** connect to the **CHA_FAN** / **SYS_FAN** headers (with PWM support for variable speed control).

**Step 13: Cable Management**
- Route cables behind the motherboard tray using the cutouts and zip ties.
- Keep cables clear of fans and the CPU cooler airflow path for proper cooling.

### Phase 4 — First Boot and Post-Build Verification

**Step 14: Pre-Boot Checks**
- Re-verify every connector is fully seated (especially the 24-pin and 8-pin CPU power — a forgotten CPU power cable is one of the most common "won't boot" causes).
- Ensure RAM is in the correct slots and fully locked.
- Confirm standoffs were used and nothing metal is touching the motherboard.

**Step 15: First Boot (POST)**
- Power on the system.
- **Successful POST:** single beep (on boards with a speaker) and a BIOS/UEFI splash screen. Use any **debug LEDs** / POST code display if the board has them.
- Enter BIOS/UEFI (usually **Del** or **F2**) and verify: CPU detected with normal temperature; all RAM detected; storage detected; boot order correct.
- Enable **XMP/EXPO** (memory overclock profile) so the RAM runs at its rated speed.

**Step 16: Install the Operating System and Drivers**
- Boot from the USB installer.
- Install the OS, then install **chipset, GPU, LAN/audio, and storage** drivers from the manufacturers' websites.
- Run **Windows Update** or the distro's package updates to apply security patches.

**Step 17: Final Stability Testing**
- Run a CPU stress test (e.g., Cinebench) and a GPU stress test (e.g., 3DMark/FurMark) for 15–30 minutes.
- Monitor temperatures with software (e.g., HWiNFO) to confirm the cooling system is adequate.
- Confirm all drives, RAM, and devices are recognized, then close the case and finish cable tidying.

### Common Assembly Mistakes to Avoid

| Mistake | Consequence |
|---|---|
| Missing motherboard standoffs | Electrical short → component damage |
| Forgetting the 8-pin CPU power cable | System powers on but no display/beep |
| RAM in wrong slots (single channel) | Reduced memory performance |
| Too much/too little thermal paste | Overheating or paste overflow |
| Forcing a component | Bent pins / cracked board |
| CPU fan plugged into wrong header | CPU overheating and throttling |
| Not grounding (static) | ESD damage to sensitive chips |

### Summary of the Assembly Sequence

1. Prepare workspace, tools, and verify component compatibility
2. Install CPU → M.2 SSD → RAM → CPU cooler on the motherboard (bench build)
3. Bench test the motherboard stack (optional)
4. Prepare the case: I/O shield + standoffs
5. Mount PSU → motherboard → SATA drives → GPU
6. Connect 24-pin ATX, 8-pin EPS, PCIe, SATA power
7. Connect front-panel headers and fans
8. Manage cables
9. First boot → check BIOS detection → enable XMP/EXPO
10. Install OS + drivers → stress test for thermal stability

---
