# 2DLattice
Program code used in the 2D lattice project

## Step 1. Rapid generation of connecting and non-clashing 2D lattices from protein building blocks
~/Rosetta/main/source/bin/flatland.static.linuxgccrelease
-in:file:s <input pdb model> -database <path to Rosetta database> \
-ignore_unrecognized_res -tag <user defined name tag for the job> \
  -C211_B <lattice parameter B for the C 1 2 layer group, takes a real number> -rot_step <search step size for the self-rotation of the building block, takes a real number> -symmetry_definition dummy -output_virtual -Cn <internal cyclic symmetry of the building block, 2> -wallpaper <layer symmetry of the final 2D lattice, C211> \
-mh:path:scores_BB_BB /gscratch/baker/zibochen/utilities/aa_count_ACDEFHIKLMNQRSTVWY_resl1_ang15_msc0.2_smooth1.3_ROSETTA/aa_count_ACDEFHIKLMNQRSTVWY_resl1_ang15_msc0.2_smooth1.3_ROSETTA -mh:score:use_ss1 true -mh:score:use_ss2 true -mh:score:use_aa1 false -mh:score:use_aa2 false #motif score specific options\
-dump_silent true -dump_everything false -cell_upper 100 -single_chain_version false -cell_step 0.5
