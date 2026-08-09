## 1. Internal Components of a PC

### 1. Motherboard (Mainboard / System Board)

**Definition:** The motherboard is the main printed circuit board (PCB) inside the PC. It is the central platform that physically and electrically connects every internal component.

**Functions:**
- Provides the **CPU socket**, **DIMM (RAM) slots**, **PCIe expansion slots**, **M.2 / SATA storage connectors**, and **I/O ports**.
- Distributes **power** from the PSU to all attached components.
- Houses the **chipset**, which manages data flow between the CPU, memory, storage, and peripherals.
- Contains the **BIOS/UEFI firmware chip** that initialises hardware during startup.
- Carries the **system buses** (data/address/control) that allow components to communicate.

**Key sub-components worth noting:** VRM (Voltage Regulator Module) for stable CPU power delivery, CMOS battery, front-panel headers, fan headers, and the 24-pin ATX + 8-pin EPS power connectors.

### 2. CPU (Central Processing Unit / Processor)

**Definition:** The processor is the "brain" of the computer — the primary computing engine.

**Functions:**
- **Fetches** instructions from RAM, **decodes** them, and **executes** them (fetch–decode–execute cycle).
- Performs **arithmetic and logic** operations via its internal ALU (Arithmetic Logic Unit).
- **Coordinates** and controls all other hardware through its Control Unit (CU).
- Uses high-speed **registers** and **cache memory** (L1/L2/L3) to speed up operations.

**Performance factors:** number of **cores and threads**, **clock speed** (measured in GHz), **cache size**, and thermal design (TDP). On modern platforms, socket types such as **LGA 1851 (Intel)** and **AM5 (AMD)** determine motherboard compatibility.

### 3. RAM (Random Access Memory) — Main Memory

**Definition:** RAM is the computer's volatile, high-speed working memory.

**Functions:**
- Temporarily stores the **data and instructions** the CPU needs while executing programs.
- Acts as the bridge between the CPU and the slower storage drives.
- Because it is **volatile**, all contents are lost when power is removed — hence it is "working" memory, not permanent storage.

**Key points:**
- Speed and capacity (GB) directly affect multitasking and application performance.
- Modern types are **DDR4** and **DDR5** (not interchangeable).
- Motherboards support **dual-channel** (or quad-channel) operation, which increases memory bandwidth.

### 4. Storage Devices — Secondary Storage

**Definition:** Storage provides long-term, non-volatile retention of the operating system, software, and user data.

**HDD (Hard Disk Drive):**
- Stores data **magnetically** on spinning platters coated with ferromagnetic material; read/write heads move across the surface.
- **Advantages:** low cost per GB, very large capacities, suitable for backups and archiving.
- **Disadvantages:** slow (typically 50–150 MB/s), mechanical, fragile to physical shock, higher power and heat.

**SSD (Solid State Drive):**
- Stores data in **NAND flash memory** chips with no moving parts.
- **Advantages:** very fast (SATA ~500 MB/s; NVMe up to ~7000 MB/s), durable, low power, near-instant boot times.
- **Disadvantages:** more expensive per GB; flash has a finite write lifespan (mitigated by wear-levelling and TRIM).

**Practical note for sysadmins:** the boot/OS drive is almost always an SSD for performance; HDDs remain popular for cheap bulk storage and NAS/backup arrays.

### 5. GPU (Graphics Processing Unit) — Graphics Card

**Definition:** The GPU is responsible for rendering images, video, and 3D graphics, and for accelerating parallel computation.

**Functions:**
- **Offloads graphics and display work** from the CPU, converting data into signals the monitor can display.
- Contains its own **VRAM (video memory)** for textures and frame buffers.
- Used heavily in **gaming, video editing, CAD, and AI/ML workloads**.

**Two forms:**
- **Dedicated (discrete) GPU:** a separate PCIe x16 expansion card with its own power connectors — required for high-end graphics.
- **Integrated GPU:** graphics built into the CPU — adequate for office work and basic display output, and typically uses shared system RAM.

### 6. PSU (Power Supply Unit)

**Definition:** The PSU converts mains **AC** power into the **regulated low-voltage DC** power that every internal component requires.

**Functions (four core roles):**
1. **AC → DC conversion:** wall outlets supply 110/220V AC; components need 12V, 5V, and 3.3V DC.
2. **Voltage regulation:** keeps voltages stable even under sudden load changes (e.g., GPU power spikes) to prevent instability and hardware damage.
3. **Power distribution:** feeds each component through dedicated connectors (24-pin ATX to the motherboard, 8-pin EPS to the CPU, PCIe/SATA/Molex for drives and GPUs).
4. **System protection:** over-voltage, over-current, over-temperature, and short-circuit protection safeguards all other components.

**Selection factors:** wattage rating, form factor (ATX/SFX/TFX), modular vs non-modular cabling, and **80 PLUS efficiency certification** (Bronze/Silver/Gold/Platinum/Titanium).

### 7. Cooling System

**Definition:** Cooling manages the heat generated by the CPU, GPU, and other high-power components so they run within safe temperature limits.

**Functions:**
- **Air cooling:** a heatsink (finned metal block) absorbs heat, and a fan dissipates it; thermal paste fills the micro-gaps between the CPU and heatsink for efficient transfer.
- **Liquid cooling:** coolant circulated by a pump through a radiator removes heat more efficiently (AIO or custom loops).
- **Case airflow:** intake and exhaust fans move cool air through the chassis, supported by good cable management.

**Why it matters:** overheating causes **thermal throttling** (performance loss) and long-term hardware degradation; failed cooling is one of the most common causes of sudden system instability.

### 8. Expansion Cards

**Definition:** Add-on circuit boards installed into PCIe slots that extend a system's capabilities.

**Examples and functions:**
- **Sound card:** higher-quality audio input/output.
- **Network card (NIC) / Wi-Fi adapter:** additional or faster network connectivity.
- **Capture card:** records/streams video from external sources.
- **USB/Thunderbolt controller cards:** extra ports and faster transfer speeds.

### 9. BIOS / UEFI Firmware Chip

**Definition:** Firmware stored on a chip on the motherboard that runs before the operating system.

**Functions:**
- Performs the **Power-On Self-Test (POST)** — checks that CPU, RAM, storage, and fans are detected and functional.
- Provides **boot order** control and loads the **bootloader** that starts the OS.
- Hosts hardware configuration settings (clock speeds, virtualization, Secure Boot, TPM).
- **UEFI** is the modern successor to the legacy **BIOS**, supporting secure boot, larger disks (GPT), and a graphical interface.

### 10. CMOS Battery

**Definition:** A small coin-cell (CR2032) battery mounted on the motherboard.

**Functions:**
- Keeps the **real-time clock** and BIOS/UEFI settings (e.g., boot order, date/time) alive when the PC is unplugged.
- A dead CMOS battery causes time/date resets and can cause settings loss — a common troubleshooting item in system maintenance.

### 11. Chassis (Case) + Front-Panel Connectors

**Definition:** The enclosure that physically houses all internal components.

**Functions:**
- **Protection and mounting** for all hardware; its size determines compatible form factors (ATX, Micro-ATX, Mini-ITX).
- Provides the **power button, reset button, status LEDs, front USB and audio ports**, which connect to the motherboard via front-panel headers.
- Supports **airflow design** (intake/exhaust mounting points) for the cooling system.

### Summary Table — Internal Components

| Component | Primary Function |
|---|---|
| Motherboard | Connects and coordinates all components |
| CPU | Executes instructions and controls the system |
| RAM | Temporary high-speed working memory |
| HDD/SSD | Permanent storage of OS, software, and data |
| GPU | Renders graphics and accelerates parallel workloads |
| PSU | Converts and distributes power to all components |
| Cooling | Prevents overheating and throttling |
| Expansion cards | Add networking, audio, and other capabilities |
| BIOS/UEFI | Initialises hardware and loads the OS |
| CMOS battery | Preserves settings and clock when powered off |
| Case | Houses, protects, and cools the components |

---
