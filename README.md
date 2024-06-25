# Program SSCPE.pl 

Author Ugo Bastolla <ubastolla@cbm.csic.es>

The framework SSCPE.pl infers Maximum Likelihood (ML) phylogenetic trees adoping the Structure and Stability constrained model of protein evolution (SSCPE) [1] that predicts site-specific selective pressures by fitting only two global parameters for the whole protein: the selection strength Lambda_str for maintaining protein structure and Lambda_stab for maintaining the protein folding stability against unfolding and misfolding [5,6]. 

The ML trees may be inferred upon request through the program RAxML-NG (Kozlov et al. 2019 [2]. This program should be installed from https://github.com/amkozlov/raxml-ng For completeness, an executable version is included in the package, but you are strongly recommend that the updated version is downloaded from the official site. The SSCPE substitution models can also be input to the program PAML (Yang 2007 [3]).

### Installation: 

You have to run the following on your computer:
Download SSCPE.zip from  https://github.com/ugobas/SSCPE/blob/main/SSCPE.zip 
It is recommended to download SSCPE.zip in an empty folder called SSCPE
Run the following:

 >unzip SSCPE.zip
 >chmod u+x script_install_SSCPE.sh
 >script_install_SSCPE.sh

The installation script script_install_SSCPE.sh compiles the programs tnm and Prot_evol, stores the executables in the current folder and modifies the paths of the programs and the input files in SSCPE.pl as needed. The program raxml-ng included in the package is also extracted to the current folder, but it is recommended that you downloead it again from the officia site.
The executable is named SSCPE.pl you can copy it to your PATH.

## Description of the framework SSCPE.pl

For a given multiple sequence alignment (MSA) and list of protein structures (PDB file) whose sequences have local identity >50% with a sequence present in the MSA, the framework SSCPE.pl:

(1) runs the program Prot_evol (also available at https://github.com/ugobas/Prot_evol [1,7,8]) for predicting site-specific amino acid frequencies based on selection on protein structure conservation as predicted by the tnm program [4] and protein folding stability against unfolding and misfolding [5,6] for an input list of PDB structures and an MSA.

(2) Prot_evol internally runs the program tnm (Torsional Network Model [2], also available at https://github.com/ugobas/tnm) for predicting the structural deformation (RMSD and energy barrier DE between the average structure of the wild-type and the mutant) that would be produced by each possible mutation. The results of these computations (<>.mut_RMSD.dat and <>.mut_DE.dat) are stored in the folder /TNM_DATA where they are looked for by Prot_evol and reused if present, unless the option -noreuse2 is set. TNM computations are slow for proteins with >500 residues (about 1h for 1000 residues) and they can be disabled with the option -nostr , but in this case the structure-constrained models RMSD and DE and the combined models RMSDWT, RMSDMF, DEWT, DEMF cannot be used;

(3) There are 3 possible binary options for the exchangeability matrix, amounting to 8 combinations: the Halpern-Bruno model of the fixation probability (default, it may be disabled with -noHB), the flux model that requires that each pair of amino acids has the same site-averaged flux as th empirical model (default, it may be disabled with the option -noflux), RAxML-NG internally normalizes the substitution rate at each site and SSCPE.pl undoes this normalization by providing as input the substitution rate computed by Prot_vol (default, it may be disabled with the option -rate 0). The empirical exchangeability matrix can be input (e.g. -matrix JTT) or it can be internally optimized by Prot_evol (default);

(4) SSCPE.pl transforms the resulting substitution models in a format readable by the programs RAxML-NG (Kozlov et al. 2019 [5], https://github.com/amkozlov/raxml-ng) and PAML (Yang 2007 [6]).

Upon request (-raxml), RAxML-NG [2] can be run to infer the phylogenetic tree, if it is present in the same directory as SSCPE.pl. The executable of RAxML-NG must be downloaded from https://github.com/amkozlov/raxml-ng and stored in the same directory as SSCPE.pl under the name raxml-ng.

### References:
1. Lorca I, Arenas M and Bastolla U. 2022. Structure and stability constrained substitution models outperform traditional substitution models used for evolutionary inference. Submitted.
2. Kozlov AM, Darriba D, Flouri T, Morel B, Stamatakis A. 2019. RAxML-NG: a fast, scalable and user-friendly tool for maximum likelihood phylogenetic inference. Bioinformatics 35: 4453-4455.
3. Yang Z. 2007. PAML 4: phylogenetic analysis by maximum likelihood. Mol. Biol. Evol. 24:1586-1591.
4. Mendez R and Bastolla U. 2010. Torsional network model: normal modes in torsion angle space better correlate with conformation changes in proteins. Phys Rev Lett. 104:228103.
5. Minning J, Porto M, Bastolla U. 2013. Detecting selection for negative design in proteins through an improved model of the misfolded state. Proteins 81:1102-1112.
6. Bastolla U. 2014. Detecting selection on protein stability through statistical mechanical models of folding and evolution. Biomolecules 4:291-31.
7. Arenas M., Sanchez-Cobos A. and Bastolla, U. 2015.
Maximum likelihood phylogenetic inference with selection on protein folding stability. Mol Biol. Evol. 32:2195-207.
8. Arenas M, Bastolla U. 2020. ProtASR2: Ancestral reconstruction of protein sequences accounting for folding stability. Meth. Ecol Evol. 11:248-257.

## Usage:

SSCPE.pl 

	-ali <MSA file> (FASTA)
 	-pdblist <list of PDB files> line: pddbcode chain
	-pdb <single PDB file>
 	-chain <single PDB chain> (default: first chain)
	 -pdbdir <path to PDB> (optional)
	 -model <MOD> (allowed: MF WT DE RMSD DEWT RMSDWT DEMF RMSDMF default: ML)
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

## Example files

10 sample PDB files and small MSA (5 sequences) are provided in the folders PDB/ and ALI/. You can run as an example one of the following

SSCPE.pl -pdb PDB/1zio.pdb -ali ALI/1ZIO_A_mafft_reduced.fasta -raxml

SSCPE.pl -pdb PDB/1bp2.pdb -ali ALI/1BP2_A_mafft_reduced.fasta -raxml

 ## Other programs

The package SSCPE.zip also contains the following programs:

### program K2_mat.pl, 

It compares two phylogenetic trees by performing two-parameter fit of the corresponding branch lengths under a modification of the program K_mat.pl (Soria-Carrasco V, Talavera G, Igea J, Castresana J. 2007. The K tree score: quantification of differences in the relative branch length and topology of phylogenetic trees. Bioinformatics 23:2954-6.)

### master script script_run_SSCPE.pl 

For given MSA (input as -msa), list of PDB files (-pdblist) and folder where they are stored (-pdbdir), it runs a combination of several SSCPE models and empirical substitution models, which you can modify by editing the script, submits the computations to a computing cluster (you should also modify the corresponding commands), and compares the resulting trees with a reference tree (-tree), it also computes several other scores, including an approximated REGMLAME score.

### installation scripts

script_install_SSCPE.sh

script_write_dir.pl
