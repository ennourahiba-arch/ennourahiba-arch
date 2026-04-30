# 🚀 RISC-V Pipelining - Guide Complet

## Table des matières
1. [Introduction au Pipelining](#introduction)
2. [Architecture RISC-V Pipeline](#architecture)
3. [Stalling (Blocage)](#stalling)
4. [Forwarding Unit](#forwarding)
5. [Comparaisons et Résumés](#comparaisons)
6. [Formules et Calculs](#formules)

---

## <a name="introduction"></a>1. Introduction au Pipelining

### Qu'est-ce que le Pipelining ?

Le **pipelining** est une technique d'optimisation qui permet à un processeur d'exécuter **plusieurs instructions en même temps**, en les divisant en étapes.

### Analogie : Chaîne de montage

```
SANS PIPELINING (Séquentiel):
Voiture 1 : Peindre → Assembler → Tester → Livrer
Voiture 2 :                              Peindre → Assembler → Tester → Livrer
```

```
AVEC PIPELINING (Parallèle):
Voiture 1 : Peindre → Assembler → Tester → Livrer
Voiture 2 :           Peindre  → Assembler → Tester → Livrer
Voiture 3 :                     Peindre  → Assembler → Tester → Livrer
```

**Résultat** : Meilleur débit d'instructions ! 🚀

---

## <a name="architecture"></a>2. Architecture RISC-V Pipeline

### Les 5 étages du Pipeline

```
┌─────────────────────────────────────────────────────────────┐
│                    PIPELINE RISC-V 5 ÉTAGES                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  IF        ID        EX        MEM       WB                 │
│  ──        ──        ──        ───       ──                 │
│ Fetch    Decode   Execute   Memory    Write Back            │
│  ↓         ↓         ↓         ↓         ↓                   │
│ Chercher  Décoder  Exécuter  Accès    Écrire              │
│ l'instr   l'instr   l'ALU     Mémoire   Résultat           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Détails de chaque étage

| Étage | Acronyme | Rôle | Accès |
|-------|----------|------|-------|
| **Instruction Fetch** | IF | Charger l'instruction depuis la mémoire | Mém. Instr |
| **Instruction Decode** | ID | Décoder et lire les registres | Registres |
| **Execute** | EX | Exécuter l'opération ALU | ALU |
| **Memory Access** | MEM | Accéder à la mémoire (Load/Store) | Mém. Données |
| **Write Back** | WB | Écrire le résultat dans les registres | Registres |

---

### Timeline simple (3 instructions)

```
Instruction 1: LW x1, 0(x2)
Instruction 2: ADD x3, x1, x4
Instruction 3: SW x3, 0(x5)

Cycle  1   2   3   4   5   6   7
────────────────────────────────────
LW    IF  ID  EX  MEM WB
ADD       IF  ID  EX  MEM WB
SW            IF  ID  EX  MEM WB

📊 Résultat: 7 cycles (au lieu de 15 sans pipelining)
```

---

### PC (Program Counter) dans le Pipeline

Le **PC** indique l'adresse de l'instruction suivante à chercher.

```
┌─────────────────────────────────────────┐
│          PC PROGRESSION                 │
├─────────────────────────────────────────┤
│                                          │
│ Cycle 1: PC = 0x0000                    │
│          IF cherche instr à 0x0000       │
│          PC = PC + 4 (RISC-V: 32 bits)  │
│                                          │
│ Cycle 2: PC = 0x0004                    │
│          IF cherche instr à 0x0004       │
│          PC = PC + 4                     │
│                                          │
│ Cycle 3: PC = 0x0008                    │
│          ...                             │
│                                          │
└─────────────────────────────────────────┘
```

---

### Registres Pipeline (Latches)

Les données se déplacent entre étages via des registres pipeline :

```
IF/ID    → Stocke l'instruction du stage IF
ID/EX    → Stocke les operandes et instruction décodée
EX/MEM   → Stocke le résultat de l'ALU
MEM/WB   → Stocke la donnée lue de la mémoire
```

**Exemple :**
```
Cycle 1: LW se charge en IF
         IF/ID ← LW

Cycle 2: LW passe à ID
         ID/EX ← information sur LW (registres lus, etc)

Cycle 3: LW passe à EX
         EX/MEM ← adresse mémoire calculée

Cycle 4: LW passe à MEM
         MEM/WB ← valeur lue de la mémoire

Cycle 5: LW passe à WB
         x1 ← valeur (finalement écrite !)
```

---

## <a name="stalling"></a>3. Stalling (Blocage)

### Qu'est-ce que le Stalling ?

Le **stalling** = **arrêter temporairement** une partie du pipeline parce qu'on ne peut pas continuer.

C'est un embouteillage au niveau du processeur. 🛑

---

### Quand avoir besoin de Stalling ?

**Problème** : Une instruction a besoin d'une donnée qui n'est pas encore disponible.

```
LW x1, 0(x2)    # Charge x1 (résultat en cycle 4 à MEM)
ADD x3, x1, x4  # Utilise x1 (a besoin de x1 en cycle 3 à EX)

❌ CONFLIT : ADD a besoin de x1 avant que LW ne l'ait chargé !
```

---

### Timeline avec Stalling

```
Cycle  1   2   3   4   5   6   7   8   9
────────────────────────────────────────────
LW    IF  ID  EX  MEM WB
ADD       IF  ID  ⏸️  ⏸️  EX  MEM WB
SUB           IF  ⏸️  ⏸️  ⏸️  ID  EX  MEM

⏸️ = Bulle (NOP - No Operation)

2 cycles d'attente pour ADD
```

---

### Comment fonctionne le Stalling ?

#### Étape 1 : Détection du Hazard

**Hazard Detection Unit** regarde :
```
Si (ID/EX.MemRead == 1) ET 
   ((ID/EX.Rd == IF/ID.Rs1) OU (ID/EX.Rd == IF/ID.Rs2))
   
Alors: STALL DÉTECTÉ !
```

**Exemple :**
```
LW x1, 0(x2)     ← ID/EX.MemRead = 1, ID/EX.Rd = x1
ADD x3, x1, x4   ← IF/ID.Rs1 = x1
                    MATCH ! → STALL !
```

#### Étape 2 : Action du Stall

```
┌─────────────────────────────────────────┐
│        ACTIONS LORS D'UN STALL          │
├─────────────────────────────────────────┤
│                                          │
│ IF STAGE:  Continue à chercher (normal) │
│                                          │
│ ID STAGE:  PC ne change pas ❌         │
│            IF/ID ne change pas ❌      │
│            (Instruction reste bloquée)  │
│                                          │
│ EX STAGE:  Continue normalement         │
│ MEM STAGE: Continue normalement         │
│ WB STAGE:  Continue normalement         │
│                                          │
└─────────────────────────────────────────┘
```

---

### Code RTL du Stalling

```verilog
// Hazard Detection Unit
if (ID_EX_MemRead && 
    ((ID_EX_RegisterRd == IF_ID_RegisterRs1) || 
     (ID_EX_RegisterRd == IF_ID_RegisterRs2))) {
    
    // INSERT STALL
    stall = 1;
    
    // Geler le PC (ne pas avancer)
    PC_write = 0;  // PC reste identique
    
    // Geler le registre IF/ID
    IF_ID_write = 0;  // IF/ID reste identique
    
    // Insérer un NOP dans ID/EX
    ID_EX = 0;  // Bulle vide
}
else {
    stall = 0;
    PC_write = 1;
    IF_ID_write = 1;
    ID_EX = données_normales;
}
```

---

### Exemple complet : Load-Use Hazard

```
Code RISC-V:
    LW x1, 0(x2)
    ADD x3, x1, x4
    SUB x5, x6, x7

SANS STALLING (❌ FAUX):
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]      ← ADD lit x1 (pas prêt !)
Cycle 4: LW [MEM]   ADD [EX]      ← x1 = 42 (trop tard !)
Cycle 5: LW [WB]    ADD [MEM]
         Registres[x1] = 42

❌ Résultat: ADD utilise une mauvaise valeur de x1


AVEC STALLING (✅ CORRECT):
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]      ← Détection du stall !
Cycle 4: LW [MEM]   ADD [⏸️]      ← ADD reste bloqué
Cycle 5: LW [WB]    ADD [ID]      ← x1 = 42 (enfin prêt !)
Cycle 6:            ADD [EX]
Cycle 7:            ADD [MEM]
Cycle 8:            ADD [WB]

✅ Résultat: ADD utilise la bonne valeur de x1 = 42
```

---

### Performance du Stalling

```
╔════════════════════════════════════════════════════════════╗
║           IMPACT DU STALLING SUR LA PERFORMANCE            ║
╠════════════════════════════════════════════════════════════╣
║                                                             ║
║ Nombre d'instructions : 100                                ║
║ Load-Use Hazards : 20 (chacun = 1 stall)                  ║
║                                                             ║
║ CPI sans stall : 1.0 (1 instr par cycle)                   ║
║ CPI avec stall : 1.2 (20% plus lent)                       ║
║                                                             ║
║ Slowdown = 1.2x                                             ║
║                                                             ║
╚════════════════════════════════════════════════════════════╝
```

---

## <a name="forwarding"></a>4. Forwarding Unit (Bypass)

### Qu'est-ce que le Forwarding ?

**Forwarding** = Au lieu d'attendre que le résultat soit écrit dans les registres, on le **détourne directement** vers où il est nécessaire.

C'est un raccourci ! ⚡

---

### Le problème sans Forwarding

```
LW x1, 0(x2)    # Charge x1 (résultat en MEM au cycle 4)
ADD x3, x1, x4  # Utilise x1 (a besoin de x1 en EX au cycle 3)

❌ Sans Forwarding + Sans Stalling:
ADD utilise une mauvaise valeur (erreur !)

❌ Sans Forwarding + Avec Stalling:
2 cycles perdus (lent !)
```

---

### La solution : Forwarding Unit

```
┌──────────────────────────────────────────────────────────┐
│             FORWARDING UNIT ARCHITECTURE                 │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  EX STAGE (Exécution)                                    │
│  ┌──────────────────────────────────────────┐            │
│  │  Sources d'opérandes pour l'ALU:         │            │
│  │                                            │            │
│  │  Option 1: Registres[Rs1] (normal)       │            │
│  │           ForwardAE = 00                  │            │
│  │                                            │            │
│  │  Option 2: EX/MEM.ALUResult (MEM stage)  │            │
│  │           ForwardAE = 01  ← Forward !    │            │
│  │                                            │            │
│  │  Option 3: MEM/WB.ALUResult (WB stage)   │            │
│  │           ForwardAE = 10  ← Forward !    │            │
│  │                                            │            │
│  └──────────────────────────────────────────┘            │
│                                                           │
│  Multiplexer (3:1) choisit la bonne source               │
│                                                           │
└──────────────────────────────────────────────────────────┘
```

---

### Règles de Forwarding

#### **Règle 1 : EX Hazard (Forward de MEM)**

```
Si:
  EX/MEM.RegWrite = 1  (l'instruction MEM écrit un registre)
  EX/MEM.Rd ≠ 0        (ce n'est pas x0)
  EX/MEM.Rd == ID/EX.Rs1  (match sur la source)

Alors:
  ForwardAE = 01  (utiliser EX/MEM.ALUResult)

Exemple:
  LW x1, 0(x2)       ← Écrira dans x1
  ADD x3, x1, x4     ← Lit x1
           ↑ ↑ Match → Forward du MEM !
```

#### **Règle 2 : MEM Hazard (Forward de WB)**

```
Si:
  MEM/WB.RegWrite = 1
  MEM/WB.Rd ≠ 0
  MEM/WB.Rd == ID/EX.Rs1 OU ID/EX.Rs2
  ET Règle 1 non matchée

Alors:
  ForwardAE/BE = 10  (utiliser MEM/WB.ALUResult)

Exemple:
  ADD x1, x2, x3       ← Écrit x1 en WB
  ...
  SUB x5, x1, x6       ← Lit x1 en EX
           ↑ ↑ Match → Forward du WB !
```

---

### Code RTL du Forwarding

```verilog
// Détection et contrôle du Forwarding
module ForwardingUnit (
    input [4:0] ID_EX_Rs1, ID_EX_Rs2,
    input [4:0] EX_MEM_Rd, MEM_WB_Rd,
    input EX_MEM_RegWrite, MEM_WB_RegWrite,
    output [1:0] ForwardAE, ForwardBE
);

// ForwardAE pour opérande A
always @(*) begin
    if (EX_MEM_RegWrite && EX_MEM_Rd != 0 && 
        EX_MEM_Rd == ID_EX_Rs1) begin
        ForwardAE = 2'b01;  // De MEM
    end
    else if (MEM_WB_RegWrite && MEM_WB_Rd != 0 && 
             MEM_WB_Rd == ID_EX_Rs1) begin
        ForwardAE = 2'b10;  // De WB
    end
    else begin
        ForwardAE = 2'b00;  // Des registres (normal)
    end
end

// ForwardBE pour opérande B (même logique)
always @(*) begin
    if (EX_MEM_RegWrite && EX_MEM_Rd != 0 && 
        EX_MEM_Rd == ID_EX_Rs2) begin
        ForwardBE = 2'b01;  // De MEM
    end
    else if (MEM_WB_RegWrite && MEM_WB_Rd != 0 && 
             MEM_WB_Rd == ID_EX_Rs2) begin
        ForwardBE = 2'b10;  // De WB
    end
    else begin
        ForwardBE = 2'b00;  // Des registres (normal)
    end
end

endmodule
```

---

### Timeline avec Forwarding

```
Code:
    LW x1, 0(x2)
    ADD x3, x1, x4

SANS FORWARDING (❌ 2 stalls):
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]        ← Stall détecté
Cycle 4: LW [MEM]   ADD [⏸️]        ← Stall
Cycle 5: LW [WB]    ADD [ID]
Cycle 6:            ADD [EX]
Cycle 7:            ADD [MEM]
Cycle 8:            ADD [WB]

⏹️ Total: 8 cycles (LENT !)


AVEC FORWARDING (✅ 0 stalls):
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]
Cycle 4: LW [MEM]   ADD [EX]  ← Forward du MEM
         42  ↓
         ADD reçoit 42 !
Cycle 5: LW [WB]    ADD [MEM]
Cycle 6:            ADD [WB]

✅ Total: 6 cycles (RAPIDE !)
```

---

### Cas où Forwarding NE MARCHE PAS

#### ⚠️ Load-Use Hazard (seul cas demandant stall)

```
LW x1, 0(x2)      # Charge x1 (résultat en MEM au cycle 4)
ADD x3, x1, x4    # Utilise x1 en EX au cycle 3

Timeline:
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]    ← ADD a besoin de x1 NOW
Cycle 4: LW [MEM]   ADD [EX]    ← x1 arrive en MEM (trop tard !)

❌ PROBLÈME : x1 arrive APRÈS que ADD en ait eu besoin
   Forwarding ne peut pas aider (x1 n'existe pas encore !)

Solution: Stall + NOP
Cycle 1: LW [IF]
Cycle 2: LW [ID]    ADD [IF]
Cycle 3: LW [EX]    ADD [ID]    ← Stall détecté
Cycle 4: LW [MEM]   NOP [⏸️]
Cycle 5: LW [WB]    ADD [ID]    ← Maintenant on peut continuer
Cycle 6:            ADD [EX]    ← x1 est disponible !
```

---

### Performance du Forwarding

```
╔════════════════════════════════════════════════════════════╗
║        IMPACT DU FORWARDING SUR LA PERFORMANCE             ║
╠════════════════════════════════════════════════════════════╣
║                                                             ║
║ Nombre d'instructions : 100                                ║
║ Dépendances totales : 50                                   ║
║  - Load-Use Hazards : 5  (demandent stall)                 ║
║  - Autres : 45 (forwarding suffit)                         ║
║                                                             ║
║ CPI sans optimisation : 1.5 (50% plus lent)                ║
║ CPI avec stalling seul : 1.2 (20% plus lent)               ║
║ CPI avec forwarding : 1.05 (5% plus lent)                  ║
║                                                             ║
║ Forwarding = 4x mieux que stalling seul ! ⚡              ║
║                                                             ║
╚════════════════════════════════════════════════════════════╝
```

---

## <a name="comparaisons"></a>5. Comparaisons et Résumés

### Comparaison : Sans rien vs Stalling vs Forwarding

```
Scénario: LW x1, 0(x2)  →  ADD x3, x1, x4

╔═══════════════════════════════════════════════════════════════════╗
║                    SANS OPTIMISATION (❌)                        ║
╠═══════════════════════════════════════════════════════════════════╣
║ Cycle 1: LW [IF]                                                  ║
║ Cycle 2: LW [ID]    ADD [IF]                                      ║
║ Cycle 3: LW [EX]    ADD [ID]    ← ADD lit x1 (pas prêt !)       ║
║ Cycle 4: LW [MEM]   ADD [EX]    ← ERREUR ! Mauvaise valeur      ║
║ Cycle 5: LW [WB]    ADD [MEM]                                     ║
║ Cycle 6:            ADD [WB]                                      ║
║                                                                    ║
║ Total: 6 cycles, RÉSULTAT FAUX ❌                                ║
╚═══════════════════════════════════════════════════════════════════╝

╔═══════════════════════════════════════════════════════════════════╗
║               AVEC STALLING (Correct mais lent)                   ║
╠═══════════════════════════════════════════════════════════════════╣
║ Cycle 1: LW [IF]                                                  ║
║ Cycle 2: LW [ID]    ADD [IF]                                      ║
║ Cycle 3: LW [EX]    ADD [ID]    ← Stall détecté                  ║
║ Cycle 4: LW [MEM]   NOP [⏸️]    ← 1er stall                      ║
║ Cycle 5: LW [WB]    ADD [ID]    ← x1 = 42 prêt !                 ║
║ Cycle 6:            ADD [EX]                                      ║
║ Cycle 7:            ADD [MEM]                                     ║
║ Cycle 8:            ADD [WB]                                      ║
║                                                                    ║
║ Total: 8 cycles, RÉSULTAT CORRECT ✅ (mais 33% plus lent)       ║
╚═══════════════════════════════════════════════════════════════════╝

╔═══════════════════════════════════════════════════════════════════╗
║            AVEC FORWARDING (Correct et rapide) ⚡                ║
╠═══════════════════════════════════════════════════════════════════╣
║ Cycle 1: LW [IF]                                                  ║
║ Cycle 2: LW [ID]    ADD [IF]                                      ║
║ Cycle 3: LW [EX]    ADD [ID]                                      ║
║ Cycle 4: LW [MEM]   ADD [EX]    ← Forward du MEM                  ║
║         42  ↓                                                      ║
║         ADD reçoit 42 immédiatement !                             ║
║ Cycle 5: LW [WB]    ADD [MEM]                                     ║
║ Cycle 6:            ADD [WB]                                      ║
║                                                                    ║
║ Total: 6 cycles, RÉSULTAT CORRECT ✅ (RAPIDE !)                 ║
╚═══════════════════════════════════════════════════════════════════╝
```

---

### Types de Hazards

```
╔════════════════════════════════════════════════════════════╗
║               TYPES DE HAZARDS (CONFLITS)                  ║
╠════════════════════════════════════════════════════════════╣
║                                                             ║
║ 1. DATA HAZARD (Conflit de données)                        ║
║    ─────────────────────────────────────                   ║
║    Instruction A écrit dans un registre                    ║
║    Instruction B lit ce même registre                      ║
║                                                             ║
║    Exemple:                                                ║
║      ADD x1, x2, x3                                        ║
║      SUB x4, x1, x5  ← Dépend de x1                       ║
║                                                             ║
║    Solutions:                                              ║
║      • Forwarding (si données disponibles)                 ║
║      • Stalling (si load-use hazard)                       ║
║                                                             ║
║ ─────────────────────────────────────────────────────────  ║
║                                                             ║
║ 2. CONTROL HAZARD (Conflit de branchement)                ║
║    ────────────────────────────────────────                ║
║    Branchement décide où aller en EX                       ║
║    Mais on cherche déjà la prochaine instr en IF           ║
║                                                             ║
║    Exemple:                                                ║
║      BEQ x1, x2, label  ← On ne sait pas où aller         ║
║      ADD x3, x4, x5     ← Quelle instr charger ?          ║
║                                                             ║
║    Solutions:                                              ║
║      • Branch Prediction                                   ║
║      • Flush du Pipeline                                   ║
║      • Branch Delay Slots                                  ║
║                                                             ║
║ ─────────────────────────────────────────────────────────  ║
║                                                             ║
║ 3. STRUCTURAL HAZARD (Conflit de ressource)               ║
║    ──────────────────────────────────────────              ║
║    2 instructions ont besoin de la même ressource          ║
║    au même moment                                          ║
║                                                             ║
║    Exemple:                                                ║
║      2 instructions veulent accéder à la mémoire            ║
║      mais il n'y a qu'un port mémoire                      ║
║                                                             ║
║    Solution:                                               ║
║      • Architecture en dual-port (2 accès parallèles)      ║
║      • Stalling (rare en RISC-V bien conçu)               ║
║                                                             ║
╚════════════════════════════════════════════════════════════╝
```

---

### Tableau récapitulatif

| Technique | Vitesse | Complexité | Efficacité | Utilisation |
|-----------|---------|-----------|-----------|------------|
| **Aucune** | ⚡⚡⚡ | Simple | ❌ Erreurs | Pas utilisé |
| **Stalling** | ⏸️ | Moyen | ⚠️ Correct | Rare (LU hazard) |
| **Forwarding** | ⚡⚡ | Complexe | ✅ Optimal | RISC-V standard |
| **Branch Pred.** | ⚡⚡⚡ | Très complexe | ✅ Excellent | CPU modernes |

**RISC-V moderne = Forwarding + Stalling (en cas de Load-Use) + Branch Prediction**

---

## <a name="formules"></a>6. Formules et Calculs

### CPI (Cycles Per Instruction)

```
CPI = Nombre total de cycles / Nombre d'instructions

Exemple:
  5 instructions en 7 cycles = 7/5 = 1.4 CPI
  
  Interprétation:
  CPI = 1.0 : En moyenne, 1 instruction par cycle (idéal)
  CPI = 1.5 : En moyenne, 1.5 cycles par instruction (lent)
```

### Performance avec et sans Stalling

```
Cas 1: Sans optimisation (stalling systématique)
────────────────────────────────────────────────
N instructions
H dépendances (chacune = 1 stall)

CPI = (N + H) / N

Exemple: 100 instr, 20 dépendances
CPI = 120 / 100 = 1.2

Speedup = CPI_sans / CPI_avec
```

### Speedup avec Forwarding

```
Cas 2: Avec Forwarding (stalling seulement pour Load-Use)
─────────────────────────────────────────────────────────
N instructions
LU Load-Use hazards (chacun = 1 stall)

CPI = (N + LU) / N

Exemple: 100 instr, 5 Load-Use
CPI = 105 / 100 = 1.05

Amélioration vs stalling seul = 1.2 / 1.05 = 1.14x plus rapide
```

### Latence d'instruction

```
Latence de LW (Load Word):
─────────────────────────
IF → ID → EX → MEM → WB = 5 cycles
(ou 4 si on compte depuis le départ)

Donc une instruction dépendante doit attendre:
- Sans forwarding: attendre jusqu'à WB (5 cycles)
- Avec forwarding: attendre jusqu'à MEM (3 cycles)
- Gain: 2 cycles économisés !
```

### Formule générale

```
Temps d'exécution = Nombre d'instructions × CPI × Cycle time

Exemple:
  100 instr, CPI=1.5, cycle=2ns
  Temps = 100 × 1.5 × 2ns = 300ns

Avec optimisation (CPI=1.05):
  Temps = 100 × 1.05 × 2ns = 210ns
  
  Speedup = 300ns / 210ns = 1.43x plus rapide
```

---

## Résumé Visuel Complet

```
┌─────────────────────────────────────────────────────────────┐
│              PIPELINE RISC-V COMPLET                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  INSTRUCTION FLOW:                                          │
│  ─────────────────                                          │
│  Mémoire Instr                                              │
│       ↓                                                      │
│  IF Stage (cherche instr à PC)                              │
│       ↓ (PC += 4)                                           │
│  ID Stage (décode, lit registres)                           │
│       ↓                                                      │
│  EX Stage (ALU, forwarding unit ici)                        │
│       ↓                                                      │
│  MEM Stage (Load/Store)                                     │
│       ↓                                                      │
│  WB Stage (écrit registre)                                  │
│       ↓                                                      │
│  Registres                                                  │
│                                                              │
│  HAZARDS DÉTECTÉS:                                          │
│  ─────────────────                                          │
│  • Data Hazard → Forwarding ou Stalling                     │
│  • Control Hazard → Branch Prediction                       │
│  • Structural Hazard → Architecture ou Stalling             │
│                                                              │
│  OPTIMISATIONS:                                             │
│  ───────────────                                            │
│  1. Forwarding Unit → Détourne les résultats               │
│  2. Hazard Detection Unit → Détecte et stalle               │
│  3. Branch Predictor → Prédiction de branches               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Checkliste pour le Cours

- [ ] **Pipelining** : Concept et avantages
- [ ] **5 étages** : IF, ID, EX, MEM, WB
- [ ] **PC** : Comment il progresse
- [ ] **Data Hazards** : Types et causes
- [ ] **Stalling** : Quand et comment
- [ ] **Forwarding** : Règles et architecture
- [ ] **Load-Use Hazard** : Cas special
- [ ] **CPI** : Calcul et interprétation
- [ ] **Performance** : Comparaisons et speedup
- [ ] **Registres Pipeline** : IF/ID, ID/EX, EX/MEM, MEM/WB

---

## Questions de Révision

### Niveau 1 (Basique)
1. Combien d'étages dans un pipeline RISC-V standard ? **Réponse: 5**
2. Comment progresse le PC ? **Réponse: PC += 4 à chaque cycle IF**
3. Qu'est-ce qu'un stall ? **Réponse: Arrêter temporairement le pipeline**
4. Qu'est-ce que le forwarding ? **Réponse: Détourner un résultat vers l'instruction suivante**

### Niveau 2 (Intermédiaire)
5. Quand utiliser stalling vs forwarding ?
6. Comment détecte-t-on un hazard ?
7. Pourquoi Load-Use hazard demande un stall ?
8. Comment calcule-t-on le CPI ?

### Niveau 3 (Avancé)
9. Implémenter une Forwarding Unit en RTL
10. Analyser le pipeline d'une séquence complexe
11. Calculer l'impact sur les performances

---

## Ressources Supplémentaires

- **Livre référence** : Computer Architecture: A Quantitative Approach (Patterson & Hennessy)
- **Simulateur** : Rechercher "RISC-V pipeline simulator" en ligne
- **RISC-V Spec** : https://riscv.org/

---

**Créé le**: 2026-04-30
**Pour**: Cours CompArch
**Niveau**: Débutant à Intermédiaire

---
