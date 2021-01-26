# CHARMM in python  

This project is parsing protein structure with CHARMM in python.

## CHARMM topology structure

Here is the topology structure of residue ALA  

```
RESI ALA          0.00
GROUP
ATOM N    NH1    -0.47  !     |
ATOM HN   H       0.31  !  HN-N
ATOM CA   CT1     0.07  !     |     HB1
ATOM HA   HB      0.09  !     |    /
GROUP                   !  HA-CA--CB-HB2
ATOM CB   CT3    -0.27  !     |    \
ATOM HB1  HA      0.09  !     |     HB3
ATOM HB2  HA      0.09  !   O=C
ATOM HB3  HA      0.09  !     |
GROUP                   !
ATOM C    C       0.51
ATOM O    O      -0.51
BOND CB CA  N  HN  N  CA
BOND C  CA  C  +N  CA HA  CB HB1  CB HB2  CB HB3 
DOUBLE O  C 
IMPR N -C CA HN  C CA +N O
CMAP -C  N  CA  C   N  CA  C  +N
DONOR HN N
ACCEPTOR O C
IC -C   CA   *N   HN    1.3551 126.4900  180.0000 115.4200  0.9996
IC -C   N    CA   C     1.3551 126.4900  180.0000 114.4400  1.5390
IC N    CA   C    +N    1.4592 114.4400  180.0000 116.8400  1.3558
IC +N   CA   *C   O     1.3558 116.8400  180.0000 122.5200  1.2297
IC CA   C    +N   +CA   1.5390 116.8400  180.0000 126.7700  1.4613
IC N    C    *CA  CB    1.4592 114.4400  123.2300 111.0900  1.5461
IC N    C    *CA  HA    1.4592 114.4400 -120.4500 106.3900  1.0840
IC C    CA   CB   HB1   1.5390 111.0900  177.2500 109.6000  1.1109
IC HB1  CA   *CB  HB2   1.1109 109.6000  119.1300 111.0500  1.1119
IC HB1  CA   *CB  HB3   1.1109 109.6000 -119.5800 111.6100  1.1114
```

* GROUP  
  Atoms following **GROUP** indicate these atoms interact electronically, sharing electron density. And a group of atoms carry an integer charge.   
  
* ATOM  
  The atom name, its type and its charge. For example, **ATOM N    NH1    -0.47**, means that the type of "N" atom is "NH1" and has -0.47 charge.  
  
* BOND/DOUBLE  
  Each two connected atoms have a bond. **BOND** means simple bond, and **DOUBLE** means double bond.  
  
* IMPR  
  This indicates sets of 4 atoms for which the improper bond interaction will be calculated. The chiral center is listed first.  
  
The another example is  

```
RESI ARG          1.00
GROUP   
ATOM N    NH1    -0.47  !     |                      HH11
ATOM HN   H       0.31  !  HN-N                       |
ATOM CA   CT1     0.07  !     |   HB1 HG1 HD1 HE     NH1-HH12
ATOM HA   HB      0.09  !     |   |   |   |   |    //(+)  
GROUP                   !  HA-CA--CB--CG--CD--NE--CZ
ATOM CB   CT2    -0.18  !     |   |   |   |         \
ATOM HB1  HA      0.09  !     |   HB2 HG2 HD2        NH2-HH22
ATOM HB2  HA      0.09  !   O=C                       |
GROUP                   !     |                      HH21
ATOM CG   CT2    -0.18
ATOM HG1  HA      0.09
ATOM HG2  HA      0.09
GROUP   
ATOM CD   CT2     0.20
ATOM HD1  HA      0.09
ATOM HD2  HA      0.09
ATOM NE   NC2    -0.70
ATOM HE   HC      0.44
ATOM CZ   C       0.64
ATOM NH1  NC2    -0.80
ATOM HH11 HC      0.46
ATOM HH12 HC      0.46
ATOM NH2  NC2    -0.80
ATOM HH21 HC      0.46
ATOM HH22 HC      0.46
GROUP   
ATOM C    C       0.51
ATOM O    O      -0.51
BOND CB  CA  CG  CB  CD CG  NE CD  CZ NE   
BOND NH2 CZ  N  HN  N  CA   
BOND C   CA  C  +N  CA HA  CB HB1   
BOND CB  HB2 CG  HG1 CG HG2 CD HD1 CD HD2   
BOND NE  HE  NH1 HH11  NH1 HH12  NH2 HH21  NH2 HH22 
DOUBLE O  C    CZ  NH1  
IMPR N  -C  CA  HN   C CA +N O   
IMPR CZ NH1 NH2 NE
CMAP -C  N  CA  C   N  CA  C  +N
DONOR HN N   
DONOR HE NE   
DONOR HH11 NH1   
DONOR HH12 NH1   
DONOR HH21 NH2   
DONOR HH22 NH2   
ACCEPTOR O C   
IC -C   CA   *N   HN    1.3496 122.4500  180.0000 116.6700  0.9973
IC -C   N    CA   C     1.3496 122.4500  180.0000 109.8600  1.5227
IC N    CA   C    +N    1.4544 109.8600  180.0000 117.1200  1.3511
IC +N   CA   *C   O     1.3511 117.1200  180.0000 121.4000  1.2271
IC CA   C    +N   +CA   1.5227 117.1200  180.0000 124.6700  1.4565
IC N    C    *CA  CB    1.4544 109.8600  123.6400 112.2600  1.5552
IC N    C    *CA  HA    1.4544 109.8600 -117.9300 106.6100  1.0836
IC N    CA   CB   CG    1.4544 110.7000  180.0000 115.9500  1.5475
IC CG   CA   *CB  HB1   1.5475 115.9500  120.0500 106.4000  1.1163
IC CG   CA   *CB  HB2   1.5475 115.9500 -125.8100 109.5500  1.1124
IC CA   CB   CG   CD    1.5552 115.9500  180.0000 114.0100  1.5384
IC CD   CB   *CG  HG1   1.5384 114.0100  125.2000 108.5500  1.1121
IC CD   CB   *CG  HG2   1.5384 114.0100 -120.3000 108.9600  1.1143
IC CB   CG   CD   NE    1.5475 114.0100  180.0000 107.0900  1.5034
IC NE   CG   *CD  HD1   1.5034 107.0900  120.6900 109.4100  1.1143
IC NE   CG   *CD  HD2   1.5034 107.0900 -119.0400 111.5200  1.1150
IC CG   CD   NE   CZ    1.5384 107.0900  180.0000 123.0500  1.3401
IC CZ   CD   *NE  HE    1.3401 123.0500  180.0000 113.1400  1.0065
IC CD   NE   CZ   NH1   1.5034 123.0500  180.0000 118.0600  1.3311
IC NE   CZ   NH1  HH11  1.3401 118.0600 -178.2800 120.6100  0.9903
IC HH11 CZ   *NH1 HH12  0.9903 120.6100  171.1900 116.2900  1.0023
IC NH1  NE   *CZ  NH2   1.3311 118.0600  178.6400 122.1400  1.3292
IC NE   CZ   NH2  HH21  1.3401 122.1400 -174.1400 119.9100  0.9899
IC HH21 CZ   *NH2 HH22  0.9899 119.9100  166.1600 116.8800  0.9914 
```

Suppose a protein has two amino acids, **ALA** and **ARG**, the protein structure is following  

```
     H                !
     |                !
   H-N-H              !
     |     HB1        !
     |    /           ! ALA
  HA-CA--CB-HB2       !
     |    \           !
     |     HB3        !
   O=C                !
     |                      HH11          !
  HN-N                       |            !
     |   HB1 HG1 HD1 HE     NH1-HH12      !
     |   |   |   |   |    //(+)           !
  HA-CA--CB--CG--CD--NE--CZ               ! ARG
     |   |   |   |         \              !
     |   HB2 HG2 HD2        NH2-HH22      !
   O=C                       |            !
     |                      HH21          !
     OXT                                  !
```
```
     HC               !
     |                !
  HC-NH3-HC           !
     |      HA        !
     |     /          ! ALA
  HA-CT1--CT3-HA      !
     |     \          !
     |      HA        !
   O=C                !
     |                           HC       !
   H-NH1                         |        !
     |    HA   HA   HA   HA     NC2-HC    !
     |    |    |    |    |     //(+)      !
  HA-CT1--CT2--CT2--CT2--NC2--C           ! ARG
     |    |    |    |          \          !
     |    HA   HA   HA          NC2-HC    !
  OC=CC                         |         !
     |                          HC        !
     OC                                   !
```
The type of **N** in first residue change from **NH1** to **NH3**, and there are three **H**, whose type are **HC**, around it.  
The type of **O** in last residue change from **O** to **OC**. And there is **OXT** on the **C** connected, the type is **OC**. Type of the connected **C** convert to **CC**.

## Energies of CHARMM

The main energies of CHARMM include **BONDS**, **ANGLES**, **DIHEDRALS** and **IMPROPER**. They all are based on bonds of residues.  

### BONDS

```
BONDS
!
!V(bond) = Kb(b - b0)**2
!
!Kb: kcal/mole/A**2
!b0: A
!
!atom type Kb          b0
!
HA   CT1   309.000     1.1110 ! ALLOW   ALI
                ! alkane update, adm jr., 3/2/92
HA   CT2   309.000     1.1110 ! ALLOW   ALI
                ! alkane update, adm jr., 3/2/92
HA   CT3   322.000     1.1110 ! ALLOW   ALI
                ! alkane update, adm jr., 3/2/92
CT1  C     250.000     1.4900 ! ALLOW   ALI PEP POL ARO
                ! Ala Dipeptide ab initio calc's (LK) fixed from 10/90 (5/91)
CE1  CT2   365.000     1.5020   ! 
		! for butene; from propene, yin/adm jr., 12/95
NC2  C     463.000     1.3650 ! ALLOW   PEP POL ARO
                ! 403.0->463.0, 1.305->1.365 guanidinium (KK)
...
```
The upper part is its calculation formula, and the **b** is the distance between two atoms.

### ANGLES

```
!
!V(angle) = Ktheta(Theta - Theta0)**2
!
!V(Urey-Bradley) = Kub(S - S0)**2
!
!Ktheta: kcal/mole/rad**2
!Theta0: degrees
!Kub: kcal/mole/A**2 (Urey-Bradley)
!S0: A
!
!atom types     Ktheta    Theta0   Kub     S0
!
HA   CT1  CT3   34.500    110.10   22.53   2.17900 ! ALLOW   ALI
                ! alkane update, adm jr., 3/2/92
CT3  CT1  C      52.000   108.0000 ! ALLOW   ALI PEP POL ARO
                ! Alanine Dipeptide ab initio calc's (LK)
HC   NC2  C      49.000   120.0000 ! ALLOW   POL PEP ARO
                ! 35.3->49.0 GUANIDINIUM (KK)
HC   NH3  CT1   30.000    109.50   20.00   2.07400 ! ALLOW   POL ALI
                ! new stretch and bend; methylammonium (KK 03/10/92)
...
```
The upper part is its calculation formula. And **ANGLES** includes two parts, angle energy and Urey-Bradley energy.  
**Theta** is the degree of angle *atom1--atom2--atom3*, **S** is the distance of *atom1-atom3*.

### DIHEDRALS

```
!
!V(dihedral) = Kchi(1 + cos(n(chi) - delta))
!
!Kchi: kcal/mole
!n: multiplicity
!delta: degrees
!
!atom types             Kchi    n   delta
!
!Neutral N terminus
CT3  CT2  CT2  CT2      0.1500  1     0.00 ! ALLOW ALI
                ! alkane update, adm jr., 3/2/92, butane trans/gauche
O    C    NH1  CT1      2.5000  2   180.00 !  ALLOW PEP
                ! Gives appropriate NMA cis/trans barrier. (LK)
O    C    NH1  CT2      2.5000  2   180.00 !  ALLOW PEP
                ! Gives appropriate NMA cis/trans barrier. (LK)
O    C    NH1  CT3      2.5000  2   180.00 !  ALLOW PEP
                ! Gives appropriate NMA cis/trans barrier. (LK)
X    CT2  CT2  X        0.1950  3     0.00 ! ALLOW   ALI
                ! alkane update, adm jr., 3/2/92                
X    C    NC2  X        2.2500  2   180.00 ! ALLOW   PEP POL ARO
                ! 9.0->2.25 GUANIDINIUM (KK)
...
```
The upper part is its calculation formula. And **X** means wildcard, if there is more than one (includeing **X**) type of dihedral angle, match those without wildcards.

### IMPROPER

```
IMPROPER
!
!V(improper) = Kpsi(psi - psi0)**2
!
!Kpsi: kcal/mole/rad**2
!psi0: degrees
!note that the second column of numbers (0) is ignored
!
!atom types           Kpsi                   psi0
!
HE2  HE2  CE2  CE2     3.0            0      0.00   ! 
		! for ethene, yin/adm jr., 12/95
HR1  NR1  NR2  CPH2    0.5000         0      0.0000 ! ALLOW ARO
                ! his, adm jr., 7/05/90
                ! 5.75->40.0 GUANIDINIUM (KK)
NH1  X    X    H      20.0000         0      0.0000 ! ALLOW   PEP POL ARO
                ! NMA Vibrational Modes (LK)
NR3  CPH2 CPH1 H       1.2000         0      0.0000 ! ALLOW ARO
                ! his, adm jr., 6/27/90
O    N    CT2  CC    120.0000         0      0.0000 ! ALLOW PEP POL PRO
                ! 6-31g* AcProNH2 and ProNH2  RLD 5/19/92
O    NH2  HA   CC     45.0000         0      0.0000 ! ALLOW PEP POL
                ! adm jr., 5/13/91, formamide geometry and vibrations
O    X    X    C     120.0000         0      0.0000 ! ALLOW   PEP POL ARO
                ! NMA Vibrational Modes (LK)
OC   X    X    CC     96.0000         0      0.0000 ! ALLOW   PEP POL ARO ION
                ! 90.0->96.0 acetate, single impr (KK)
CC   X    X    CT1    96.0000         0      0.0000 ! ALLOW   PEP POL ARO ION
                ! 90.0->96.0 acetate, single impr (KK)
...
```
The upper part is its calculation formula. And **X** means wildcard, if there is more than one (includeing **X**) type of dihedral angle, match those without wildcards.
