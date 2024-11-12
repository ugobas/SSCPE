## Usage:

```sh
SSCPE.pl  
	 -ali <MSA file> (FASTA)  
	 -pdblist <list of PDB files> line: pddbcode chain  
	 -pdb <single PDB file>  
	 -chain <single PDB chain> (default: first chain)  
	 -pdbdir <path to PDB> (if the PDB file is not in current folder)  
	 -model <MOD> (allowed: MF WT DE RMSD DEWT RMSDWT DEMF RMSDMF)  
	 -raxml ! infer ML tree running RAxML-NG  
	 -index <index of output files (optional)>  
	 -nostr  (Do not run TNM if its results do not exist)  
	 -noreuse  (Do not reuse Prot_evol results even if exist)  
	 -noreuse2 (Do not reuse Prot_evol & TNM results even if exist)  
	 -option <one line file with other RAxML options> (optional)  
	 -thread (number of processors used by RAxML-NG, default 4)  
	 -noclean (Do not clean RAxML partitions after use)  

	 Options fot the exchangeability matrix:  
	 -matrix <global subst. matrix: LG WAG JTT OPT (default OPT)>  
	 -rate <1,0> 1: BU=site-spec subst.rate; 0: BU=1;  
	 -nohb   (Do not include fixation prob. with Halpern-Bruno formula)  
	 -noflux (Do not use flux of emp. model instead of exchangeability)  
```

If `-model` is not specified, the program estimates the approximate likelihood of all SSCPE models and adopts the model that maximizes this score.

## Example files

10 sample PDB files and small MSA (5 sequences) are provided in the folders PDB/ and ALI/. You can run as an example one of the following

```sh  
perl SSCPE.pl -pdb PDB/1zio.pdb -ali ALI/1ZIO_A_mafft_reduced.fasta -raxml  
perl SSCPE.pl -pdb PDB/1bp2.pdb -ali ALI/1BP2_A_mafft_reduced.fasta -raxml  
```  

## Other programs  

The package SSCPE.zip also contains the following programs:

### program K2_mat.pl

It compares two phylogenetic trees by performing two-parameter fit of the corresponding branch lengths under a modification of the program K_mat.pl (Soria-Carrasco V, Talavera G, Igea J, Castresana J. 2007. The K tree score: quantification of differences in the relative branch length and topology of phylogenetic trees. Bioinformatics 23:2954-6.)

### master script `script_run_SSCPE_models.pl`

For given MSA (input as -msa), list of PDB files (-pdblist) and folder where they are stored (-pdbdir), it runs a combination of several SSCPE models and empirical substitution models, which you can modify by editing the script, it generates one job for each model and submits the computations to a computing cluster (you should also modify the corresponding line `$run=...` as needed).  

For sending the computations to a queue system, the master script uses the provided PERL program `qsubmit.pl`.

If `script_run_SSCPE_models.pl` is run with the option `-examine`, it reads the ML trees computed for each model in the computation run and compares them with the reference tree specified with `-tree <>` by runnning the program `K2_mat.pl` and reading the generated files. It also computes several other scores, including an approximated REGMLAME score, where the parameter "\mu" is obtained by fitting the log-likelihood as a a function of the mean branch length across all generated trees.

### installation scripts

script_install_SSCPE.sh  
script_write_dir.pl  
