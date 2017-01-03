# ploidyNGS: Visually exploring ploidy with Next Generation Sequencing data

**Motivation:** Due to the decreasing costs of genome sequencing researchers are increasingly assessing the genome information of non-model organisms using Next Generation Sequencing technologies. An important question to answer when assessing a new genome sequence is: What is the ploidy level of the organism under study?

**Results:** We have developed `ploidyNGS`, a model-free, open source tool to visualize and explore ploidy levels in a newly sequenced genome, exploiting short read data. We tested `ploidyNGS` using both simulated and real NGS data of the model yeast *Saccharomyces cerevisiae*. `ploidyNGS` allows the identification of the ploidy level of a newly sequenced genome in a visual way. We have applied `ploidyNGS` to a wide range of different genome ploidy and heterozigosity levels, as well as to a range of sequencing depths.

**Availability and implementation:** `ploidyNGS` is available under the GNU General Public License (GPL) at https://github.com/diriano/ploidyNGS. ploidyNGS is implemented in Python and R.

## Requirements

- python >=2.7.8
- pysam  >=0.9
- biopython >=1.66
- R and ggplot2

## Installation

### Get ploidyNGS from github

```bash
cd ~
git clone https://github.com/diriano/ploidyNGS.git
```
### Python virtual environment and python dependencies

Make sure pip is installed. If not, then please check your distribution's documentation and install it. In Ubuntu 16.04 LTS you can do:

```bash
sudo apt install python-pip
```

Install the virtualenv module:

```bash
pip install --upgrade pip
pip install virtualenv
```

Go to the ploidyNGS folder, create and active a new virtual environment

```bash
cd ploidyNGS
virtualenv .venv
source .venv/bin/activate
```

Install dependencies:

pysam requires the zlib headers. Please make sure you have these installed. In Ubuntu 16.04 LTS you can do:

```bash
sudo apt install zlib1g-dev
```

Then install pysam:

```bash
pip install pysam
```

Install biopython dependencies, and then biopython itself (for details see: http://biopython.org/DIST/docs/install/Installation.html):

```bash
pip install numpy
pip install biopython
```

At this point you can deactive the python virtual environment:

```bash
deactivate
```
### R

In Ubuntu 16.04 LTS:

```bash
  sudo apt install r-base
  sudo apt install r-cran-ggplot2
```

### Test ploidyNGS

In order to use `ploidyNGS` please start by activating the python virtual environment that you created before:

```bash
cd ~
cd ploidyNGS
source .venv/bin/activate
```

And then run:

```bash
./ploidyNGS.py -o diploidTest -b test_data/simulatedDiploidGenome/Ploidy2.bowtie2.sorted.bam
```

This should print the following in your screen:

```bash
No index available for pileup. Creating an index...
Getting the number of mapped reads from BAM
460400
```

And generate the files:

* diploidTest_depth100.tab
* diploidTest_depth100.tab.PloidyNGS.pdf

The PDF file should have a histogram identical to this https://github.com/diriano/ploidyNGS/tree/master/images/diploidTest_depth100.tab.PloidyNGS.png

After running ploidyNGS, do not forget to deactivate your python virtual environment:

```bash
deactivate
```
## ploidyNGS usage and examples

Active the python virtual environment, created above, before using `ploidyNGS`:

````bash
cd ~/ploidyNGS/
source .venv/bin/activate
```

The switch `-h` will give you access to the full help page:

```bash
./ploidyNGS.py -h
usage: ploidyNGS.py [-h] [-v] -o file -b mappingGenome.bam [-m 0.95 default)]
                    [-d 100 (default]

ploidyNGS: Visual exploration of ploidy levels

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program's version number and exit
  -o file, --out file   Base name for a TAB file that will keep the allele
                        counts
  -b mappingGenome.bam, --bam mappingGenome.bam
                        BAM file used to get allele frequencies
  -m 0.95 (default), --max_allele_freq 0.95 (default)
                        Fraction of the maximum allele frequency (float betwen
                        0 and 1, default: 0.95)
  -d 100 (default), --max_depth 100 (default)
                        Max number of reads kepth at each position in the
                        reference genome (integer, default: 100)
```

There are two required parameters: the input BAM file (`-b` or `--bam`) and a string (`-o` or `--out`) that will be used to generate the output files.

Two additional parameters help you control some of the behavior of `ploidyNGS`:

* `-m` or `--max_allele_freq`: Float between 0 and 1. Default 0.95. Ignore positions where the frequencey of the most abundant allele is higher or equal than `max_allele_freq`
* `-d` or `--max_depth`: Integer. Default 100. Maximum sequencing depth to consider, e.g., if d=100, then only the first 100 mapped reads will be examined.

## Examples and test
-------------------

In the folder test_data you will find the follosing sub-folder, containig datasets with different ploidy levels:

* HaploidGenome
* simulatedDiploidGenome
* simulatedTetraploidGenome
* simulatedTriploidGenome

### Interpretation of results of ploidyNGS with different ploidy levels

Additionally you will also find the folder ploidyNGS_results that contains the graphs generated by `ploidyNGS` foreach of these datasets.
The resulting PDF (PNG versions are also provided) files for each ploidyNGS run are:

- `DataTestPloidy1.tab.ExplorePloidy.pdf` - haploid organism
- `DataTestPloidy2.tab.ExplorePloidy.pdf` - diploid
- `DataTestPloidy3.tab.ExplorePloidy.pdf` - triploid
- `DataTestPloidy4.tab.ExplorePloidy.pdf` - tetraploid

For the interpretation of the resulting plots you can either use our simulation data (https://github.com/diriano/ploidyNGS/tree/master/simulation/results) or the following table. Briefly, in `ploidyNGS`'s graphs, the X-axis position of the peaks is directly related to the ploidy level of the organism under study. For instance, if you observe peaks for the two most frequent alleles close to 50%, you have a diploid organism (see `DataTestPloidy2.tab.ExplorePloidy.pdf`). The percentages mean that for heteromorphic positions, each allele was covered by approx. that percent of total reads in the positions.


| Ploidy level | Second most frequent allele (expected proportion) | Most frequence allele (expected proportion) |
| ------------ |-------------------------------:|-------------------------------:|
| Diploidy | 50 | 50 |
| Triploidy | 33.33 | 66.67 |
| Tetraploidy | 25 | 75 |
| Tetraploidy | 50 | 50 |
| Pentaploidy | 20 | 80 |
| Pentaploidy | 40 | 60 |
| Hexaploidy | 16.67 | 83.33 |
| Hexaploidy | 50 | 50 |
| Hexaploidy | 33.33 | 66.67 |
| Heptaploidy | 28.57 | 71.43 |
| Heptaploidy | 42.86 | 57.14 |
| Heptaploidy | 14.29 | 85.71 |

## Full analysis example - diploid organism

Here is a complete example of a pipeline for ploidy analysis using `ploidyNGS`.
For this example, we use a simulated diploid yeast chromosome generated using our script `simulatePloidyData.py`, which results in the plot above, `test_data/ploidyNGS_results/DataTestPloidy2.tab.ExplorePloidy.pdf`.

* NOTE: For real data, the user will have only reads from sequencing, not from simulated genome sequence.
* For real data, the BAM used in ploidyNGS should be created from mapping reads to the assembled genome sequence, or to a reference sequence (e.g. a closely related strain).

a) Generation of the simulated diploid organism sequences.

We used the *Saccharomyces cerevisiae* S288c chromosome I sequence (haploid) to generate the diploid one, as shown here:

```
$ python3 simulatePloidyData.py --genome GCA_000146045.2_R64_genomic_chromosomeI.fna --heterozygosity 0.01 --ploidy 2
```

Here, `simulatePloidyData.py` takes the input chromosome (`GCA_000146045.2_R64_genomic_chromosomeI.fna`) and creates two chromosomes (ploidy 2) with heteromorphic positions very around 100 bases (for the heterozigosity rate set, 0.01). In this case, the heteromorphic positions have the expected proportion for a diploid (50% of each allele).

b) Generation of the simulated reads for the diploid organism.

For the generation of simulated HiSeq 2500 Illumina paired-end reads, we used the command below in ART (Huang, Weichun, *et al.* 2012):

```
$ art_illumina -na -i simulatedChroms_Ploidy2_Heter0.01.fasta -p -l 100 -ss HS25 -f 100 -m 200 -s 10 -o Ploidy2_100x
```

The paired-end reads are generated with 100 bases and an average fragment size of 200.

The `.fastq` files are in the directory `test_data/simulatedDiploidGenome` :

The sofware outputs the reads in two files:
- Ploidy2_100x1.fq.gz
- Ploidy2_100x2.fq.gz

These reads are used in mapping step.

c) Mapping step using `Bowtie2`.

A BAM with reads mapping the genomic sequence is required in ploidyNGS main script.
It can be generated with Bowtie2 or other mapping algorithm.

Here, we used the haploid *S. cerevisiae* chromosome I to map our reads from the diploid. Here is how `Bowtie2` (version 2.2.3) is run:

```
$ bowtie2 --met-file align_metrics.txt -t --very-sensitive -q -x GCA_000146045.2_R64_genomic_chromosomeI.fna -1 Ploidy2_100x1.fq -2 Ploidy2_100x2.fq | samtools view -bS - | samtools sort - Ploidy2.bowtie2.sorted
```

`samtools` (Li *et al.* 2009) is also used after the pipe in order to generate the BAM file and then sort it (by genome coordinates).

If you are working on a multi-threaded cluster, `Bowtie2` (and other mappers) also allows to use multiple threads, speeding up the process.

The resulting BAM file is then used in ploidyNGS script (next step).

d) Running ploidyNGS main script (`ploidyNGS.py`)

`ploidyNGS.py` only requires a BAM and a name for the output file. Here is how to use it:

```
$ ploidyNGS.py --out DataTestPloidy2.tab --bam Ploidy2.bowtie2.sorted.bam 
```

The script outputs two files:
- `DataTestPloidy4.tab.ExplorePloidy.pdf`, the plot with the frequencies of allele proportions in heterozygous sites.
- `DataTestPloidy2.tab.Rscript`, the R script used to generate the plot above. 

# RECOMMENDATIONS

`ploidyNGS` was developed to assess ploidy level in small genomes, of a few tens Mbp. If your genome is larger than that we recommend that you:

a) look at individual chromosomes. For this you can use bamtools, check: https://www.biostars.org/p/46327/.

b) look at a region of a single chromosome.

The running time and memory usage of `ploidyNGS` depends on the number of reads in the BAM file, i.e., the sequencing depth. We have seen that a sequencing depth of 100x is enough to get a good idea of the ploidy level. So if you have sequenced your genome to a larger depth, please sub-sample you data before creating the BAM file for `ploidyNGS`. Alternatively you could use the parameter `-d` in `ploidyNGS` to look at the `d` first mapped reads, however pysam will still load the full BAM file.

# REFERENCES

Huang, Weichun, et al. "ART: a next-generation sequencing read simulator." *Bioinformatics* 28.4 (2012): 593-594.

Li, Heng, et al. "The sequence alignment/map format and SAMtools." *Bioinformatics* 25.16 (2009): 2078-2079.
