### Polymorphic antigen CCS analysis pipeline

---



#### Workflow 1



Start with a Galaxy collection of fastq files (post-LIMA demultiplexing).

Create a renaming file with column 1 listing the file names assigned to fastqs resulting from the demultiplexing in LIMA (e.g. "demultiplex.bc1001_Fwds--bc1041_Rvup.hifi_reads.fastq") and column 2 listing the corresponding sample's name for the specific barcode combination in the sequencing library (e.g. "V9_neat__synthetic_lib5_Plate2_A1"). Your PCR negative controls (water controls, samples with the word "H2O" anywhere in the name) will be used as a filter to discard samples with a readcount below background (defined as the mean number of reads in the water controls plus 2 SDs). 

> __These are your inputs__



The workflow will convert fastq to tabular files, count reads per sample and compute mean readcount and standard deviation in your negative controls.



You will use several outputs in workflow 2.



---





#### Workflow 2



> You will need a root for your phylogenetic tree.

> Here we used _Plasmodium billcolinsi msp2_ as the root for _Plasmodium falciparum msp2_. This FASTA file is named "msp2_PbillcollinsiG01.fasta"

> You will also need the genotyping oligos (in FASTA format) for your gene.

Start by uploading the root __gene__ FASTA file (your phylogenetic tree will be nucleotide sequence-based).



You will then upload your genoptying oligos (FASTA file called "genotyping_oligos_dimorphism.fasta") and the collapsed collection from workflow 1 as well as the file with reads per sample.



Expand step 5, _Sample_cov_thresholding_. 

 You should type in the value you see in the output _Sample_read_cov_threshold_ in your history bar to the right. *Ensure the condition reads __c2>=your number!__*



Expand step 6, _Seqs_from_covfilt_samples_. 

 Expand the output _Collapse Collection_ from workflow 1.

 Scroll to column 2.

 Here you see the CCS sample identity including the batch number and read identifier. You want to keep the __read__ identifier since it is unique.

 Copy and paste from _m#_\#_\#/_ and add to the first regex pattern box in the workflow, under _Filter Tabular Input Lines_. __Do not add anything to the replacement expression box.__

 

 Do the same with _/ccs_ into the second _Filter Tabular Input Lines_ regex pattern box. Again, do not add anything to the replacement expression box.



> You should keep the number between the two dashes // as this is the unique identity number for that ccs read.



Workflow 2 will give you many different outputs including read coverage matrices for size variants (files "sizevar_readcov_matrix_IC_post_thresholding", "sizevar_readcov_matrix_FC27_post_thresholding" and "sizevar_readcov_matrix_IC_FC27_post_thresholding") and sequence variants (file "seqvar_readcov_matrix_post_thresholding"), protein alignments (files "protseq_MAFFT_IC", "protseq_MAFFT_FC27" and "protseq_MAFFT_all_unique"), T cell epitope predictions (files "Consensus_MHC_II_binding_only" and "Consensus_MHC_II_binders_rank_equal_or_lower_to_1_percent"), phylogenetic trees (file "IC_FC27_phylogeny_MaxLikelihood") and estimates of sample multiplicity of infection (number of different P, falciparum clones in the isolate, file "MOI_c2_sizevar-based_c3_seqvar-based"). **You may run the workflow as it is



Nine different pipelines are provided for all the possible combinations of FPR at the size and sequence variant calling levels. The preset FPRs are 0.001, 0.01 and 0.05 (3x3 equals 9!! :))



_If you would like to run the phylogenetic calculations under different parameters, scroll down to steps 171, 172, 173 and 173 before running the workflow. Currently it is set to 1000x bootstrapping._



> The workflow will take several hours to run.

> _Have a coffee break if you like!_





---

Visualize your protein alignments using something like [Jalview](https://www.jalview.org/).



Look at your phylogenetic trees using software such as [ITOL Interactive Tree of Life](https://itol.embl.de/). 
