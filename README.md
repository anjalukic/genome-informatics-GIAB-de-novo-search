# De novo variants in the reconstructed GIAB genome

## Summary
&nbsp;&nbsp;&nbsp;&nbsp;The goal of this project is to find de novo variants in the genome of a child (son), given three (mother, father and son) GIAB sample files in FASTQ format. First, it was necessary to reconstruct the GIAB samples from FASTQ to BAM and get VCF files which represent variants from reference genome. Secondly, we needed to write a Python script to analyse the three VCF files and to find all de novo variants in the son. Thirdly, we used RTG Tools VCFEval, a tool that performs sophisticated comparison of VCF files, to find the number of de novo variants in the child more accurately. Finally, we compared the results we got from our script to the ones we got using the tool made by Seven Bridges. We also used SBG VCF Benchmark for better visual representation of compared results.

---
### De novo
&nbsp;&nbsp;&nbsp;&nbsp;A genetic alteration that is present for the first time in one family member as a result of a variant (or mutation) on one of chromosomes of one of the parents is called de novo variant. It is also called new mutation or new variant.

---
### VCF files
&nbsp;&nbsp;&nbsp;&nbsp;The Variant Call Format (VCF) specifies the format of a text file used in bioinformatics for storing gene sequence variations. It contains the VCF header and the columns of a VCF. The header begins the file and provides metadata describing the body of the file. The body of VCF follows the header, and is tab separated into 8 mandatory columns and an unlimited number of optional columns that may be used to record other information about the sample(s).

![VCF file exapmle](images/vcf_file.png)

*VCF file example*  

---
### Environment
&nbsp;&nbsp;&nbsp;&nbsp;This project was executed on two platforms. Every tool execution was done on Seven Bridges platform while all Python scripts were done locally on Ubuntu 20.04 OS in Jupyter Notebook. 

## I Reconstruction of GIAB samples
&nbsp;&nbsp;&nbsp;&nbsp;GRAF Germline Variant Detection Workflow tool was used to reconstuct whole genome from initial FASTQ files given the reference genome GRCh38. Output of the previously mentioned tool is a BAM file, which represents the whole genome that is reconstructed and aligned, and a VCF file, which represents all variations each sample had relative to reference genome GRCh38. In the continuation of the project, we will use the three VCF files we recieved from this tool.

## II Creating tool for finding de novo child variants 
&nbsp;&nbsp;&nbsp;&nbsp;The code is organized into three parts. The first part will make an intersection of one parent and a child, which results in all the mutations that a child could have gotten from that parent. Then, an union of these intersections was performed in order to get all the variations child could have gotten from the parents by inheritance. In the end, the disjunction of the child and the obtained union was made, which will give all the variations that the child has that weren't inherited. 

![Code structure](images/code_structure.png)

*Code structure*  

---
### Somatic chromosomes
&nbsp;&nbsp;&nbsp;&nbsp; Talk about inheritance and all combinations

### X and Y chromosomes
&nbsp;&nbsp;&nbsp;&nbsp; Talk about inheritance and all combinations and all problems and errors with XY chromosomes


## III Finding de novo variants using RTG Tools VCFEval 
&nbsp;&nbsp;&nbsp;&nbsp;We used RTG Tools VCFEval to find the number of de novo variants in the child genome more accurately. This was done by creating a Workflow application on Seven Bridges platform. The application first used Tabix BGZIP and Index tools to convert VCF files to the format required by VCFEval tool. The first comparison was done on the mother and the son, and second on the father and the son (parents as baseline, child as calls) and false-positive output was taken from both comparisons. False-positive VCF contains called variants that were not in the baseline VCF, which in this case means child's variants that were not in the parent. Two false-positive VCF files were forwarded to the third comparison where true-positive output was taken. True positive VCF represents variants that exist in both input VCF files, in other words, a variation of the child that is not found in either parent or de novo. 

![Code structure](images/rtg_tools_vcfeval.png)

*RTG Tools VCFEval application structure*  

---

&nbsp;&nbsp;&nbsp;&nbsp;As it can be seen from the picture above, we stored true-positive VCF files after the first and second comparison. True-positive VCF is equivalent of the intersection done in this project. This was done so that we can not only compare end results (the number of de novo variants), but also so that we can compare how each component (intersection and union/disjunction) performs against professional sophisticated comparison done by RTG Tools VCFEval.

## IV Results and future improvements
&nbsp;&nbsp;&nbsp;&nbsp;Results obtained in this project can be seen in the table below. The column 'Partial' represents the results we got when we used intersection created by RTG Tools VCFEval, while union and disjunction were executed by scripts from this project. In other words, how accurate VCFEval tool is on separate components of our project.

&nbsp; | This project | Partial | RTG Tools VCFEval
------------- | ------------- | ------------ | -------------
Number of de novo variants detected | **184,689** | 131,570 | **85,381**
Percentage [%] | **3.765** | 2.682 | **1.740** 

&nbsp;&nbsp;&nbsp;&nbsp;As it can be seen from the table above, the number of de novo variants this project found is much greater than it should be. This is happening because there are some situations our script does not detect, resulting in a lot of false positive de novo variants. Some situations that our script will not detect and can be implemented in the future:
  
  * The GRAF tool divides one longer variation into several smaller ones. Our script will report all of the smaller ones as de novo variants instead of detecting longer one, if the same variants do not exist in either parents.
  * If both parents have genotype 1/2 where the child inherited one of both and the sequencer shortened REF and ALT of that variant. The script will detect de novo variant if ref and alt are different, eg. position 3719890, chr1 | Father: TTG-> T, TTGTG | Mother: TTG-> T, TTGTGTG | Child inherited the other two D: T-> TTG, TTGTG. It will be detected as de novo variant even though it isn't.
  * The mutation occurred on one chromosome of the child. The GRAF tool then, instead of changing only one ALT of the child, shortens them both and our script reports de novo variant on both chromosomes instead of on only one. Eg. position 775840, chr1 | both parents: C-> CA 1/1 | Child: CA-> C, CAA where de novo is only CA-> C and not both.

---
&nbsp;&nbsp;&nbsp;&nbsp;More detailed comparison of the results was done by SBG VCF Benchmark tool and Venn diagram showing number of true positives, false positives and false negatives between our project and RTG Tools VCFEval results can be seen below.

![Venn diagram1](images/venn_diagram1.png)

*Venn diagram comparing results*  

