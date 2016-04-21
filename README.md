# ncbi_blast_tutorial

Short introduction to using NCBI blast tools from the command line

## Using Blast from the command line

Sometimes, you may have to use blast on your own computer to query thousands of
sequences against a custom database of hundreds of thousands of sequences. To
do that, you will need to install Blast on your computer, format the database
and then blast the sequences.

Here is a short tutorial on how to do this.

## Installing Blast+ tools

Get the compiled executables from this URL:

```
ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/
```

Decompress the archive:

```
tar xvfz ncbi-blast-2.3.0+-x64-linux.tar.gz
```

Add the `bin` folder from the extracted archive to your path. For example, add
the following line to your `~/.bashrc` file:

```
export PATH="/PATH/TO/ncbi-blast-2.3.0+/bin":$PATH
```

And change the `/PATH/TO` part to the path where you have put the extracted
archive.

## Example sequences to use with the tutorial

In order to test blast, you need a test fasta file. Use the following files
that come with the tutorial:

- `sequences.fasta`
- `reference.fasta

## Create blast database

```
makeblastdb -in reference.fasta -title reference -dbtype nucl -out reference
```

## Blast

blastn -db reference -query sequences.fasta -evalue 1e-3 -word_size 11 -outfmt 6 -max_target_seqs 1 > sequences.reference

## Blast with parallel

time cat sequences.fasta | parallel -k --block 1k --recstart '>' --pipe 'blastn -db reference -query - -evalue 1e-3 -word_size 11 -outfmt 6 -max_target_seqs 1' > sequences.reference

