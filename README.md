<a href="https://github.com/padlocbio/padloc/blob/master/LICENSE" alt="License"><img src="https://img.shields.io/github/license/padlocbio/padloc" /></a> <a href="https://anaconda.org/padlocbio/padloc" alt="Conda release"><img src="https://img.shields.io/conda/vn/padlocbio/padloc?color=yellow&label=conda%20release"></a> <a href="https://github.com/padlocbio/padloc/releases" alt="Github release"><img src="https://img.shields.io/github/v/release/padlocbio/padloc?label=github%20release" /></a> <a href="https://github.com/padlocbio/padloc/commits/master" alt="GitHub commits since latest release"><img src="https://img.shields.io/github/commits-since/padlocbio/padloc/latest?sort=semver"></a> <a href="https://github.com/padlocbio/padloc/" alt="Last update"><img src="https://img.shields.io/github/last-commit/padlocbio/padloc?label=last%20update" /></a> 


# PADLOC: Prokaryotic Antiviral Defence LOCator

## About

[PADLOC](https://github.com/padlocbio/padloc) is a software tool for identifying antiviral defence systems in prokaryotic genomes. [PADLOC](https://github.com/padlocbio/padloc) screens genomes against a database of HMMs and system classifications to find and annotate defence systems based on sequence homology and genetic architecture.

[PADLOC](https://github.com/padlocbio/padloc) can be installed and used via the command line or via our [web server](https://padloc.otago.ac.nz).

## Citation

If you use [PADLOC](https://github.com/padlocbio/padloc) or [PADLOC-DB](https://github.com/padlocbio/padloc-db) please cite:

> Payne, L.J., Todeschini, T.C., Wu, Y., Perry, B.J., Ronson, C.W., Fineran, P.C., Nobrega, F.L., Jackson, S.A. (2021) Identification and classification of antiviral defence systems in bacteria and archaea with PADLOC reveals new system types. *Nucleic Acids Res*, **X**, XXXX-XXXX. doi: [10/gzgh](https://doi.org/10/gzgh)

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
Output:
    --outdir [d]      Output directory
Optional:
    --data [d]        Data directory
    --cpu [n]         Use [n] CPUs (default '1')
    --raw-out         Include a summarised raw output file
```

## Output

| File           | Description                                         |
| -------------- | --------------------------------------------------- |
| *.domtblout    | Domain table file generated by HMMER.               |
| *_prodigal.faa | Amino acid FASTA file generated by prodigal.        |
| *_prodigal.gff | GFF annotation file generated by prodigal.          |
| *_padloc.csv   | PADLOC output file for identified defence systems.  |
| *_padloc.gff   | GFF annotation file for identified defence systems. |

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

  `ID=WP_000000001.1 ` *or* ``ID=cds-WP_000000001.1 ``

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

The relevant refences for individual HMMs can be found by inspecting the `hmm_meta.txt` file provided with [PADLOC-DB](https://github.com/padlocbio/padloc-db).

## License

This software and data is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).



