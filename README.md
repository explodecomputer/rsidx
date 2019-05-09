# rsidx

Daniel Standage, 2019  
https://github.com/bioforensics/rsidx

**rsidx** is a package for random access searches of VCF files by rsID.
This package enables rapid search of large VCF files by rsID in the same way that `tabix` enables rapid search by genomic coordinates.
In fact, the rsidx search uses `tabix` under the hood to search by genomic coordinates retrieved from the rsidx index.
This index is simply an sqlite3 database containing a mapping of rsID values to genomic coordinates.


## Installation

Installation with bioconda is pending.
For now, the following installation procedure is recommended.

```
pip install git+https://github.com/bioforensics/rsidx
```

This package requires that the program `tabix` be in your `$PATH`.


## Demo: command line interface

Invoke `rsidx index --help` and `rsidx search --help` for complete usage instructions.

``` bash
# VCF should be sorted by genomic coordinates and indexed by tabix
rsidx index dbSNP151_GRCh38.vcf.gz dbSNP151_GRCh38.rsidx
rsidx search dbSNP151_GRCh38.vcf.gz dbSNP151_GRCh38.rsidx rs3114908 rs10756819
```


## Demo: Python API

```python
import sqlite3
import rsidx

# Index the VCF file
with sqlite3.connect('myidx.db') as dbconn, open('myvar.vcf.gz', 'r') as vcffh:
    rsidx.index.index(dbconn, vcffh)

# Search the index
rsidlist = ['rs1260965680', 'rs1309677886', 'rs1174660622', 'rs1291927541']
with sqlite3.connect('myidx.db') as dbconn, open('myvar.vcf.gz', 'r') as vcffh:
    for line in rsidx.search.search(rsidlist, dbconn, vcffh)
        # process lines of VCF data
```