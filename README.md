## Jupyter Binder tutorial

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Middle-Author-Bioinformatics/binder-sandbox/HEAD)


### Basic Commands
List files in the current directory:

    ls

Change directory:

    cd path/to/directory

View the first 10 lines of a file:

    head filename

View the last 10 lines of a file:

    tail filename

Find your current directory (present working directory):

    pwd

Copy a file:

    cp source_file destination_file

Move or rename a file:

    mv source_file destination_file

Delete a file:

    rm filename

### Genome Assembly Analysis Commands
Once you are familiar with the command-line basics, you can use the following commands to explore and analyze genome assembly files:


Count the number of contigs in a genome assembly FASTA file:

    grep -c "^>" assembly.fasta

Calculate the total number of bases in the assembly:

    grep -v "^>" assembly.fasta | wc -c


Extract contigs longer than 1000 bases:

    awk '/^>/ {if (seqlen > 1000) print seqname; seqlen=0; seqname=$0} !/^>/ {seqlen += length($0)} END {if (seqlen > 1000) print seqname}' assembly.fasta

Identify the longest contig in the file:

    awk '/^>/ {if (seqlen > maxlen) {maxlen=seqlen; longest=seqname}; seqlen=0; seqname=$0} !/^>/ {seqlen += length($0)} END {if (seqlen > maxlen) longest=seqname; print longest}' assembly.fasta

Generate a list of contig headers sorted by sequence length:

    awk '/^>/ {if (seqlen) print seqlen, seqname; seqlen=0; seqname=$0} !/^>/ {seqlen += length($0)} END {print seqlen, seqname}' assembly.fasta | sort -nr

Extract sequences for a specific contig by its header (Replace contig_name with the name of the desired contig.):

    awk '/^>contig_name/ {print; getline; while ($0 !~ /^>/) {print; getline}}' assembly.fasta

Rename contigs to a simpler format (e.g., contig_1, contig_2):

    awk '/^>/ {print ">contig_" ++i; next} {print}' assembly.fasta > renamed_assembly.fasta

Split each contig into a separate file:

    awk '/^>/ {filename = substr($0, 2) ".fasta"; print $0 > filename; next} {print >> filename}' assembly.fasta


Count occurrences of each nucleotide:

    grep -v "^>" assembly.fasta | fold -w1 | sort | uniq -c


### Working with .ffn (Nucleotide FASTA) Files
Count the Number of Gene Sequences

    grep -c "^>" genes.ffn

Extract Gene Sequences by Header (Replace gene_name with the header of the desired gene.)

    awk '/^>gene_name/ {print; getline; while ($0 !~ /^>/) {print; getline}}' genes.ffn

Filter Genes Longer than a Certain Length (Adjust 1000 to the desired length threshold.)

    awk '/^>/ {if (seqlen >= 1000) {print header; print sequence}; seqlen=0; header=$0; sequence=""} !/^>/ {seqlen+=length($0); sequence=sequence $0} END {if (seqlen >= 1000) {print header; print sequence}}' genes.ffn > long_genes.ffn

### Working with .gff3 (Gene Annotation) Files
Extract All Genes

    awk '$3 == "gene" {print $0}' annotations.gff3

Extract Genes with Specific Attributes (For example, genes on chromosome 1):

    awk '$1 == "chromosome_1" && $3 == "gene" {print $0}' annotations.gff3

Count the Number of Genes

    grep -c -p "\tgene" annotations.gff3

Extract Genes within a Specific Range (For example, genes between positions 1000 and 5000):

    awk '$3 == "gene" && $4 >= 1000 && $5 <= 5000 {print $0}' annotations.gff3

Convert GFF3 to BED Format

    awk '$3 == "gene" {print $1 "\t" $4-1 "\t" $5 "\t" $9}' annotations.gff3 > annotations.bed
