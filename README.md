# 2DLattice
Program code used in the 2D lattice project

## Step 1. Rapid generation of connecting and non-clashing 2D lattices from protein building blocks
~/Rosetta/main/source/bin/flatland.static.linuxgccrelease \
-in:file:s [input pdb model] \
-database [path to Rosetta database] \
-ignore_unrecognized_res \
-mh:path:scores_BB_BB /gscratch/baker/zibochen/utilities/aa_count_ACDEFHIKLMNQRSTVWY_resl1_ang15_msc0.2_smooth1.3_ROSETTA/aa_count_ACDEFHIKLMNQRSTVWY_resl1_ang15_msc0.2_smooth1.3_ROSETTA -mh:score:use_ss1 true -mh:score:use_ss2 true -mh:score:use_aa1 false -mh:score:use_aa2 false #motif score specific options\
 -symmetry_definition dummy \
 -output_virtual \
-tag [user defined name tag for the job] \
 -rot_step [search step size for the self-rotation of the building block, takes a real number]\
 -Cn [internal cyclic symmetry of the building block, 2] \
 -wallpaper [layer symmetry of the final 2D lattice, C211] \
-dump_silent [dump a silent file containing all the lattices, boolean] \
 -C211_B [lattice parameter B for the C 1 2 layer group, takes a real number] \
-cell_upper [upper limit for the cell dimensions, takes a real number] \
-single_chain_version [if the input model is monomerized, the code accomondates for this psudeo-symmetry. Boolean] \
-cell_step [search step size for the lattice cell dimensions, takes a real number]\
