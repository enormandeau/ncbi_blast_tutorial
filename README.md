# NCBI blast tutorial

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

Decompress the archive. For example:

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
- `reference.fasta`

## Create blast database

The different blast tools require a formatted database to search against. In
order to create the database, we use the `makeblastdb` tool:

```
makeblastdb -in reference.fasta -title reference -dbtype nucl -out databases/reference
```

This will create a list of files in the `databases` folder. These are all part
of the blast database.

## Blast

We can now blast our sequences against the database. In this case, both our
query sequences and database sequences are DNA sequences, so we use the
`blastn` tool:

```
blastn -db databases/reference -query sequences.fasta -evalue 1e-3 -word_size 11 -outfmt 0 -max_target_seqs 1 > sequences.reference
```

## Blast with parallel

If you need to run your blasts faster (and who doesn't?), you can maximise your
blast speed using `gnu parallel`. You will find it [at this
link](http://ftp.gnu.org/gnu/parallel/parallel-latest.tar.bz2).

Download the archive, extract it (with `tar xvfB parallel-latest.tar.bz2`) and
install it with the following commands:

```
./configure
make
sudo make install
```

We can now use `parallel` to speed up blast:

```
time cat sequences.fasta | parallel -k --block 1k --recstart '>' --pipe 'blastn -db databases/reference -query - -evalue 1e-3 -word_size 11 -outfmt 0 -max_target_seqs 1' > sequences.reference
```

## More options and getting help

If you need help to know the options and parameters you can pass `blastn` and
the other blast+ utilities, use the `--help` option and pipe the output into
`less`, for example:

```
blastn --help | less
```

NCBI blast tools cover more cases than DNA against DNA searches. For example,
you can search a protein database with either DNA or protein sequences. Here is
an exhaustive list of the programs that come with the blast+ distribution:

```
blastdb_aliastool
blastdbcheck
blastdbcmd
blast_formatter
blastn
blastp
blastx
convert2blastmask
deltablast
dustmasker
legacy_blast.pl
makeblastdb
makembindex
makeprofiledb
psiblast
rpsblast
rpstblastn
segmasker
tblastn
tblastx
update_blastdb.pl
windowmasker
```

## References

> O. Tange (2011): GNU Parallel - The Command-Line Power Tool, ;login: The USENIX Magazine, February 2011:42-47.

## Licence

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Text" property="dct:title" rel="dct:type">NCBI blast tutorial</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/enormandeau/ncbi_blast_tutorial" property="cc:attributionName" rel="cc:attributionURL">Eric Normandeau</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.<br />Based on a work at <a xmlns:dct="http://purl.org/dc/terms/" href="https://github.com/enormandeau/ncbi_blast_tutorial" rel="dct:source">https://github.com/enormandeau/ncbi_blast_tutorial</a>.

