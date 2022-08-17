# UniqueKMER
Generate unique k-mers for every contig in a FASTA file.  

Unique k-mer is consisted of k-mer keys (i.e. ATCGATCCTTAAGG) that are only presented in one contig, but not presented in any other contigs (for both forward and reverse strands).  

This tool accepts the input of a FASTA file consisting of many contigs, and extract unique k-mers for each contig.

The output unique k-mer file and Genome file can be used for fastv: https://github.com/OpenGene/fastv, which is an ultra-fast tool to identify and visualize microbial sequences from sequencing data.

# what does UniqueKMER output?
This tool outputs a folder (folder name can be specified by `-o/--outdir`), which contains:
* a `index.html` file.
* a `kmercollection.fasta` file, which is a single file lists all the genome names along with their unique k-mer. Each k-mer key is represented by an individual line.
* a subfolder `genomes_kmers`, which contains a k-mer file and a Genome file for each contig, both in FASTA format.

You can open the `index.html` with any browser, then click on the contig names to find its k-mer file and Genome file.
* a small example: http://opengene.org/uniquekmer/test/index.html. This is generated by a small FASTA (http://opengene.org/test.fasta)
* a big example: http://opengene.org/uniquekmer/viral/index.html. This is generated by a big FASTA (http://opengene.org/viral.genomic.fasta) containing all NCBI complete RefSeq release of viral sequences, which can be found from https://ftp.ncbi.nlm.nih.gov/refseq/release/viral/

# get this tool
## download binary 
This binary is only for Linux systems: http://opengene.org/uniquekmer/uniquekmer
```shell
# this binary was compiled on CentOS, and tested on CentOS/Ubuntu
wget http://opengene.org/uniquekmer/uniquekmer
chmod a+x ./uniquekmer
```
## or compile from source
```shell
# step 1: get the code
git clone https://github.com/OpenGene/UniqueKMER.git

# step 2: build
cd UniqueKMER
make

# step 3: install it to system if you have a sudo permission
make install
```

# simple example:
```shell
uniquekmer -f test.fasta
```
You can get the test.fasta from: http://opengene.org/test.fasta

# more examples
### set the k-mer key length
```shell
# 16-mer (i.e. ATCGATCGATCGATCG...)
uniquekmer -f test.fasta -k 16
```
### filter the k-mer keys that can be mapped to a reference genome (i.e. human genome)
```shell
# k-mer sequences that can be mapped to hg38 with `edit distance <=2`  will be removed
uniquekmer -f test.fasta -r hg38.fasta -e 2
```
### set the spacing to avoid many continuous k-mer keys
```shell
# the spacing will be 2, which means if `key(pos)` is stored, then `key(pos+1)`  and `key(pos+2)` will be skipped
uniquekmer -f test.fasta -s 2
```

options:
```shel
  -f, --fasta            FASTA input file name (string)
  -o, --outdir           Directory for output. Default is unique_kmers in the current directory. (string [=unique_kmers])
  -k, --kmer             The length k of k-mer (3~32), default 25 (int [=25])
  -s, --spacing          If a key with POS is recorded, then skip [POS+1...POS+spacing] to avoid too compact result (0~100). default 0 means no skipping. (int [=0])
  -g, --genome_limit     Process up to genome_limit genomes in the FASTA input file. Default 0 means no limit. This option is for DEBUG. (int [=0])
  -r, --ref              Reference genome FASTA file name. Specify this only when you want to filter out the unique k-mer that can be mapped to reference genome. (string [=])
  -e, --edit_distance    k-mer mapped to reference genome with edit distance <= edit_distance will be removed (0~16). 3 for default. (int [=3])
  -?, --help             print this message
```

## get the pre-built k-mer file, genomes file or k-mer collection file for viruses
* You can download `k-mer` files and `genomes` files of viruses from http://opengene.org/uniquekmer/viral/index.html. This is generated by extracting unique k-mers for all genomes in a big FASTA (http://opengene.org/viral.genomic.fasta), which contains all NCBI complete RefSeq release of viral sequences that can be found from https://ftp.ncbi.nlm.nih.gov/refseq/release/viral/. The k-mers that can be mapped to human reference genome (GRCh38) with `edit_distance <= 3` have already been filtered out.
* You can download the `k-mer collection` file for viral genomes from: http://opengene.org/viral.kc.fasta.gz

## get the pre-built k-mer file, genomes file or k-mer collection file for viruses and human microorganisms
* You can download `k-mer` files and `genomes` files of viruses from http://opengene.org/uniquekmer/microbial/index.html. This is generated by extracting unique k-mers for all genomes in a big FASTA (http://opengene.org/microbial.genomic.fasta), which contains genomes for the viruses above and common human microorganisms. The k-mers that can be mapped to human reference genome (GRCh38) with `edit_distance <= 3` have already been filtered out.
* You can download the `k-mer collection` file for viral genomes from: http://opengene.org/microbial.kc.fasta.gz

# Citation
If you use `fastv`, `UniqueKMER` or the pre-generated resources provided by this repository, please cite our work as:

Shifu Chen, Changshou He, Yingqiang Li, Zhicheng Li, Charles E Melancon III. A Computational Toolset for Rapid Identification of SARS-CoV-2, other Viruses, and Microorganisms from Sequencing Data. bioRxiv 2020.05.12.092163; doi: https://doi.org/10.1101/2020.05.12.092163
