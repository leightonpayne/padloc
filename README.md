<a href="https://github.com/padlocbio/padloc/blob/master/LICENSE" alt="License"><img src="https://img.shields.io/github/license/padlocbio/padloc" /></a> <a href="https://anaconda.org/padlocbio/padloc" alt="Conda release"><img src="https://img.shields.io/conda/vn/padlocbio/padloc?color=yellow&label=conda%20release"></a> <a href="https://github.com/padlocbio/padloc/releases" alt="Github release"><img src="https://img.shields.io/github/v/release/padlocbio/padloc?label=github%20release" /></a> <a href="https://github.com/padlocbio/padloc/commits/master" alt="GitHub commits since latest release"><img src="https://img.shields.io/github/commits-since/padlocbio/padloc/latest?sort=semver"></a> <a href="https://github.com/padlocbio/padloc/" alt="Last update"><img src="https://img.shields.io/github/last-commit/padlocbio/padloc?label=last%20update" /></a> 


# PADLOC: Prokaryotic Antiviral Defence LOCator

> [!NOTE]
> Highlights information that users should take into account, even when skimming.

> [!IMPORTANT]
> Crucial information necessary for users to succeed.

> [!WARNING]
> Critical content demanding immediate user attention due to potential risks.

## About

[PADLOC](https://github.com/padlocbio/padloc) is a software tool for identifying antiviral defence systems in prokaryotic genomes. [PADLOC](https://github.com/padlocbio/padloc) screens genomes against a database of HMMs and system classifications to find and annotate defence systems based on sequence homology and genetic architecture.

[PADLOC](https://github.com/padlocbio/padloc) can be installed and used via the command line or via our [web server](https://padloc.otago.ac.nz).

## Citation

If you use [PADLOC](https://github.com/padlocbio/padloc) or [PADLOC-DB](https://github.com/padlocbio/padloc-db) please cite:

> Payne, L. J., Todeschini, T. C., Wu, Y., Perry, B. J., Ronson, C. W., Fineran, P. C., Nobrega, F. L., Jackson, S. A. (2021) Identification and classification of antiviral defence systems in bacteria and archaea with PADLOC reveals new system types. *Nucleic Acids Research*, **49**, 10868-10878. doi: https://doi.org/10/gzgh

The HMMs in [PADLOC-DB](https://github.com/padlocbio/padloc-db) were built/curated using data from various sources, we encourage you to also give credit to these groups by [citing them too](#References).

## Installation

### Conda (recommended)

It is recommended that [PADLOC](https://github.com/padlocbio/padloc) be installed via conda.

```bash
# Install PADLOC into a new conda environment
conda create -n padloc -c conda-forge -c bioconda -c padlocbio padloc
# Activate the environment
conda activate padloc
# Download the latest database
padloc --db-update
```

### GitHub

The latest development version of [PADLOC](https://github.com/padlocbio/padloc) can be installed by cloning this GitHub repository and installing the dependencies manually.

```bash
# Clone repo to $HOME
git clone https://github.com/padlocbio/padloc $HOME/padloc
# Add to $PATH
export PATH="$HOME/padloc/bin:$PATH"
# Download the latest database
padloc --db-update
```

## Examples

```bash
# BASIC: Search an amino acid fasta file with accompanying GFF annotations
padloc --faa genome.faa --gff features.gff
```

```bash
# BASIC: Search a nucleic acid fasta file, identifying CDS with prodigal
padloc --fna genome.fna
```

```bash
# BASIC: Include CRISPR array information from CRISPRDetect output
padloc --faa genome.faa --gff features.gff --crispr arrays.gff
```

```bash
# INTERMEDIATE: Use multiple cpus and save output to a different directory
padloc --faa genome.faa --gff features.gff --outdir path_to_output --cpu 4
```

```bash
# ADVANCED: Use your own HMMs and system models
padloc --faa genome.faa --gff features.gff --data path_to_database
```

## Test

```bash
# Try running PADLOC on the test data provided
padloc --faa padloc/test/GCF_001688665.2.faa --gff padloc/test/GCF_001688665.2.gff
padloc --fna padloc/test/GCF_004358345.1.fna
```

## Options

```
General:
    --help            Print this help message
    --version         Print version information
    --citation        Print citation information
    --check-deps      Check that dependencies are installed
    --debug           Run with debug messages
Database:
    --db-list         List all PADLOC-DB releases
    --db-install [n]  Install specific PADLOC-DB release [n]
    --db-update       Install latest PADLOC-DB release
    --db-version      Print database version information
Input:
    --faa [f]         Amino acid FASTA file (only valid with [--gff])
    --gff [f]         GFF file (only valid with [--faa])
    --fna [f]         Nucleic acid FASTA file
    --crispr [f]      CRISPR array input file (.gff from CRISPRDetect)
Output:
    --outdir [d]      Output directory
Optional:
    --data [d]        Data directory
    --cpu [n]         Use [n] CPUs (default '1')
    --raw-out         Include a summarised raw output file
    --fix-prodigal    Set this flag when providing an FAA and GFF file 
                      generated with prodigal to force fixing of sequence IDs
```

## Output

| File           | Description                                         |
| -------------- | --------------------------------------------------- |
| *.domtblout    | Domain table file generated by HMMER.               |
| *_prodigal.faa | Amino acid FASTA file generated by prodigal.        |
| *_prodigal.gff | GFF annotation file generated by prodigal.          |
| *_padloc.csv   | PADLOC output file for identified defence systems.  |
| *_padloc.gff   | GFF annotation file for identified defence systems. |

## Interpreting Output

| Column               | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `system.number`      | Distinct system number.                                      |
| `seqid`              | Sequence ID of the contig.                                   |
| `system`             | Name of the system identified.                               |
| `target.name`        | Protein ID.                                                  |
| `hmm.accession`      | PADLOC HMM accession number.                                 |
| `hmm.name`           | PADLOC HMM name.                                             |
| `protein.name`       | Defence system protein name.                                 |
| `full.seq.E.value`   | Full sequence E-value. From the HMMER Documentation: "The E-value is a measure of statistical signiﬁcance. The lower the E-value, the more signiﬁcant the hit." |
| `domain.iE.value`    | Domain E-value. From the HMMER Documentation: "If the full sequence E-value is signiﬁcant but the single best domain E-value is not, the target sequence is probably a multidomain remote homolog". |
| `target.coverage`    | Fraction of the target sequence aligning to the HMM.         |
| `hmm.coverage`       | Fraction of the HMM aligning to the target sequence.         |
| `start`              | Start position of the target sequence in the contig.         |
| `end`                | End position of the target sequence in the contig.           |
| `strand`             | Strand; forward (+) or reverse (-)                           |
| `target.description` | Target sequence descrition taken from the input file.        |
| `relative.position`  | Relative position of the target sequence in the contig.      |
| `contig.end`         | Relative position of the last sequence in the contig.        |
| `all.domains`        | Concatenated list of all domains identified with HMMER.      |
| `best.hits`          | Top 5 hits identified with HMMER.                            |

## PADLOC-DB

The HMMs and defence system models used by [PADLOC](https://github.com/padlocbio/padloc) are available from the repository [PADLOC-DB](https://github.com/leightonpayne/padloc-db). The latest version of the database can be downloaded by running `padloc --db-update`. Alternatively, a custom database can be specified with `--data [d]`, refer to [PADLOC-DB](https://github.com/leightonpayne/padloc-db) for more information about configuring a custom database.

## FAQ

- **What are the requirements for an FAA/GFF file pair as input?**

  The GFF file should conform to the [GFF3 specification](https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md). Each sequence in the FAA file is matched to an entry in the GFF file based on its ID attribute e.g. for the following sequence:

  ```
  >WP_000000001.1 molybdopterin-dependent oxidoreductase, partial [Escherichia coli]
  AAAAAAAGLSVPGVARAVLVSRKPSNGIKAPCRFCGTGCGVLVGTQQGRVVACQGDPDAPVNRGLNCIKG
  YFLPKIMYGKDRLTQPLLRMKNGKYDKEGEFTPITWDQAFDVMEEKFKTALKEKGPESIGMFGSGQWTIW
  EGYAASKLFKAGFRSNNIDPNARHCMASAVVGFMRTFGMDEPMGCYDDIEQADAFVLWGANMAEMHPILW
  SRITNRRLSN
  ```

  The corresponding entry in the GFF file should contain an ID attribute of the form:

  `ID=WP_000000001.1` *or* ``ID=cds-WP_000000001.1 ``

  FAA/GFF combinations that are known to work 'out-of-the-box' are from genomes annotated with:

  - [NCBI's prokaryotic genome annotation pipeline](https://doi.org/10.1093/nar/gkw569) (i.e. genomes from [RefSeq](https://www.ncbi.nlm.nih.gov/refseq/) and [GenBank](https://www.ncbi.nlm.nih.gov/genbank/)) 
  - [JGI's IMG annotation pipeline](https://img.jgi.doe.gov/docs/pipelineV5/) (i.e. genomes from [IMG](https://img.jgi.doe.gov/cgi-bin/mer/main.cgi?section=TreeFile&page=domain&domain=all))
  - [Prokka](https://github.com/tseemann/prokka)

- **Why are there parsing failures when using a GFF file from prokka?**

  The following warning may be thrown when using a GFF file generated by prokka:

  ```
  Warning: 46324 parsing failures.
   row col  expected    actual         file
  2612  -- 9 columns 1 columns 'prokka.gff'
  2613  -- 9 columns 1 columns 'prokka.gff'
  2614  -- 9 columns 1 columns 'prokka.gff'
  2615  -- 9 columns 1 columns 'prokka.gff'
  2616  -- 9 columns 1 columns 'prokka.gff'
  .... ... ......... ......... ............
  See problems(...) for more details.
  ```

  This is because these GFF files are appended with the contig sequences of the annotated genome. This warning can be avoided by removing the contig sequences from the GFF file with:

  ```bash
  sed '/^##FASTA/Q' prokka.gff > nosequence.gff
  ```

- **Why can't I use a nucleotide FASTA file with < 100 kbp?**

  According to [Prodigal's own documentation](https://github.com/hyattpd/prodigal/wiki/Advice-by-Input-Type#plasmids-phages-viruses-and-other-short-sequences), sequences < 100 kbp are *"too short to gather enough statistics to predict genes well"*. To avoid issues arising from this, PADLOC won't try to run prodigal over anything < 100 kbp. 

  If you know what you're doing then you can use Prodigal or another gene prediction program to generate your own FAA and GFF files to then use with PADLOC.

## Issues

Bugs and feature requests can be submitted to the [Issues tab](https://github.com/leightonpayne/padloc/issues) (see [Sample bug report](/../../issues/6)).

## Dependencies

These dependencies are installed automatically when using conda.

### Mandatory

- **R >= 4.1.0**  
  *R Core Team (2018). R: A language and environment for statistical computing. R Foundation for Statistical Computing, Vienna, Austria. https://www.R-project.org/.*
  - **tidyverse >= 1.3.1**  
    *Wickham et al. (2019). Welcome to the tidyverse. Journal of Open Source Software, 4(43), 1686, https://doi.org/10.21105/joss.01686.*
  - **yaml >= 2.2.1**  
    *Stephens, J., et al. (2020). yaml: Methods to Convert R Data to YAML and Back. https://CRAN.R-project.org/package=yaml.*
  - **getopt >= 1.20.3**  
    *Davis, T., et al. (2019). getopt: C-Like 'getopt' Behavior. https://CRAN.R-project.org/package=getopt.*
- **HMMER >= 3.3.2**  
*Finn, R.D., Clements, J., and Eddy, S.R. (2011). HMMER web server: interactive sequence similarity searching. Nucleic Acids Res 39, W29–W37.*

### Optional

- **Prodigal >= 2.6.3**  
  *Hyatt, D., Chen, GL., Locascio, P.F., Land, M.L., Larimer, F.W., and Hauser, L.J. (2010) Prodigal: prokaryotic gene recognition and translation initiation site identification. BMC Bioinformatics 11, 119.*

## References

The HMMs in [PADLOC-DB](https://github.com/padlocbio/padloc-db) were built/curated using data from various sources, we encourage you to also give credit to these groups by citing them too:

> Doron, S., Melamed, S., Ofir, G., Leavitt, A., Lopatina, A., Keren, M., Amitai, G. and Sorek, R. (2018) Systematic discovery of antiphage defense systems in the microbial pangenome. *Science*, **359**, eaar4120. doi: [10/ggqhzm](https://doi.org/10/ggqhzm)

> Millman, A., Melamed, S., Amitai, G. and Sorek, R. (2020) Diversity and classification of cyclic-oligonucleotide-based anti-phage signalling systems. *Nature Microbiology*, **5**, 1608–1615. doi: [10/gg84nk](https://doi.org/10/gg84nk)

> Couvin, D., Bernheim, A., Toffano-Nioche, C., Touchon, M., Michalik, J., Néron, B., Rocha, E. P. C., Vergnaud, G., Gautheret, D. and Pourcel, C. (2018) CRISPRCasFinder, an update of CRISRFinder, includes a portable version, enhanced performance and integrates search for Cas proteins. *Nucleic Acids Res*, **46**, W246–W251. doi: [10/ggdjdf](https://doi.org/10/ggdjdf)

> Makarova, K. S., Wolf, Y. I., Iranzo, J., Shmakov, S. A., Alkhnbashi, O. S., Brouns, S. J. J., Charpentier, E., Cheng, D., Haft, D. H., Horvath, P., et al. (2020) Evolutionary classification of CRISPR–Cas systems: a burst of class 2 and derived variants. *Nat Rev Microbiol*, **18**, 67–83. doi: [10/ggkfgj](https://doi.org/10/ggkfgj)

> Shah, S. A., Alkhnbashi, O. S., Behler, J., Han, W., She, Q., Hess, W. R., Garrett, R. A. and Backofen, R. (2019) Comprehensive search for accessory proteins encoded with archaeal and bacterial type III CRISPR-cas gene cassettes reveals 39 new cas gene families. *RNA Biology*, **16**, 530–542. doi: [10/ggqv9p](https://doi.org/10/ggqv9p)

> Shmakov, S. A., Makarova, K. S., Wolf, Y. I., Severinov, K. V. and Koonin, E. V. (2018) Systematic prediction of genes functionally linked to CRISPR-Cas systems by gene neighborhood analysis. *PNAS*, **115**, E5307–E5316. doi: [10/gdpqwq](https://doi.org/10/gdpqwq)

> Russel, J., Pinilla-Redondo, R., Mayo-Muñoz, D., Shah, S. A. and Sørensen, S. J. (2020) CRISPRCasTyper: Automated Identification, Annotation, and Classification of CRISPR-Cas Loci. *The CRISPR Journal*, **3**, 462–469. doi: [10/gshm](https://doi.org/10/gshm)

> Gao, L., Altae-Tran, H., Böhning, F., Makarova, K. S., Segel, M., Schmid-Burgk, J. L., Koob, J., Wolf, Y. I., Koonin, E. V. and Zhang, F. (2020) Diverse enzymatic activities mediate antiviral immunity in prokaryotes. *Science*, **369**, 1077–1084. doi: [10/gpsx](https://doi.org/10/gpsx)

> Makarova, K. S., Timinskas, A., Wolf, Y. I., Gussow, A. B., Siksnys, V., Venclovas, Č. and Koonin, E. V. (2020) Evolutionary and functional classification of the CARF domain superfamily, key sensors in prokaryotic antivirus defense. *Nucleic Acids Res*, **48**, 8828–8847. doi: [10/gg7qx6](https://doi.org/10/gg7qx6)

> Bouchard, J. D., Dion, E., Bissonnette, F. and Moineau, S. (2002) Characterization of the Two-Component Abortive Phage Infection Mechanism AbiT from Lactococcus lactis. *Journal of Bacteriology*, **184**, 6325–6332. doi: [10/btq97w](https://doi.org/10/btq97w)

> Parma, D. H., Snyder, M., Sobolevski, S., Nawroz, M., Brody, E. and Gold, L. (1992) The Rex system of bacteriophage lambda: tolerance and altruistic cell death. *Genes Dev.*, **6**, 497–510. doi: [10/b9xpsb](https://doi.org/10/b9xpsb)

> Jabbar, M. A. and Snyder, L. (1984) Genetic and physiological studies of an Escherichia coli locus that restricts polynucleotide kinase- and RNA ligase-deficient mutants of bacteriophage T4. *Journal of Virology*, **51**, 522–529. doi: [10/gndc4t](https://doi.org/10/gndc4t)

> Cram, D., Ray, A. and Skurray, R. (1984) Molecular analysis of F plasmid pif region specifying abortive infection of T7 phage. *Mol Gen Genet*, **197**, 137–142. doi: [10/fg7s8g](https://doi.org/10/fg7s8g)

> Smith, H. S., Pizer, L. I., Pylkas, L. and Lederberg, S. (1969) Abortive Infection of Shigella dysenteriae P2 by T2 Bacteriophage. *Journal of Virology*, **4**, 162–168. doi: [10/gndc4r](https://doi.org/10/gndc4r)

> Dai, G., Su, P., Allison, G. E., Geller, B. L., Zhu, P., Kim, W. S. and Dunn, N. W. (2001) Molecular Characterization of a New Abortive Infection System (AbiU) from Lactococcus lactisLL51-1. *Applied and Environmental Microbiology*, **67**, 5225–5232. doi: [10/cwbxdg](https://doi.org/10/cwbxdg)

> Lindahl, G., Sironi, G., Bialy, H. and Calendar, R. (1970) Bacteriophage Lambda; Abortive Infection of Bacteria Lysogenic for Phage P2. *PNAS*, **66**, 587–594. doi: [10/fkgq7x](https://doi.org/10/fkgq7x)

> Bergsland, K. J., Kao, C., Yu, Y. T. N., Gulati, R. and Snyder, L. (1990) A site in the T4 bacteriophage major head protein gene that can promote the inhibition of all translation in Escherichia coli. *Journal of Molecular Biology*, **213**, 477–494. doi: [10/fv36tb](https://doi.org/10/fv36tb)

> Durmaz, E. and Klaenhammer, T. R. (2007) Abortive Phage Resistance Mechanism AbiZ Speeds the Lysis Clock To Cause Premature Lysis of Phage-Infected Lactococcus lactis. *Journal of Bacteriology*, **189**, 1417–1425. doi: [10/dnndhw](https://doi.org/10/dnndhw)

> Haaber, J., Moineau, S., Fortier, L. C. and Hammer, K. (2008) AbiV, a Novel Antiphage Abortive Infection Mechanism on the Chromosome of Lactococcus lactis subsp. cremoris MG1363. *Applied and Environmental Microbiology*, **74**, 6528–6537. doi: [10/cnwcq7](https://doi.org/10/cnwcq7)

> Emond, E., Dion, E., Walker, S. A., Vedamuthu, E.R., Kondo, J.K. and Moineau, S. (1998) AbiQ, an Abortive Infection Mechanism fromLactococcus lactis. *Applied and Environmental Microbiology*, **64**, 4748–4756. doi: [10/gndc4p](https://doi.org/10/gndc4p)

> Domingues, S., Chopin, A., Ehrlich, S. D. and Chopin, M. C. (2004) The Lactococcal Abortive Phage Infection System AbiP Prevents both Phage DNA Replication and Temporal Transcription Switch. *Journal of Bacteriology*, **186**, 713–721. doi: [10/bjjwc6](https://doi.org/10/bjjwc6)

> Prevots, F. and Ritzenthaler, P. (1998) Complete Sequence of the New Lactococcal Abortive Phage Resistance Gene abiO. *Journal of Dairy Science*, **81**, 1483–1485. doi: [10/cg69rg](https://doi.org/10/cg69rg)

> Prévots, F., Tolou, S., Delpech, B., Kaghad, M. and Daloyau, M. (1998) Nucleotide sequence and analysis of the new chromosomal abortive infection gene abiN of Lactococcus lactis subsp. cremoris S114. *FEMS Microbiology Letters*, **159**, 331–336. doi: [10/c94sd6](https://doi.org/10/c94sd6)

> Deng, Y. M., Liu, C. Q. and W. Dunn, N. (1999) Genetic organization and functional analysis of a novel phage abortive infection system, AbiL, from Lactococcus lactis. *Journal of Biotechnology*, **67**, 135–149. doi: [10/bwc9k8](https://doi.org/10/bwc9k8)

> Emond, E., Holler, B. J., Boucher, I., Vandenbergh, P. A., Vedamuthu, E. R., Kondo, J. K. and Moineau, S. (1997) Phenotypic and genetic characterization of the bacteriophage abortive infection mechanism AbiK from Lactococcus lactis. *Applied and Environmental Microbiology*, **63**, 1274–1283. doi: [10/gndc4n](https://doi.org/10/gndc4n)

> Deng, Y. M., Harvey, M. L., Liu, C. Q. and Dunn, N. W. (1997) A novel plasmid-encoded phage abortive infection system from Lactococcus lactis biovar. diacetylactis. *FEMS Microbiology Letters*, **146**, 149–154. doi: [10/d24tcq](https://doi.org/10/d24tcq)

> Su, P., Harvey, M., Im, H. J. and Dunn, N. W. (1997) Isolation, cloning and characterisation of the abiI gene from Lactococcus lactis subsp. lactis M138 encoding abortive phage infection. *Journal of Biotechnology*, **54**, 95–104. doi: [10/ckwmpp](https://doi.org/10/ckwmpp)

> Prévots, F., Daloyau, M., Bonin, O., Dumont, X. and Tolou, S. (1996) Cloning and sequencing of the novel abortive infection gene abiH of Lactococcus lactis ssp. lactis biovar. diacetylactis S94. *FEMS Microbiology Letters*, **142**, 295–299. doi: [10/dvjcb5](https://doi.org/10/dvjcb5)

> O’Connor, L., Coffey, A., Daly, C. and Fitzgerald, G. F. (1996) AbiG, a genotypically novel abortive infection mechanism encoded by plasmid pCI750 of Lactococcus lactis subsp. cremoris UC653. *Applied and Environmental Microbiology*, **62**, 3075–3082. doi: [10/gndc4m](https://doi.org/10/gndc4m)

> Garvey, P., Fitzgerald, G. F. and Hill, C. (1995) Cloning and DNA sequence analysis of two abortive infection phage resistance determinants from the lactococcal plasmid pNP40. *Applied and Environmental Microbiology*, **61**, 4321–4328. doi: [10/gndc3x](https://doi.org/10/gndc3x)

> Dy, R. L., Przybilski, R., Semeijn, K., Salmond, G. P. C. and Fineran, P. C. (2014) A widespread bacteriophage abortive infection system functions through a Type IV toxin–antitoxin mechanism. *Nucleic Acids Research*, **42**, 4590–4605. doi: [10/f5zkmw](https://doi.org/10/f5zkmw)

> McLandsborough, L.A., Kolaetis, K. M., Requena, T. and McKay, L. L. (1995) Cloning and characterization of the abortive infection genetic determinant abiD isolated from pBF61 of Lactococcus lactis subsp. lactis KR5. *Applied and Environmental Microbiology*, **61**, 2023–2026. doi: [10/gndc4j](https://doi.org/10/gndc4j)

> Garvey, P., Fitzgerald, G. F. and Hill, C. (1995) Cloning and DNA sequence analysis of two abortive infection phage resistance determinants from the lactococcal plasmid pNP40. *Applied and Environmental Microbiology*, **61**, 4321–4328. doi: [10/gndc3x](https://doi.org/10/gndc3x)

> Anba, J., Bidnenko, E., Hillier, A., Ehrlich, D. and Chopin, M. C. (1995) Characterization of the lactococcal abiD1 gene coding for phage abortive infection. *Journal of Bacteriology*, **177**, 3818–3823. doi: [10/gndc3w](https://doi.org/10/gndc3w)

> Durmaz, E., Higgins, D. L. and Klaenhammer, T. R. (1992) Molecular characterization of a second abortive phage resistance gene present in Lactococcus lactis subsp. lactis ME2. *Journal of Bacteriology*, **174**, 7463–7469. doi: [10/gndc3v](https://doi.org/10/gndc3v)

> Parreira, R., Ehrlich, S. D. and Chopin, M. C. (1996) Dramatic decay of phage transcripts in lactococcal cells carrying the abortive infection determinant AbiB. *Molecular Microbiology*, **19**, 221–230. doi: [10/b835bf](https://doi.org/10/b835bf)

> Cluzel, P. J., Chopin, A., Ehrlich, S. D. and Chopin, M. C. (1991) Phage abortive infection mechanism from Lactococcus lactis subsp. lactis, expression of which is mediated by an Iso-ISS1 element. *Applied and Environmental Microbiology*, **57**, 3547–3551. doi: [10/gndc3t](https://doi.org/10/gndc3t)

> Dinsmore, P. K. and Klaenhammer, T. R. (1994) Phenotypic Consequences of Altering the Copy Number of abiA, a Gene Responsible for Aborting Bacteriophage Infections in Lactococcus lactis. *Applied and Environmental Microbiology*, **60**, 1129–1136. doi: [10/gndc3s](https://doi.org/10/gndc3s)

> Owen, S. V., Wenner, N., Dulberger, C. L., Rodwell, E. V., Bowers-Barnard, A., Quinones-Olvera, N., Rigden, D. J., Rubin, E. J., Garner, E. C., Baym, M., et al. (2021) Prophages encode phage-defense systems with cognate self-immunity. *Cell Host & Microbe*, **29**, 1620-1633. doi: [10/g29b](https://doi.org/10/g29b)

> Bari, S. M. N., Chou-Zheng, L., Howell, O., Cater, K., Dandu, V. S., Thomas, A., Aslan, B., and Hatoum-Aslan, A. (2021). A unique mode of nucleic acid immunity performed by a single multifunctional enzyme. *bioRxiv*. doi: [10/gkw6wr](https://doi.org/10/gkw6wr)

> Burroughs A. M., Ando Y., Aravind L. (2014) New perspectives on the diversification of the RNA interference system: insights from comparative genomics and small RNA sequencing. *WIREs RNA*. **5**, 141–181. doi: [10/gfkh7c](https://doi.org/10/gfkh7c)

> Makarova, K. S., Wolf, Y. I., van der Oost, J., and Koonin, E. V. (2009). Prokaryotic homologs of Argonaute proteins are predicted to function as key components of a novel system of defense against mobile genetic elements. *Biol Direct*, **4**, 29. doi: [10/cvfhm6](https://doi.org/10/cvfhm6)

> Burroughs, A. M., Iyer, L. M., and Aravind, L. (2013). Two novel PIWI families: roles in inter-genomic conflicts in bacteria and Mediator-dependent modulation of transcription in eukaryotes. *Biology Direct* **8**, 13. doi: [10/f46qcd](https://doi.org/10/f46qcd)

> Zeng, Z., Chen, Y., Pinilla-Redondo, R., Shah, S. A., Zhao, F., Wang, C., Hu, Z., Zhang, C., Whitaker, R. J., She, Q., et al. (2021). A short prokaryotic argonaute cooperates with membrane effector to confer antiviral defense. *bioRxiv*. doi: [10//hgr9](https://doi.org//hgr9)

> Mestre, M. R., González-Delgado, A., Gutiérrez-Rus, L. I., Martínez-Abarca, F., and Toro, N. (2020). Systematic prediction of genes functionally associated with bacterial retrons and classification of the encoded tripartite systems. *Nucleic Acids Research*, **48**, 12632–12647. doi: [10/fzk3](https://doi.org/10/fzk3)

> Millman, A., Bernheim, A., Stokar-Avihail, A., Fedorenko, T., Voichek, M., Leavitt, A., Oppenheimer-Shaanan, Y., and Sorek, R. (2020). Bacterial Retrons Function In Anti-Phage Defense. *Cell*, **183**, 1551-1561.e12. doi: [10/ghjct3](https://doi.org/10/ghjct3)

> Bernheim, A., Millman, A., Ofir, G., Meitav, G., Avraham, C., Shomar, H., Rosenberg, M. M., Tal, N., Melamed, S., Amitai, G., et al. (2021). Prokaryotic viperins produce diverse antiviral molecules. *Nature*, **589**, 120–124. doi: [10/d9ss](https://doi.org/10/d9ss)

> Goldfarb, T., Sberro, H., Weinstock, E., Cohen, O., Doron, S., Charpak‐Amikam, Y., Afik, S., Ofir, G., and Sorek, R. (2015). BREX is a novel phage resistance system widespread in microbial genomes. *EMBO J*, **34**, 169–183. doi: [10/f2wkj3](https://doi.org/10/f2wkj3)

> Ofir, G., Melamed, S., Sberro, H., Mukamel, Z., Silverman, S., Yaakov, G., Doron, S., and Sorek, R. (2018). DISARM is a widespread bacterial defence system with broad anti-phage activities. *Nat Microbiol*, **3**, 90–98. doi: [10/ggnszb](https://doi.org/10/ggnszb)

> Yuan, Y., Hutinet, G., Valera, J. G., Hu, J., Hillebrand, R., Gustafson, A., Iwata-Reuyl, D., Dedon, P. C., and de Crécy-Lagard, V. (2018). Identification of the minimal bacterial 2′-deoxy-7-amido-7-deazaguanine synthesis machinery. *Molecular Microbiology*, **110**, 469–483. doi: [10/gfft6b](https://doi.org/10/gfft6b)

> Thiaville, J. J., Kellner, S. M., Yuan, Y., Hutinet, G., Thiaville, P. C., Jumpathong, W., Mohapatra, S., Brochier-Armanet, C., Letarov, A. V., Hillebrand, R., et al. (2016). Novel genomic island modifies DNA with 7-deazaguanine derivatives. *Proc Natl Acad Sci USA*, **113**, E1452–E1459. doi: [10/ggr4f7](https://doi.org/10/ggr4f7)

> Tong, T., Chen, S., Wang, L., Tang, Y., Ryu, J. Y., Jiang, S., Wu, X., Chen, C., Luo, J., Deng, Z., et al. (2018). Occurrence, evolution, and functions of DNA phosphorothioate epigenetics in bacteria. *Proc Natl Acad Sci USA*, **115**, E2988–E2996. doi: [10/gdbj2n](https://doi.org/10/gdbj2n)

> Xu, T., Yao, F., Zhou, X., Deng, Z., and You, D. (2010). A novel host-specific restriction system associated with DNA backbone S-modification in Salmonella. *Nucleic Acids Research*, **38**, 7133–7141. doi: [10/d5zqcp](https://doi.org/10/d5zqcp)

> Xiong, L., Liu, S., Chen, S., Xiao, Y., Zhu, B., Gao, Y., Zhang, Y., Chen, B., Luo, J., Deng, Z., et al. (2019). A new type of DNA phosphorothioation-based antiviral system in archaea. *Nat Commun*, **10**, 1688. [10/ggctjz](https://doi.org/10/ggctjz)

> Xiong, X., Wu, G., Wei, Y., Liu, L., Zhang, Y., Su, R., Jiang, X., Li, M., Gao, H., Tian, X., et al. (2020). SspABCD–SspE is a phosphorothioation-sensing bacterial defence system with broad anti-phage activities. *Nat Microbiol*, **5**, 917–928. [10/ggq8nb](https://doi.org/10/ggq8nb)

> Wang, S., Wan, M., Huang, R., Zhang, Y., Xie, Y., Wei, Y., Ahmad, M., Wu, D., Hong, Y., Deng, Z., et al. (2021). SspABCD-SspFGH Constitutes a New Type of DNA Phosphorothioate-Based Bacterial Defense System. *MBio*, **12**, e00613-21. [10/gbsw](https://doi.org/10/gbsw)

The relevant refences for individual HMMs can be found by inspecting the `hmm_meta.txt` file provided with [PADLOC-DB](https://github.com/padlocbio/padloc-db).

## License

This software and data is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).



