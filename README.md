<a href="https://tiledb.com"><img src="https://github.com/TileDB-Inc/TileDB/raw/dev/doc/source/_static/tiledb-logo_color_no_margin_@4x.png" alt="TileDB logo" width="400"></a>

[![Build Status](https://dev.azure.com/TileDB-Inc/CI/_apis/build/status/TileDB-VCF?branchName=master)](https://dev.azure.com/TileDB-Inc/CI/_build/latest?definitionId=8&branchName=master)

# :dna: TileDB-VCF

A C++ library for efficient storage and retrieval of genomic variant-call data using [TileDB][].

## Features

- Easily ingest large amounts of variant-call data at scale
- Supports ingesting single sample VCF and BCF files
- New samples are added *incrementally*, avoiding computationally expensive merging operations
- Allows for highly compressed storage using TileDB sparse arrays
- Efficient, parallelized queries of variant data stored locally or remotely on S3
- Export lossless VCF/BCF files or extract specific slices of a dataset

## What's Included?

- Command line interface (CLI)
- APIs for C, C++, Python, and Java
- Integrates with Spark, Dask, and AWS Batch

Check out our [docs][vcf] for installation and usage instructions:

|           |       :package:       |       :memo:       |
|----------:|:---------------------:|:------------------:|
|       CLI | [Install][inst-cli]   | [Usage][use-cli]   |
|    Python | [Install][inst-py]    | [Usage][use-py]    |
|     Spark | [Install][inst-spark] | [Usage][use-spark] |
| AWS Batch | [Install][inst-aws]   | [Usage][use-aws]   |

## Quick Start

The docs linked above provide more comprehensive examples but here a few quick exercises to get you started.

By the way, we host a publicly accessible version of the `vcf-samples-20` array on S3. If you have TileDB-VCF installed and you'd like to follow along just swap out the `uri`'s below for `s3://tiledb-inc-demo-data/tiledb-arrays/2.0/vcf-samples-20`. And if you *don't* have TileDB-VCF installed yet, you can use our Docker images to test things out.

### CLI

Export complete BCF files for a subset of samples:

```sh
tiledbvcf export \
  --uri vcf-samples-20 \
  --sample-names v2-usVwJUmo,v2-WpXCYApL \
  --output-dir bcfs
```

Create a table of all variants within one or more regions of interest:

```sh
tiledbvcf export \
  --uri vcf-samples-20-files/vcf-samples-20 \
  --sample-names v2-tJjMfKyL,v2-eBAdKwID \
  -Ot --tsv-fields "CHR,POS,REF,S:GT" \
  --regions "chr7:144000320-144008793,chr11:56490349-56491395"
```


### Python

Run the same query in python

```py
import tiledbvcf

ds = tiledbvcf.TileDBVCFDataset(uri="vcf-samples-20", mode="r")

ds.read(
    attrs=["sample_name", "pos_start", "fmt_GT"],
    regions=["chr7:144000320-144008793", "chr11:56490349-56491395"],
    samples=["v2-tJjMfKyL", "v2-eBAdKwID"]
)
```

Results are returned as a pandas `DataFrame`

```
     sample_name  pos_start    fmt_GT
0    v2-nGEAqwFT  143999569  [-1, -1]
1    v2-tJjMfKyL  144000262  [-1, -1]
2    v2-tJjMfKyL  144000518  [-1, -1]
3    v2-nGEAqwFT  144000339  [-1, -1]
4    v2-nzLyDgYW  144000102  [-1, -1]
..           ...        ...       ...
566  v2-nGEAqwFT   56491395    [0, 0]
567  v2-ijrKdkKh   56491373    [0, 0]
568  v2-eBAdKwID   56491391    [0, 0]
569  v2-tJjMfKyL   56491392  [-1, -1]
570  v2-nzLyDgYW   56491365  [-1, -1]
```

* Motivation and use case: https://docs.tiledb.com/genomics/
* Installation: https://docs.tiledb.com/genomics/installation
* Usage: https://docs.tiledb.com/genomics/usage
* Further reading: https://docs.tiledb.com/genomics/advanced
* TileDB-VCF developer resources: https://github.com/TileDB-Inc/TileDB-VCF/wiki

# Code of Conduct

All participants in TileDB spaces are expected to adhere to high standards of
professionalism in all interactions. This repository is governed by the
specific standards and reporting procedures detailed in depth in the
[TileDB core repository Code Of Conduct](
https://github.com/TileDB-Inc/TileDB/blob/dev/CODE_OF_CONDUCT.md).

<!-- links -->
[tiledb]: https://tiledb.com
[vcf]: https://docs.tiledb.com/genomics/

[inst-cli]: https://docs.tiledb.com/genomics/installation/standalone-tiledb-vcf
[inst-py]: https://docs.tiledb.com/genomics/installation/python
[inst-spark]: https://docs.tiledb.com/genomics/installation/spark
[inst-aws]: https://docs.tiledb.com/genomics/installation/aws-batch

[use-cli]: https://docs.tiledb.com/genomics/usage/cli
[use-py]: https://docs.tiledb.com/genomics/usage/python
[use-spark]: https://docs.tiledb.com/genomics/usage/spark
[use-aws]: https://docs.tiledb.com/genomics/usage/aws-batch
