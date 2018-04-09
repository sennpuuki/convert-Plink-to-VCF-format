# convert-Plink-to-VCF-format

Source: [Dave Tang's blog](https://davetang.org/muse/2015/08/19/converting-ped-into-vcf/)

## Step 1 - Flip SNPs on reverse strand to forward strand
Tool: [Snpflip](https://github.com/biocore-ntnu/snpflip)

Illumina's exome SNP array (e.g. Illumina HumanOmniExpressExome v1.2 BeadChip) has a different strand strategy than regular forward-reverse strands of hg19. The "forward" strand from their supporting documents means the forward strand from dbSNP, and it is not the same thing as the forward strand on a reference genome (e.g. hg19).

So the first thing to do is to flip all SNPs on reverse strand to forward strand in terms of hg19.

[Snpflip](https://github.com/biocore-ntnu/snpflip) is used to do this.
The
