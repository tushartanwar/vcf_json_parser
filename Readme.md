# Find Variant VCF-JSON Parser

## Problem Statement

To parse a variant call format file ([https://en.wikipedia.org/wiki/Variant_Call_Format)](https://en.wikipedia.org/wiki/Variant_Call_Format)). The file has one thousand variants in it. Each line after the header lines is a variant. Open the variant file and inspect its contents. Parse each line and transform it into a JSON. The output will look something like this _(I have shortened the output)_:

```json
{
        "ALT": "G",
        "CHROM": "4",
        "FILTER": "PASS",
        "ID": ".",
        "INFO": {

            "Gene.ensGene": "ENSG00000109471,ENSG00000138684",
            "Gene.refGene": "IL2,IL21",
            "GeneDetail.ensGene": "dist=38306,dist=117597",
            "GeneDetail.refGene": "dist=38536,dist=117597"
        },
        "POS": 123416186,
        "QUAL" :23.25,
        "REF": "A",
        "SAMPLE": {
            "XG102": {
                "AD": "51,8",
                "DP": "59",
                "GQ": "32",
                "GT": "0/1",
                "PL": "32,0,1388"
            }
    }
```

The script is written in such a way that it will guide you through writing small functions that will be used to generate the JSON above. Note that the method outlined in the script is not necessarily optimal. It is just one way of parsing the file and how to break a large problem into small parts.

Note that the header which corresponds to the columns starts with one hash/pound `(#)` symbol. Skip all headers that start with two hashes/pounds `(##)`.

## How script works?

The script has 12 steps with each step building a small function which will be used in the final step to generate the final output. 

## Input

A file containing 1000 variants. (`lab1_data.vcf`)

## Output

- A json file (`lab1.json`) containing the parsed variants into json. 

- A function which can take `CHROM`,`REF`,`ALT` ,`POS`  and `output_filename` _(containing all variants as json)_ as input and return a list of variants that match the given `CHROM`,`REF`,`ALT` and `POS`.

## Understanding VCF header

| NAME    | BRIEF DESCRIPTION                                                                                                                                                                                               |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CHROM   | The name of the sequence (typically a chromosome) on which the variation is being called. This sequence is usually known as 'the reference sequence', i.e. the sequence against which the given sample varies.  |
| POS     | The 1-based position of the variation on the given sequence.                                                                                                                                                    |
| ID      | The identifier of the variation, e.g. a dbSNP rs identifier, or if unknown a ".". Multiple identifiers should be separated by semi-colons without white-space.                                                  |
| REF     | The reference base (or bases in the case of an indel) at the given position on the given reference sequence.                                                                                                    |
| ALT     | The list of alternative alleles at this position.                                                                                                                                                               |
| QUAL    | A quality score associated with the inference of the given alleles.                                                                                                                                             |
| FILTER  | A flag indicating which of a given set of filters the variation has passed.                                                                                                                                     |
| INFO    | An extensible list of key-value pairs (fields) describing the variation. See below for some common fields. Multiple fields are separated by semicolons with optional values in the format: <key>=<data>[,data]. |
| FORMAT  | An (optional) extensible list of fields for describing the samples. See below for some common fields.                                                                                                           |
| SAMPLEs | For each (optional) sample described in the file, values are given for the fields listed in FORMAT.                                                                                                             |




