# MutaCLASH

## Description
The **MutaCLASH** project is designed to detect the coordinates of Crosslink Induced Mutation Sites (CIMS) in NGS data. It provides a comprehensive analysis pipeline for identifying mutation sites and binding sites in hybrid-reads derived from CLASH or iCLIP experiments.

## Features
- Uses [CLASH Analyst](https://cosbi7.ee.ncku.edu.tw/CLASHanalyst/input/) to preprocess NGS data.
- Uses [HYB](https://github.com/gkudla/hyb), [CLAN](https://sourceforge.net/projects/clan-mapping) and [ChiRA](https://github.com/pavanvidem/chira) to identify suitable hybrid-reads.
- Detects mutation information using [Bowtie2](https://bowtie-bio.sourceforge.net/bowtie2/manual.shtml) and [BWA](https://bio-bwa.sourceforge.net).
- Utilizes algorithms such as [pirScan](http://cosbi4.ee.ncku.edu.tw/pirScan/), [miRanda](https://bioweb.pasteur.fr/packages/pack@miRanda@3.3a), and [RNAup](https://github.com/ViennaRNA/ViennaRNA) to identify binding sites.
- Generates visualizations of the distribution of mutations.

## Usage
To run the MutaCLASH pipeline, execute the following command:
```
sh run.sh <input file> <regulator file> <transcript file> <tool> <algorithm> <abundance analysis type>
```
- **input file:** NGS data in FASTQ format.
- **regulator file**: regulator file in FASTA format.
- **transcript file**: transcript file in FASTA format.
- **tool**: Tool used to detect hybrid reads, which can be `chira, hyb, clan`.
- **algorithm**: Algorithm used to predict binding sites, which can be `pirScan, miRanda, RNAup`.
- **abundance analysis type**: Method used to analyze abundance, which can be `abu, region, site, up` refers to "mRNA abundance" (check more details about this in `pipeline/add_abundance/abu_data/`), and 22G-RNA abundance (WAGO-1 IP) in "CLASH identified region", "pirScan binding site", "RNAup binding site". If this parameter is not specified, abundance analysis will not be executed.

After executing the command, the pipeline will run and complete all the necessary steps.

### Preprocessing Configuration

By default, the built-in [Trim Galore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/) tool within CLASH Analyst is utilized for quality trimming (Q=30) and adapter trimming (auto detect), with a default length range of 17-70nt. To modify the preprocessing settings, please adjust the configuration in `preprocess.conf` accordingly.

## Output
The output files are stored in the `data/output/` directory. The directory contains the following files:
- **CSV file**: Contains results with all information fields.
- **Figures:** The final generated figures are stored in the `figure/` subdirectory.
- **Logs**: Records commands, and summarizes the quantity, proportion, and distribution of various mutations, which are stored in the `log/` subdirectory.
- **Intermediate Files:** The intermediate files generated during the analysis are stored in various formats (.csv, etc.) and can be found in their respective tool directories.

### Figures
The output figures generated by the MutaCLASH pipeline include:

- **Score Distribution:** Presents the quantity and trend of different scores.
<img src="examples/fig/score.png" width=300 />

- **Mutation Distribution:** Provides information on the distribution of mutations, including deletions and substitutions.
<img src="examples/fig/distribution.png" width=300 />

- **Pairing Ratio:** Calculates and analyzes the pairing ratios at both global and individual coordinates.  
In statistical testing, ** and * indicate significant differences, with U-test P<0.05 and 0.10, respectively.
<img src="examples/fig/pairing_ratio_diff.png" width=600 />
<img src="examples/fig/pairing_ratio.png" width=600 />
<img src="examples/fig/pairing_ratio_at_position.png" width=300 />

- **Abundance Analysis:** Performs abundance analysis, comparing wild-type samples and fold-change measurements.
<!--img src="examples/fig/22G.png" width=300 /-->
<img src="examples/fig/fold_change.png" width=300 />
<img src="examples/fig/fold_change_per-score.png" width=500 />

- **Cumulative Distribution Function (CDF):** Calculates and visualizes the cumulative distribution function.
<!--img src="examples/fig/22G_CDF.png" width=300 /-->
<img src="examples/fig/fold_change_CDF.png" width=300 />

<!--Please refer to the corresponding tool documentation for more details on the specific output files and their interpretations.-->

## Requirements
Running MutaCLASH require Linux or MacOS. Other Unix environments will probably work but have not been tested. Windows users can use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

- Python >= 3.5
- numpy >= 1.12.1
- seaborn >= 0.9
- matplotlib >= 2.2.2
- pandas >= 0.23.0
- scipy >= 1.1.0
- statannot = 0.2.3
- cutadapt >= 2.10

## LICENSE
Please refer to our [MIT license](https://github.com/RyanCCJ/MutaCLASH/blob/master/LICENSE).
