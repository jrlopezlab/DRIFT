# DRIFT

**Dynamic Relaxation for Immediate Flexible Transitions**

**Status: Work in Progress (WIP)**

DRIFT is a computational framework that adapts **Dynamic Relaxation (DR)** from structural engineering to explore conformational changes in biological macromolecules. Given two known conformations, it generates a guided structural transition while seeking to preserve molecular geometry and avoid atomic overlaps.

## The conformational-transition problem

Biological macromolecules often change shape to perform their functions. Connecting two experimentally determined conformations through structurally feasible intermediate structures helps visualize collective motions and provides starting points for further analyses, including molecular dynamics and docking. Generating such trajectories efficiently becomes increasingly challenging as the systems grow larger.

The following RNase III animations illustrate this challenge: generating a continuous conformational transition between the enzyme's closed and open structures.

**RNase III — Conformational transition, view 1**
<!-- VIDEO: RNaseIII_iMORPH_view1_github.mp4 -->
https://github.com/user-attachments/assets/6cafb354-2e81-4dfe-ac84-4ca7f444351c



**RNase III — Conformational transition, view 2**
<!-- VIDEO: RNaseIII_iMORPH_view2_github.mp4 -->
https://github.com/user-attachments/assets/4b381957-c4e5-4756-ad24-2af027b301c5

*These introductory animations were generated with [iMORPH](https://github.com/chaconlab/IMOD), our earlier normal-mode-based morphing method, **not with DRIFT**.*

DRIFT tackles the same problem using a different computational approach based on Dynamic Relaxation.

## How DRIFT works

DRIFT combines an **elastic network** to preserve structural quality, **target springs** to drive the transition, and **collision avoidance** to prevent atomic overlaps. An iterative **Dynamic Relaxation engine** updates the molecular geometry until convergence.

### 1. Elastic network — Preserving structural quality

The *bonding elastic network* helps preserve bond lengths, bond angles, non-φ/ψ dihedral angles, and disulfide bridges.

**Bonding elastic network**
<!-- VIDEO: RNaseIII_bonding_EN_github.mp4 -->
https://github.com/user-attachments/assets/21a3c970-c8e1-413e-8980-ed75adda4e99

The *non-bonding elastic network* uses Tirion/Gō-like springs between selected non-bonded atom pairs to help maintain the overall molecular architecture.

**Non-bonding elastic network**
<!-- VIDEO: RNaseIII_nonbonding_EN_github.mp4 -->
https://github.com/user-attachments/assets/bde7b74f-61a6-42fd-ba35-3deafb97258b


### 2. Target springs — Driving the transition

Target springs use information from the initial and final structures to guide the molecule toward the target conformation.

**Target springs**
<!-- VIDEO: RNaseIII_target_springs_github.mp4 -->
https://github.com/user-attachments/assets/dd767fa3-7ea4-4342-999c-c5955beb1c8f


### 3. Collision avoidance — Preventing atomic overlaps

Overlapping soft spheres generate repulsive corrections, while a linked-cell grid efficiently identifies nearby collision candidates.

**Soft-sphere collision avoidance**
<!-- VIDEO: RNaseIII_collision_avoidance_github.mp4 -->
https://github.com/user-attachments/assets/d28fba55-5a98-412d-a018-736a0fe66a49



### 4. Dynamic Relaxation — The iterative engine

At each iteration, DRIFT propagates motion, evaluates elastic and collision forces, applies geometric corrections and damping, and checks convergence. Unlike normal-mode-based approaches, it does not require matrix diagonalization.

## Conformational transitions — From proteins to viral capsids

The following demonstrations progress from individual proteins to very large macromolecular assemblies, following the presentation given at the **Stockholm symposium in June 2026**. All results are **preliminary**; runtimes refer to the computational setups used in that presentation.

### RNase III — First DRIFT example

RNase III is a bacterial enzyme that recognizes double-stranded RNA and contributes to RNA processing and gene regulation. Its large conformational rearrangement provides a clear example of the structural flexibility involved in RNA recognition and cleavage.

**[1yyo](https://www.rcsb.org/structure/1YYO) → [1yyw](https://www.rcsb.org/structure/1YYW)** · 439 residues · 3,666 atoms  
Initial-to-final ΔRMSD: **17.56 Å** · Final RMSD: **0.88 Å** · Runtime: **27 s**

**View 1**
<!-- VIDEO: RNaseIII_1yyo_1yyw_view1_github.mp4 -->
https://github.com/user-attachments/assets/a60a7479-6dc7-477f-a770-9db1bdac24e4

**View 2**
<!-- VIDEO: RNaseIII_1yyo_1yyw_view2_github.mp4 -->
https://github.com/user-attachments/assets/bad3597e-66f0-4329-891a-73721764b4fb


### Benchmark — 46 protein transitions

DRIFT was evaluated on **46 conformational transitions** from the earlier iMOD/iMORPH benchmark, using full-atom protein models containing **186–994 residues**.

| Benchmark summary | Mean |
| --- | ---: |
| Final RMSD | **0.69 Å** |
| Runtime | **31 s** |

Within this size range, runtime scaled **approximately linearly with the number of residues**.

### Small and medium-sized proteins

#### Adenylate kinase

Adenylate kinase helps maintain cellular energy balance by interconverting ATP, ADP, and AMP. Its opening and closing motions are a classic example of how enzyme domain rearrangements accompany substrate binding and catalysis.

**[1akeA](https://www.rcsb.org/structure/1AKE) → [4akeA](https://www.rcsb.org/structure/4AKE)** · 214 residues · 1,656 atoms  
Initial-to-final ΔRMSD: **7.15 Å** · Final RMSD: **0.86 Å** · Runtime: **13 s**

**View 1**
<!-- VIDEO: AdenylateKinase_1ake_4ake_view1_github.mp4 -->
https://github.com/user-attachments/assets/458e69b7-570c-4f18-981e-f5b504b00ca1

**View 2**
<!-- VIDEO: AdenylateKinase_1ake_4ake_view2_github.mp4 -->
https://github.com/user-attachments/assets/e72ed08c-7567-4a65-a067-33c402e374e4

#### GroEL/GroES monomer

GroEL is an ATP-dependent molecular chaperone that helps other proteins fold correctly, working together with the cochaperone GroES. Changes in the shape of individual GroEL subunits are essential to the coordinated operation of the folding chamber.

**[1aonH](https://www.rcsb.org/structure/1AON) → [1aonA](https://www.rcsb.org/structure/1AON)** · 524 residues · 3,848 atoms  
Initial-to-final ΔRMSD: **12.24 Å** · Final RMSD: **0.94 Å** · Runtime: **27 s**

**View 1**
<!-- VIDEO: GroEL_monomer_1aonH_1aonA_view1_github.mp4 -->
https://github.com/user-attachments/assets/7fe70c3c-b980-4534-b56f-95bfa7c13e7f


**View 2**
<!-- VIDEO: GroEL_monomer_1aonH_1aonA_view2_github.mp4 -->
https://github.com/user-attachments/assets/11eda45b-78b1-4c53-8b78-fc8990943dc5


#### SERCA calcium ATPase

SERCA pumps calcium ions from the cytoplasm into the sarcoplasmic reticulum using energy from ATP, helping muscles relax after contraction. Its transport cycle involves large domain rearrangements that couple ATP hydrolysis to calcium movement.

**[1su4](https://www.rcsb.org/structure/1SU4) → [1t5s](https://www.rcsb.org/structure/1T5S)** · 994 residues · 7,671 atoms  
Initial-to-final ΔRMSD: **13.54 Å** · Final RMSD: **1.21 Å** · Runtime: **50 s**

**Surface representation**
<!-- VIDEO: SERCA_1su4_1t5s_surface_github.mp4 -->
https://github.com/user-attachments/assets/9acbf89c-6b8b-49ea-80b4-1b8537621c5e

**Cartoon representation**
<!-- VIDEO: SERCA_1su4_1t5s_cartoon_github.mp4 -->
https://github.com/user-attachments/assets/07bf3e5b-ee40-4ff6-a4d8-77a7450ae111



### Large macromolecular assemblies

#### SARS-CoV-2 spike

The SARS-CoV-2 spike glycoprotein mediates host-cell attachment and membrane fusion during viral entry. Its conformational flexibility is central to infection and is also relevant to antibody recognition and vaccine design.

**[6xr8](https://www.rcsb.org/structure/6XR8) → [8cy6](https://www.rcsb.org/structure/8CY6)** · 2,904 residues · 22,772 atoms  
Initial-to-final ΔRMSD: **22.63 Å** · Final RMSD: **0.70 Å** · Runtime: **<3 min**

<!-- VIDEO: SARS2_Spike_6xr8_8cy6_github.mp4 -->
https://github.com/user-attachments/assets/5a993607-1d67-403f-b645-e592381255f5


#### GroEL/GroES chaperonin

The complete GroEL/GroES system forms an ATP-regulated folding machine with two stacked GroEL rings and a GroES cap. Coordinated subunit movements reshape its internal chamber, enabling the assisted folding of other proteins.

**H ring → A ring** · 3,668 residues · 26,992 atoms  
Initial-to-final ΔRMSD: **15.18 Å** · Final RMSD: **1.00 Å** · Runtime: **5 min**

**View 1**
<!-- VIDEO: GroEL_chaperonin_Hring_Aring_view1_github.mp4 -->
https://github.com/user-attachments/assets/8a4a51de-2194-46f9-802d-2b415298f1ff


**View 2**
<!-- VIDEO: GroEL_chaperonin_Hring_Aring_view2_github.mp4 -->
https://github.com/user-attachments/assets/d34e7f33-5302-47bc-a02a-83d1fdc5f2aa


#### Alpha-latrotoxin (Lt1a)

Alpha-latrotoxin is a potent neurotoxin from the black widow spider that stimulates neurotransmitter release at nerve terminals. The transition from a *prepore* to a membrane-inserted *pore* involves major structural rearrangements that are key to its mechanism of action.

**[9go9](https://www.rcsb.org/structure/9GO9) → [9goa](https://www.rcsb.org/structure/9GOA)** · 4,320 residues · 37,100 atoms  
Initial-to-final ΔRMSD: **13.85 Å** · Final RMSD: **1.73 Å** · Runtime: **5 min**

**View 1**

<!-- VIDEO: Lt1a_9go9_9goa_view1_github.mp4 -->
https://github.com/user-attachments/assets/795fe1ea-575a-4aed-bbfc-a71e52ce9738

**View 2**

<!-- VIDEO: Lt1a_9go9_9goa_view2_github.mp4 -->
https://github.com/user-attachments/assets/4250b719-7b6f-462c-877f-b16a0b7d1033


#### Bacteriophage T7 portal protein

The portal protein forms a ring-shaped gateway through which bacteriophage DNA passes during genome packaging and release. In phage T7, the portal's closed and open conformations help explain how the viral genome is retained and subsequently ejected during infection.

**[6qx5](https://www.rcsb.org/structure/6QX5) → [6qxm](https://www.rcsb.org/structure/6QXM)** · 4,212 residues · 33,468 atoms  
Initial-to-final ΔRMSD: **6.89 Å** · Final RMSD: **1.11 Å** · Runtime: **4 min**

**View 1**

<!-- VIDEO: PortalProtein_6qx5_6qxm_view1_github.mp4 -->
https://github.com/user-attachments/assets/f94a03b2-3eff-410b-8af0-27e11e1d9526


**View 2**

<!-- VIDEO: PortalProtein_6qx5_6qxm_view2_github.mp4 -->
https://github.com/user-attachments/assets/628c7dda-2584-4ba9-8097-3bd506a0b6c4


#### Mitochondrial ATP synthase — *Polytomella sp.*

ATP synthase produces much of the ATP used by cells, coupling proton flow across a membrane to rotary catalysis. The three structures capture successive rotational states of this molecular machine, illustrating its large, coordinated motions.

**[6RD9](https://www.rcsb.org/structure/6RD9) → [6RDC](https://www.rcsb.org/structure/6RDC) → [6REP](https://www.rcsb.org/structure/6REP)** · 7,103 residues · 53,595 atoms  
Three transitions · Initial-to-final ΔRMSDs: **12.38 / 13.29 / 12.05 Å**  
Final RMSDs: **0.59 / 0.64 / 0.59 Å** · Runtime: **approximately 6 min per transition**

<!-- VIDEO: Mitochondrial_ATPsynthase_6RD9_6RDC_6REP_github.mp4 -->
https://github.com/user-attachments/assets/7afe9563-d0f7-4fe0-b625-dfcb73b8c501


### Very large systems and viral capsids

#### Ryanodine receptor 1 (RyR1)

RyR1 is a large intracellular calcium-release channel that triggers skeletal muscle contraction. Its transition between closed and activated states involves coordinated motion across a massive protein complex, linking molecular structure to calcium signaling.

**[5tb4](https://www.rcsb.org/structure/5TB4) → [5t9v](https://www.rcsb.org/structure/5T9V)** · 16,672 residues · 117,476 atoms  
Initial-to-final ΔRMSD: **7.49 Å** · Final RMSD: **0.43 Å** · Runtime: **<14 min**

**View 1**

<!-- VIDEO: RyanodineReceptor1_5tb4_5t9v_view1_github.mp4 -->
https://github.com/user-attachments/assets/dcd59edb-c4ef-4fc4-98de-a51bf601f9ad

**View 2**

<!-- VIDEO: RyanodineReceptor1_5tb4_5t9v_view2_github.mp4 -->
https://github.com/user-attachments/assets/2409bf4b-299a-43cc-9501-6a0abeee5896

#### Cowpea chlorotic mottle virus (CCMV) capsid

CCMV is a plant RNA virus whose protein capsid protects the viral genome. Its shell undergoes substantial, reversible expansion and contraction, making it a useful model for studying collective motions in viral particles.

**[1ZA7](https://www.rcsb.org/structure/1ZA7) → [8CPY](https://www.rcsb.org/structure/8CPY)** · 28,860 residues · 216,900 atoms  
Initial-to-final ΔRMSD: **22.16 Å** · Final RMSD: **0.82 Å** · Runtime: **27 min**

<!-- VIDEO: CCMV_capsid_1ZA7_8CPY_github.mp4 -->
https://github.com/user-attachments/assets/454edd6b-9e01-400d-aab2-04f527cc2cca


#### HK97 bacteriophage capsid

Bacteriophage HK97 builds a protective protein shell around its DNA genome. During capsid maturation, the initially compact *Prohead II* undergoes dramatic expansion and reorganization toward a stable infectious particle, providing an extreme-scale test of conformational modeling.

**Prohead II ([2gp1](https://www.rcsb.org/structure/2GP1)) → Expansion Intermediate IV ([2frp](https://www.rcsb.org/structure/2FRP))**  
106,680 residues · 827,820 atoms  
Initial-to-final ΔRMSD: **50.77 Å** · Final RMSD: **1.42 Å** · Runtime: **<3 h**

<!-- VIDEO: HK97_capsid_2gp1_2frp_github.mp4 -->
https://github.com/user-attachments/assets/703dbff1-eb65-414f-9980-c05325d6a755


## Project status

**DRIFT is under active development.** Ongoing work includes parameter refinement, broader benchmarking, and preparation of an open-source release. Code, documentation, and additional examples will be added as development progresses.
