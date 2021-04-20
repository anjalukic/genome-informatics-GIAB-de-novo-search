# De novo variants in the reconstructed GIAB genome

## Summary
&nbsp;&nbsp;&nbsp;&nbsp;The goal of this project is to find de novo variants in genome of a child (son), given three (mother, father and son) GIAB sample files in FASTQ format. First, it was necessary to reconstruct the GIAB samples from FASTQ to VCF files which represent variations from reference genome. In second part we needed to write Python script to analyse three VCF files and to find all de novo variants in son. Thirdly, we used the RTG Tools VCFEval, tool that performs sophisticated comparison of VCF files, to find more accurate number of de novo variants in child. Finally, we compared results we got from our script to ones we got using tool made by Seven Bridges. We also used SBG VCF Benchmark for better visual representation of compared results.

### De novo
&nbsp;&nbsp;&nbsp;&nbsp;A genetic alteration that is present for the first time in one family member as a result of a variant (or mutation) on one of chromosomes of one of the parents is called de novo variant. It is also called new mutation or new variant.

### VCF files
&nbsp;&nbsp;&nbsp;&nbsp;The Variant Call Format (VCF) specifies the format of a text file used in bioinformatics for storing gene sequence variations. It contains the VCF header and the columns of a VCF. The header begins the file and provides metadata describing the body of the file. The body of VCF follows the header, and is tab separated into 8 mandatory columns and an unlimited number of optional columns that may be used to record other information about the sample(s).

![VCF file exapmle](images/vcf_file.png)

## VCF files
&nbsp;&nbsp;&nbsp;&nbsp;
