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

## Step 2. HBNet search at the interfaces of extracted adjacent building blocks
~/Rosetta/main/source/bin/rosetta_scripts.static.linuxgccrelease \
-in:file:s [input pdb model] \
-out::file::pdb_comments \
-run:preserve_header \
-use_input_sc \
-out:prefix HBNet_ \
-beta \
-missing_density_to_jump true \
-parser:protocol 2D_HBNet.xml \
-database [path to Rosetta database] \
-chemical:exclude_patches LowerDNA  UpperDNA Cterm_amidation VirtualBB ShoveBB VirtualDNAPhosphate VirtualNTerm CTermConnect sc_orbitals pro_hydroxylated_case1 pro_hydroxylated_case2 ser_phosphorylated thr_phosphorylated  tyr_phos
phorylated tyr_sulfated lys_dimethylated lys_monomethylated  lys_trimethylated lys_acetylated glu_carboxylated cys_acetylated tyr_diiodinated N_acetylated C_methylamidated MethylatedProteinCterm \
-in:file:fullatom \
-multi_cool_annealer 10\
-no_optH false\
-optH_MCA true\
-flip_HNQ

## Step 3. Regenerate the complete 2D lattice and map newly designed interfaces to all symmetric copies
~/Rosetta/main/source/bin/symm_seq_gen_2D.default.linuxgccrelease \
-database [path to Rosetta database] \
-s [input pdb model] \
-cn [symmetry of the building block, 2]

## Step 4. Symmetric design of the 2D lattice in the context of its symmetry
~/Rosetta/main/source/bin/symm_seq_gen_2D.default.linuxgccrelease \
-database [path to Rosetta database] \ 
-in:file:silent [input Rosetta silent file containing the 2D lattice] \
-parser:script_vars resfile=[input resfile to enfore newly designed interfaces stay intact] \
-out::file::pdb_comments \
-run:preserve_header \
-multi_cool_annealer 10 \
-use_input_sc \
-symmetry_definition dummy \
-out:prefix packed_ \
-beta
-missing_density_to_jump true  \
-symmetry:detect_bonds false \
-parser:protocol 2D_final_design.xml
