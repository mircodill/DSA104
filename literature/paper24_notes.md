**Multi-Threshold Analysis for Chemical Space Mapping of Ni-Catalyzed Suzuki-Miyaura Couplings**

Paper: Multi-Threshold Analysis for Chemical Space Mapping of Ni-Catalyzed Suzuki-Miyaura Couplings

What question is being solved computationally?
which monophosphine ligands will work in Ni-catalyzed SMC reactions, without having to test all of them experimentally
task: *binary classification — active vs. inactive ligand*, based on physicochemical descriptors

Where does learning replace explicit modeling?
no mechanistic model is built from first principles
a *decision tree learns thresholds directly from HTE data* (450 reactions, 90 ligands × 5 reactions)
the algorithm scans all descriptors and picks the ones that best separate active/inactive — no human pre-selection

What is the ground truth and how reliable is it?
ground truth = experimental yield from high-throughput experimentation (HTE)
active = yield > 10% above ligand-less control
HTE conditions are standardized but not identical to benchtop — validation set was ~15% higher-yielding than training data, so the threshold is condition-dependent
Key result / why it's convincing
*two descriptors are enough: %V_bur(min) (steric) + V_min(Boltz) (electronic)*
two-node tree: *accuracy 81%, precision 66%, recall 97%* — almost no active ligands are missed
single descriptor alone: accuracy 66%, precision 51% → adding the second threshold cuts false positives significantly
validated on 28 new ligands per reaction: accuracy 83%, precision 85%
Figure 5: PCA and UMAP scatter active ligands all over the place → not useful for selection. The double threshold space clusters them cleanly

What might break when deployed outside the paper?
model was trained only on Ni-SMC with monophosphines → doesn't generalize to Pd-catalysis (explicitly shown, Figures S4–S9)
*fixed reaction conditions assumed* — changing solvent, temperature, base, etc. could shift the thresholds
only covers ligands in the kraken library — exotic or new ligand scaffolds may fall outside the descriptor space

What new errors/biases does ML introduce?
class weighting is user-defined (10:1 favor recall over precision) → this is a design choice that bakes in bias toward false positives
the y-cut (10% yield) is also chosen by the chemist — different cutoffs give different classifiers
upper-right quadrant (high steric + high electronic) is underrepresented in training data → model has less confidence there
P(4-FPh)₃ is a consistent outlier → fluorine substituent behaves differently than a simple inductive EWG, model doesn't capture this
