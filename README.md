# convert-Plink-to-VCF-format

Input files:
`input.bim`

`input.bed`

`input.fam`

## Step 1 - Flip SNPs on reverse strand to forward strand
Tool: [Snpflip](https://github.com/biocore-ntnu/snpflip)

Illumina's exome SNP array (e.g. Illumina HumanOmniExpressExome v1.2 BeadChip) has a different strand strategy than regular forward-reverse strands of hg19. The "forward" strand from their supporting documents means the forward strand from dbSNP, and it is not the same thing as the forward strand on a reference genome (e.g. hg19).

So the first thing to do is to flip all SNPs on reverse strand to forward strand in terms of hg19.

[Snpflip](https://github.com/biocore-ntnu/snpflip) is used to do this.
It outputs three files:
`input.reverse` 

`input.ambiguous` 

`input.annotated_bim` 


The `input.ambiguous` file contains the SNPs that cannot be assigned to strand. So these SNPs are removed for further analysis (in my case, there are about 4.5% ambiguous SNPs).

Then use plink's `--flip` to flip all SNPs in `input.reverse` to forward strand.

Use: `snpflip --bfile input --flip input.reverse --make-bed --out input_rmreverse`
Output: `input_rmreverse.bim``input_rmreverse.bed``input_rmreverse.fam`

## Step 2 - PLINK v1.9 --recode vcf
Tool: [PLINK v1.9](https://www.cog-genomics.org/plink2)

PLINK does have the recode function to convert PLINK files into VCF. But the resulting VCF file also states that
```
##INFO=<ID=PR,Number=0,Type=Flag,Description="Provisional reference allele, may not be based on real reference genome">
```
which means that the allele at REF column in the resulting VCF is not necessarily REF.

We PLINK v1.9 to do the conversion first and then fix the REF issue in Step 3.

Use: `plink --bfile input_rmreverse --recode vcf --out input_rmreverse`
Output: `input_rmreverse.vcf`

## Step 3 - fix REF
Tool: [Dave Tang's blog](https://davetang.org/muse/2015/08/19/converting-ped-into-vcf/)

For people who do not have PLINKSEQ and older PLINK, we can borrow the last part of the python script from [Dave Tang's blog](https://davetang.org/muse/2015/08/19/converting-ped-into-vcf/).
Note that python package [bx](https://github.com/bxlab/bx-python) is required for this script to run. Also, reference genome is required.

Use: `plink_to_vcf-partial.py input_rmreverse.vcf hg19.2bit`
Output: `input_rmreverse-fix.vcf`




