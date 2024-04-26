## Jupyter Binder tutorial

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Arkadiy-Garber/binder-variant-calling/HEAD)

### Bash tricks

Print current working directory

    pwd

List the contents of the current working direcory
    
    ls

change directory to move into the **data** folder
    
    cd data/

Make a copy of example.txt file

    ls
    cp example.txt example_copy.txt
    ls

Make a copy somewhere else and relative paths

    ls data/
    cp example.txt ../example_copy.txt
    ls data/

Make a new directory and move the file into it

    mkdir new_dir
    mv example.txt new_dir/
    cd new_dir
    ls
    cd ../
    ls

Removing files and directories

    rm new_dir/example.txt
    rm -r new_dir

Terminal text editor

    nano example.txt

Redirects

    ls -lht
    ls -lht > file_contents.txt

Wildcard

    wc -l example.txt

Pipes

    ls | wc -l

The asterisk

    ls *txt

History

    history
    history | tail
    history | less
    history | grep 'nano'

The for-loop

    for i in *; do
        echo $i
    done

    for i in *; do echo $i; done

Iterating over letters and numbers:

    for i in {1..10}; do
        echo $i
    done

    for number in {1..10}; do
        echo sample-${number}.fa
    done

    for letter in {A..Z}; do
        echo sample-${letter}.fa
    done
    
Basics with cut, paste, and grep

    

### Breseq

unzip the gzipped FASTQ reads

    gzip -d reads.R1.fq.gz

    gzip -d reads.R1.fq.gz

print Breseq help menu

    breseq -h

Run basic Breseq analysis

    breseq -j 16 -r genome.gbk reads.R1.fq reads.R2.fq
