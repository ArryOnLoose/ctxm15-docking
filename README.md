# Docking Screen of Natural Compounds against CTX-M-15 Beta-Lactamase

Computational screening of 30 natural compounds as possible binders of CTX-M-15, an extended-spectrum beta-lactamase (ESBL) that makes bacteria resistant to penicillins and third-generation cephalosporins such as cefotaxime. The work uses molecular docking with AutoDock Vina and PLIP interaction analysis.

**Author:** Ansh Shukla

## What this project does

1. Prepares the CTX-M-15 structure (PDB ID: 4HBT): removes water and other crystal additives, adds missing atoms and hydrogens at pH 7.4, and converts it to PDBQT.
2. Builds a library of 30 natural compounds (flavonoids, phenolics, alkaloids, terpenoids) plus 4 reference compounds (cefotaxime, avibactam, clavulanic acid, tazobactam) from PubChem.
3. Docks everything with AutoDock Vina 1.2.5 into a 22 x 22 x 22 A box centred on the catalytic residues (Ser70, Lys73, Ser130, Asn132, Glu166, Asn170, Lys234, Ser237).
4. Redocks the top compounds with a different seed and higher exhaustiveness as a reproducibility check.
5. Analyses the poses with PLIP and distance checks, using the original 4HBT residue numbers.

## Main results

| Compound | Vina score (kcal/mol) | Ligand efficiency |
|---|---|---|
| Sanguinarine | -9.16 | -0.367 |
| Epigallocatechin gallate | -8.95 | -0.271 |
| Ellagic acid | -8.67 | -0.394 |
| Rutin | -8.52 | -0.198 |
| Luteolin | -7.98 | -0.380 |
| Myricetin | -7.94 | -0.345 |
| *Cefotaxime (reference substrate)* | *-7.61* | *-0.254* |

Shortlist after score, ligand efficiency, reproducibility and interaction analysis: **ellagic acid, myricetin and baicalein**, with **sanguinarine** as a cautious fourth (different binding mode, and a compound type that often gives false positives in lab assays).

The full table is in `results/docking_results.csv`.

## Important notes

- Only four compounds (sanguinarine, epigallocatechin gallate, ellagic acid, luteolin) were redocked at exhaustiveness 32 with seed 7. The other compounds were docked once at exhaustiveness 8 with seed 42.
- The protein is rigid, water is removed, and ligands were docked in neutral form.
- The covalent inhibitors (avibactam, clavulanic acid, tazobactam) cannot be modelled properly by Vina. Their scores are not comparable with the natural compounds.
- **This is docking only.** It predicts that a compound may fit in the active site. It does not show enzyme inhibition or antibacterial activity.

## Repository layout

```
notebooks/   Colab notebook with the full workflow
data/        library.csv (compound names and SMILES), box_center.json
structures/  receptor_clean.pdb, receptor.pdbqt
poses/       best Vina pose per compound (exhaustiveness 8)
poses_exh32/ redocked poses (exhaustiveness 32)
complexes/   protein + ligand PDB files with original 4HBT numbering
results/     docking_results.csv, pose_analysis.csv, plip_interactions.csv
report/      written report
```

## Reproducing the work

Software used:

- AutoDock Vina 1.2.5 (command-line program)
- Open Babel 3.x (command-line program, `obabel`)
- Python 3 with the packages in `requirements.txt`

Run the notebook in `notebooks/` from top to bottom. It was written for Google Colab, with files saved to Google Drive. Change the `WORKDIR` path if you run it elsewhere.

Note that the residue numbering in `structures/receptor_clean.pdb` starts from 1, so it does not match the standard class A numbering. The files in `complexes/` use the original 4HBT numbering.

## References

- Trott O, Olson AJ. J Comput Chem. 2010;31(2):455-461 (AutoDock Vina).
- Eberhardt J, et al. J Chem Inf Model. 2021;61(8):3891-3898 (AutoDock Vina 1.2).
- Salentin S, et al. Nucleic Acids Res. 2015;43(W1):W443-W447 (PLIP).
- Berman HM, et al. Nucleic Acids Res. 2000;28(1):235-242 (Protein Data Bank).
