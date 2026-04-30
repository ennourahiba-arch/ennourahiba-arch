# 🚀 RISC-V Pipelining - Complete Guide (English)

## Table of Contents
1. [Introduction to Pipelining](#introduction)
2. [RISC-V Pipeline Architecture](#architecture)
3. [Stalling (Pipeline Blocking)](#stalling)
4. [Forwarding Unit](#forwarding)
5. [Comparisons and Summaries](#comparisons)
6. [Formulas and Calculations](#formulas)

---

## <a name="introduction"></a>1. Introduction to Pipelining

### What is Pipelining?

**Pipelining** is an optimization technique that allows a processor to execute **multiple instructions simultaneously** by dividing them into stages.

### Analogy: Assembly Line

```
WITHOUT PIPELINING (Sequential):
Car 1 : Paint → Assemble → Test → Ship
Car 2 :                              Paint → Assemble → Test → Ship
```

```
WITH PIPELINING (Parallel):
Car 1 : Paint → Assemble → Test → Ship
Car 2 :         Paint  → Assemble → Test → Ship
Car 3 :                  Paint  → Assemble → Test → Ship
```

**Result**: Better instruction throughput! 🚀

---

## <a name="architecture"></a>2. RISC-V Pipeline Architecture

### The 5 Pipeline Stages

```
┌─────────────────────────────────────────────────────────────┐
│                  RISC-V 5-STAGE PIPELINE                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  IF        ID        EX        MEM       WB                 │
│  ──        ──        ──        ───       ──                 │
│ Fetch    Decode   Execute   Memory    Write Back            │
│  ↓         ↓         ↓         ↓         ↓                   │
│ Fetch    Decode   ALU Op    Memory     Write                │
│ Instr    Instr             Access      Result               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Details of Each Stage

| Stage | Acronym | Role | Resource |
|-------|---------|------|----------|
| **Instruction Fetch** | IF | Load instruction from memory | Instruction Memory |
| **Instruction Decode** | ID | Decode and read registers | Registers |
| **Execute** | EX | Execute ALU operation | ALU |
| **Memory Access** | MEM | Access memory (Load/Store) | Data Memory |
| **Write Back** | WB | Write result to registers | Registers |

---

### Simple Timeline (3 Instructions)

```
Instruction 1: LW x1, 0(x2)
Instruction 2: ADD x3, x1, x4
Instruction 3: SW x3, 0(x5)

Cycle  1   2   3   4   5   6   7
────────────────────────────────────
LW    IF  ID  EX  MEM WB
ADD       IF  ID  EX  MEM WB
SW            IF  ID  EX  MEM WB

📊 Result: 7 cycles (instead of 15 without pipelining)
```

---

### PC (Program Counter) in Pipeline

The **PC** indicates the address of the next instruction to fetch.

```
┌─────────────────────────────────────────┐
│          PC PROGRESSION                 │
├─────────────────────────────────────────┤
│                                          │
│ Cycle 1: PC = 0x0000                    │
│          IF fetches instruction at 0x0000
│          PC = PC + 4 (RISC-V: 32 bits)  │
│                                          │
│ Cycle 2: PC = 0x0004                    │
│          IF fetches instruction at 0x0004
│          PC = PC + 4                     │
│                                          │
│ Cycle 3: PC = 0x0008                    │
│          ...                             │
│                                          │
└─────────────────────────────────────────┘
```

---

### Pipeline Registers (Latches)

Data moves between stages through pipeline registers:

```
IF/ID    → Stores instruction from IF stage
ID/EX    → Stores operands and decoded instruction
EX/MEM   → Stores ALU result
MEM/WB   → Stores data read from memory
```

**Example:**
```
Cycle 1: LW loads in IF
         IF/ID ← LW

Cycle 2: LW moves to ID
         ID/EX ← LW info (read registers, etc)

Cycle 3: LW moves to EX
         EX/MEM ← calculated memory address

Cycle 4: LW moves to MEM
         MEM/WB ← value read from memory

Cycle 5: LW moves to WB
         x1 ← value (finally written!)
```

---

## <a name="stalling"></a>3. Stalling (Pipeline Blocking)

### What is Stalling?

**Stalling** = **temporarily stop** part of the pipeline because we cannot continue.

It's a bottleneck at the processor level. 🛑

---

### When Do We Need Stalling?

**Problem**: An instruction needs data that isn't ready yet.

```
LW x1, 0(x2)    # Load x1 (result in MEM at cycle 4)
ADD x3, x1, x4  # Use x1 (needs x1 in EX at cycle 3)

❌ CONFLICT: ADD needs x1 before LW has loaded it!
```

---

### Timeline with Stalling

```
Cycle  1   2   3   4   5   6   7   8   9
────────────────────────────────────────────
LW    IF  ID  EX  MEM WB
ADD       IF  ID  ⏸️  ⏸️  EX  MEM WB
SUB           IF  ⏸️  ⏸️  ⏸️  ID  EX  MEM

⏸️ = Bubble (NOP - No Operation)

2 cycles of waiting for ADD
```

---

### How Stalling Works?

#### Step 1: Hazard Detection

**Hazard Detection Unit** checks:
```
If (ID/EX.MemRead == 1) AND 
   ((ID/EX.Rd == IF/ID.Rs1) OR (ID/EX.Rd == IF/ID.Rs2))
   
Then: STALL DETECTED!
```

**Example:**
```
LW x1, 0(x2)     ← ID/EX.MemRead = 1, ID/EX.Rd = x1
ADD x3, x1, x4   ← IF/ID.Rs1 = x1
                    MATCH! → STALL!
```

#### Step 2: Stall Action

```
┌─────────────────────────────────────────┐
│       ACTIONS DURING A STALL            │
├─────────────────────────────────────────┤
│                                          │
│ IF STAGE:  Continue fetching (normal)   │
│                                          │
│ ID STAGE:  PC doesn't change ❌        │
│            IF/ID doesn't change ❌     │
│            (Instruction stays blocked)  │
│                                          │
│ EX STAGE:  Continue normally            │
│ MEM STAGE: Continue normally            │
│ WB STAGE:  Continue normally            │
│                                          │
└─────────────────────────────────────────┘
```

---

### Stalling RTL Code

```verilog
// Hazard Detection Unit
if (ID_EX_MemRead && 
    ((ID_EX_RegisterRd == IF_ID_RegisterRs1) || 
     (ID_EX_RegisterRd == IF_ID_RegisterRs2))) {
    
    // INSERT STALL
    stall = 1;
    
    // Freeze the PC (don't advance)
    PC_write = 0;  // PC stays the same
    
    // Freeze the IF/ID register
    IF_ID_write = 0;  // IF/ID stays the same
    
    // Insert a NOP in ID/EX
    ID_EX = 0;  // Empty bubble
}
else {
    stall = 0;
    PC_write = 1;
    IF_ID_write = 1;
    ID_EX = normal_data;
}
```

---

### Complete Example: Load-Use Hazard

```
RISC-V Code:
    LW x1, 0(x2)
    ADD x3, x1, x4
    SUB x5, x6, x7

WITHOUT STALLING (❌ WRONG):
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]      ← ADD reads x1 (not ready!)
Cycle 4: LW [MEM]   ADD [EX]      ← x1 = 42 (too late!)
Cycle 5: LW [WB]    ADD [MEM]
         Registres[x1] = 42

❌ Result: ADD uses wrong value of x1


WITH STALLING (✅ CORRECT):
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]      ← Stall detected!
Cycle 4: LW [MEM]   ADD [⏸️]      ← ADD stays blocked
Cycle 5: LW [WB]    ADD [ID]      ← x1 = 42 (finally ready!)
Cycle 6:            ADD [EX]
Cycle 7:            ADD [MEM]
Cycle 8:            ADD [WB]

✅ Result: ADD uses correct value x1 = 42
```

---

### Stalling Performance Impact

```
╔════════════════════════════════════════════════════════════╗
║         STALLING IMPACT ON PERFORMANCE                     ║
╠════════════════════════════════════════════════════════════╣
║                                                             ║
║ Number of instructions : 100                               ║
║ Load-Use Hazards : 20 (each = 1 stall)                    ║
║                                                             ║
║ CPI without stall : 1.0 (1 instr per cycle)                ║
║ CPI with stall : 1.2 (20% slower)                          ║
║                                                             ║
║ Slowdown = 1.2x                                             ║
║                                                             ║
╚════════════════════════════════════════════════════════════╝
```

---

## <a name="forwarding"></a>4. Forwarding Unit (Bypass)

### What is Forwarding?

**Forwarding** = Instead of waiting for the result to be written to registers, we **divert it directly** to where it's needed.

It's a shortcut! ⚡

---

### The Problem Without Forwarding

```
LW x1, 0(x2)    # Load x1 (result in MEM at cycle 4)
ADD x3, x1, x4  # Use x1 (needs x1 in EX at cycle 3)

❌ Without Forwarding + Without Stalling:
ADD uses wrong value (error!)

❌ Without Forwarding + With Stalling:
2 cycles lost (slow!)
```

---

### The Solution: Forwarding Unit

```
┌──────────────────────────────────────────────────────────┐
│          FORWARDING UNIT ARCHITECTURE                    │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  EX STAGE (Execute)                                      │
│  ┌──────────────────────────────────────────┐            │
│  │  Sources of operands for ALU:            │            │
│  │                                            │            │
│  │  Option 1: Registers[Rs1] (normal)       │            │
│  │           ForwardAE = 00                  │            │
│  │                                            │            │
│  │  Option 2: EX/MEM.ALUResult (MEM stage)  │            │
│  │           ForwardAE = 01  ← Forward!     │            │
│  │                                            │            │
│  │  Option 3: MEM/WB.ALUResult (WB stage)   │            │
│  │           ForwardAE = 10  ← Forward!     │            │
│  │                                            │            │
│  └──────────────────────────────────────────┘            │
│                                                           │
│  Multiplexer (3:1) selects correct source                │
│                                                           │
└──────────────────────────────────────────────────────────┘
```

---

### Forwarding Rules

#### **Rule 1: EX Hazard (Forward from MEM)**

```
If:
  EX/MEM.RegWrite = 1  (MEM instruction writes a register)
  EX/MEM.Rd ≠ 0        (not x0)
  EX/MEM.Rd == ID/EX.Rs1  (match on source)

Then:
  ForwardAE = 01  (use EX/MEM.ALUResult)

Example:
  LW x1, 0(x2)       ← Will write to x1
  ADD x3, x1, x4     ← Reads x1
           ↑ ↑ Match → Forward from MEM!
```

#### **Rule 2: MEM Hazard (Forward from WB)**

```
If:
  MEM/WB.RegWrite = 1
  MEM/WB.Rd ≠ 0
  MEM/WB.Rd == ID/EX.Rs1 OR ID/EX.Rs2
  AND Rule 1 didn't match

Then:
  ForwardAE/BE = 10  (use MEM/WB.ALUResult)

Example:
  ADD x1, x2, x3       ← Writes x1 in WB
  ...
  SUB x5, x1, x6       ← Reads x1 in EX
           ↑ ↑ Match → Forward from WB!
```

---

### Forwarding RTL Code

```verilog
// Forwarding Unit
module ForwardingUnit (
    input [4:0] ID_EX_Rs1, ID_EX_Rs2,
    input [4:0] EX_MEM_Rd, MEM_WB_Rd,
    input EX_MEM_RegWrite, MEM_WB_RegWrite,
    output [1:0] ForwardAE, ForwardBE
);

// ForwardAE for operand A
always @(*) begin
    if (EX_MEM_RegWrite && EX_MEM_Rd != 0 && 
        EX_MEM_Rd == ID_EX_Rs1) begin
        ForwardAE = 2'b01;  // From MEM
    end
    else if (MEM_WB_RegWrite && MEM_WB_Rd != 0 && 
             MEM_WB_Rd == ID_EX_Rs1) begin
        ForwardAE = 2'b10;  // From WB
    end
    else begin
        ForwardAE = 2'b00;  // From registers (normal)
    end
end

// ForwardBE for operand B (same logic)
always @(*) begin
    if (EX_MEM_RegWrite && EX_MEM_Rd != 0 && 
        EX_MEM_Rd == ID_EX_Rs2) begin
        ForwardBE = 2'b01;  // From MEM
    end
    else if (MEM_WB_RegWrite && MEM_WB_Rd != 0 && 
             MEM_WB_Rd == ID_EX_Rs2) begin
        ForwardBE = 2'b10;  // From WB
    end
    else begin
        ForwardBE = 2'b00;  // From registers (normal)
    end
end

endmodule
```

---

### Timeline with Forwarding

```
Code:
    LW x1, 0(x2)
    ADD x3, x1, x4

WITHOUT FORWARDING (❌ 2 stalls):
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]        ← Stall detected
Cycle 4: LW [MEM]   ADD [⏸️]        ← Stall
Cycle 5: LW [WB]    ADD [ID]
Cycle 6:            ADD [EX]
Cycle 7:            ADD [MEM]
Cycle 8:            ADD [WB]

⏹️ Total: 8 cycles (SLOW!)


WITH FORWARDING (✅ 0 stalls):
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]
Cycle 4: LW [MEM]   ADD [EX]  ← Forward from MEM
         42  ↓
         ADD receives 42 immediately!
Cycle 5: LW [WB]    ADD [MEM]
Cycle 6:            ADD [WB]

✅ Total: 6 cycles (FAST!)
```

---

### When Forwarding Doesn't Work

#### ⚠️ Load-Use Hazard (only case requiring stall)

```
LW x1, 0(x2)      # Load x1 (result in MEM at cycle 4)
ADD x3, x1, x4    # Use x1 in EX at cycle 3

Timeline:
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]    ← ADD needs x1 NOW
Cycle 4: LW [MEM]   ADD [EX]    ← x1 arrives in MEM (too late!)

❌ PROBLEM: x1 arrives AFTER ADD needs it
   Forwarding can't help (x1 doesn't exist yet!)

Solution: Stall + NOP
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]    ← Stall detected
Cycle 4: LW [MEM]   NOP [⏸️]
Cycle 5: LW [WB]    ADD [ID]    ← Now we can continue
Cycle 6:            ADD [EX]    ← x1 is available!
```

---

### Forwarding Performance Impact

```
╔════════════════════════════════════════════════════════════╗
║       FORWARDING IMPACT ON PERFORMANCE                     ║
╠════════════════════════════════════════════════════════════╣
║                                                             ║
║ Number of instructions : 100                               ║
║ Total dependencies : 50                                    ║
║  - Load-Use Hazards : 5  (require stall)                   ║
║  - Others : 45 (forwarding sufficient)                     ║
║                                                             ║
║ CPI without optimization : 1.5 (50% slower)                ║
║ CPI with stalling only : 1.2 (20% slower)                  ║
║ CPI with forwarding : 1.05 (5% slower)                     ║
║                                                             ║
║ Forwarding = 4x better than stalling alone! ⚡            ║
║                                                             ║
╚════════════════════════════════════════════════════════════╝
```

---

## <a name="comparaisons"></a>5. Comparisons and Summaries

### Comparison: Nothing vs Stalling vs Forwarding

```
Scenario: LW x1, 0(x2)  →  ADD x3, x1, x4

╔═══════════════════════════════════════════════════════════════════╗
║                    NO OPTIMIZATION (❌)                          ║
╠═══════════════════════════════════════════════════════════════════╣
║ Cycle 1: LW [IF]                                                  ║
║ Cycle 2: LW [ID]    ADD [IF]                                      ║
║ Cycle 3: LW [EX]    ADD [ID]    ← ADD reads x1 (not ready!)     ║
║ Cycle 4: LW [MEM]   ADD [EX]    ← ERROR! Wrong value            ║
║ Cycle 5: LW [WB]    ADD [MEM]                                     ║
║ Cycle 6:            ADD [WB]                                      ║
║                                                                    ║
║ Total: 6 cycles, RESULT WRONG ❌                                 ║
╚═══════════════════════════════════════════════════════════════════╝

╔═══════════════════════════════════════════════════════════════════╗
║            WITH STALLING (Correct but slow)                       ║
╠═══════════════════════════════════════════════════════════════════╣
║ Cycle 1: LW [IF]                                                  ║
║ Cycle 2: LW [ID]    ADD [IF]                                      ║
║ Cycle 3: LW [EX]    ADD [ID]    ← Stall detected                 ║
║ Cycle 4: LW [MEM]   NOP [⏸️]    ← 1st stall                      ║
║ Cycle 5: LW [WB]    ADD [ID]    ← x1 = 42 ready!                 ║
║ Cycle 6:            ADD [EX]                                      ║
║ Cycle 7:            ADD [MEM]                                     ║
║ Cycle 8:            ADD [WB]                                      ║
║                                                                    ║
║ Total: 8 cycles, RESULT CORRECT ✅ (but 33% slower)             ║
╚═══════════════════════════════════════════════════════════════════╝

╔═══════════════════════════════════════════════════════════════════╗
║         WITH FORWARDING (Correct and fast) ⚡                    ║
╠═══════════════════════════════════════════════════════════════════╣
║ Cycle 1: LW [IF]                                                  ║
║ Cycle 2: LW [ID]    ADD [IF]                                      ║
║ Cycle 3: LW [EX]    ADD [ID]                                      ║
║ Cycle 4: LW [MEM]   ADD [EX]    ← Forward from MEM               ║
║         42  ↓                                                      ║
║         ADD receives 42 immediately!                              ║
║ Cycle 5: LW [WB]    ADD [MEM]                                     ║
║ Cycle 6:            ADD [WB]                                      ║
║                                                                    ║
║ Total: 6 cycles, RESULT CORRECT ✅ (FAST!)                      ║
╚═══════════════════════════════════════════════════════════════════╝
```

---

### Types of Hazards

```
╔════════════════════════════════════════════════════════════╗
║                   TYPES OF HAZARDS                         ║
╠════════════════════════════════════════════════════════════╣
║                                                             ║
║ 1. DATA HAZARD (Data dependency conflict)                  ║
║    ───────────────────────────────────────                 ║
║    Instruction A writes to a register                      ║
║    Instruction B reads that same register                  ║
║                                                             ║
║    Example:                                                ║
║      ADD x1, x2, x3                                        ║
║      SUB x4, x1, x5  ← Depends on x1                      ║
║                                                             ║
║    Solutions:                                              ║
║      • Forwarding (if data available)                      ║
║      • Stalling (if load-use hazard)                       ║
║                                                             ║
║ ─────────────────────────────────────────────────────────  ║
║                                                             ║
║ 2. CONTROL HAZARD (Branch conflict)                        ║
║    ────────────────────────────────────                    ║
║    Branch decides where to go in EX                        ║
║    But we're already fetching next instr in IF             ║
║                                                             ║
║    Example:                                                ║
║      BEQ x1, x2, label  ← We don't know where to go       ║
║      ADD x3, x4, x5     ← Which instr to fetch?           ║
║                                                             ║
║    Solutions:                                              ║
║      • Branch Prediction                                   ║
║      • Pipeline Flush                                      ║
║      • Branch Delay Slots                                  ║
║                                                             ║
║ ─────────────────────────────────────────────────────────  ║
║                                                             ║
║ 3. STRUCTURAL HAZARD (Resource conflict)                   ║
║    ────────────────────────────────────────                ║
║    2 instructions need the same resource                   ║
║    at the same time                                        ║
║                                                             ║
║    Example:                                                ║
║      2 instructions want to access memory                  ║
║      but there's only one memory port                      ║
║                                                             ║
║    Solution:                                               ║
║      • Dual-port architecture (2 parallel accesses)        ║
║      • Stalling (rare in well-designed RISC-V)            ║
║                                                             ║
╚════════════════════════════════════════════════════════════╝
```

---

### Summary Table

| Technique | Speed | Complexity | Efficiency | Usage |
|-----------|-------|-----------|-----------|-------|
| **None** | ⚡⚡⚡ | Simple | ❌ Wrong Results | Not used |
| **Stalling** | ⏸️ | Medium | ⚠️ Correct | Rare (LU hazard) |
| **Forwarding** | ⚡⚡ | Complex | ✅ Optimal | Standard RISC-V |
| **Branch Pred.** | ⚡⚡⚡ | Very Complex | ✅ Excellent | Modern CPUs |

**Modern RISC-V = Forwarding + Stalling (for Load-Use) + Branch Prediction**

---

## <a name="formulas"></a>6. Formulas and Calculations

### CPI (Cycles Per Instruction)

```
CPI = Total number of cycles / Number of instructions

Example:
  5 instructions in 7 cycles = 7/5 = 1.4 CPI
  
  Interpretation:
  CPI = 1.0 : On average, 1 instruction per cycle (ideal)
  CPI = 1.5 : On average, 1.5 cycles per instruction (slow)
```

### Performance with and without Stalling

```
Case 1: Without optimization (systematic stalling)
──────────────────────────────────────────────────
N instructions
H hazards (each = 1 stall)

CPI = (N + H) / N

Example: 100 instr, 20 hazards
CPI = 120 / 100 = 1.2

Speedup = CPI_without / CPI_with
```

### Speedup with Forwarding

```
Case 2: With Forwarding (stalling only for Load-Use)
──────────────────────────────────────────────────────
N instructions
LU Load-Use hazards (each = 1 stall)

CPI = (N + LU) / N

Example: 100 instr, 5 Load-Use
CPI = 105 / 100 = 1.05

Improvement vs stalling alone = 1.2 / 1.05 = 1.14x faster
```

### Instruction Latency

```
Latency of LW (Load Word):
─────────────────────────
IF → ID → EX → MEM → WB = 5 cycles
(or 4 if counting from start)

So a dependent instruction must wait:
- Without forwarding: wait until WB (5 cycles)
- With forwarding: wait until MEM (3 cycles)
- Gain: 2 cycles saved!
```

### General Formula

```
Execution Time = Number of instructions × CPI × Cycle time

Example:
  100 instr, CPI=1.5, cycle=2ns
  Time = 100 × 1.5 × 2ns = 300ns

With optimization (CPI=1.05):
  Time = 100 × 1.05 × 2ns = 210ns
  
  Speedup = 300ns / 210ns = 1.43x faster
```

---

## Complete Visual Summary

```
┌─────────────────────────────────────────────────────────────┐
│            COMPLETE RISC-V PIPELINE                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  INSTRUCTION FLOW:                                          │
│  ─────────────────                                          │
│  Instruction Memory                                         │
│       ↓                                                      │
│  IF Stage (fetch instr at PC)                               │
│       ↓ (PC += 4)                                           │
│  ID Stage (decode, read registers)                          │
│       ↓                                                      │
│  EX Stage (ALU, forwarding unit here)                       │
│       ↓                                                      │
│  MEM Stage (Load/Store)                                     │
│       ↓                                                      │
│  WB Stage (write register)                                  │
│       ↓                                                      │
│  Registers                                                  │
│                                                              │
│  HAZARDS DETECTED:                                          │
│  ─────────────────                                          │
│  • Data Hazard → Forwarding or Stalling                     │
│  • Control Hazard → Branch Prediction                       │
│  • Structural Hazard → Architecture or Stalling             │
│                                                              │
│  OPTIMIZATIONS:                                             │
│  ───────────────                                            │
│  1. Forwarding Unit → Diverts results                       │
│  2. Hazard Detection Unit → Detects and stalls              │
│  3. Branch Predictor → Predicts branches                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Checklist for Class

- [ ] **Pipelining**: Concept and advantages
- [ ] **5 stages**: IF, ID, EX, MEM, WB
- [ ] **PC**: How it progresses
- [ ] **Data Hazards**: Types and causes
- [ ] **Stalling**: When and how
- [ ] **Forwarding**: Rules and architecture
- [ ] **Load-Use Hazard**: Special case
- [ ] **CPI**: Calculation and interpretation
- [ ] **Performance**: Comparisons and speedup
- [ ] **Pipeline Registers**: IF/ID, ID/EX, EX/MEM, MEM/WB

---

## Review Questions

### Level 1 (Basic)
1. How many stages in a standard RISC-V pipeline? **Answer: 5**
2. How does the PC progress? **Answer: PC += 4 at each IF cycle**
3. What is a stall? **Answer: Temporarily stop the pipeline**
4. What is forwarding? **Answer: Divert a result to the next instruction**

### Level 2 (Intermediate)
5. When to use stalling vs forwarding?
6. How to detect a hazard?
7. Why does Load-Use hazard require a stall?
8. How to calculate CPI?

### Level 3 (Advanced)
9. Implement a Forwarding Unit in RTL
10. Analyze the pipeline of a complex sequence
11. Calculate performance impact

---

## Additional Resources

- **Reference Book**: Computer Architecture: A Quantitative Approach (Patterson & Hennessy)
- **Simulator**: Search "RISC-V pipeline simulator" online
- **RISC-V Specification**: https://riscv.org/

---

**Created**: 2026-04-30
**For**: CompArch Course
**Level**: Beginner to Intermediate
**Language**: English

---
