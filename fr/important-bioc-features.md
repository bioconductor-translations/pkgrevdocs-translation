# Important *Bioconductor* Package Development Features

## biocViews

Packages added to the *Bioconductor* Project require a `biocViews:` field in their DESCRIPTION file. The field name “biocViews” is case-sensitive and must begin with a lower-case ‘b’.

biocViews terms are “keywords” used to describe a given package. They are broadly divided into four categories, representing the type of packages present in the *Bioconductor* Project

1.  Software
2.  Annotation Data
3.  Experiment Data
4.  Workflow

biocViews are available for the \[release\]\[release-biocviews\] and [devel](#biocviews) branches of *Bioconductor*. The devel branch has a check box under the tree structure which, when checked, displays biocViews that are defined but not used by any package, in addition to biocViews that are in use. See also [description section](#description-biocviews)

### Motivation

One can use biocViews for two broad purposes.

1.  A researcher might want to identify all packages in the *Bioconductor* Project which are related to a specific purpose. For example, one may want to look for all packages related to “Copy Number Variants”.

2.  During development, a package contributor can “tag” their package with biocViews so that when someone looking for packages (like in scenario 1) can easily find their package.

### biocViews during new package development

Visit the [‘devel’ biocViews](#biocviews) when you are in the process of adding biocViews to your new package. Identify as many terms as appropriate from the hierarchy. Prefer ‘leaf’ terms at the end of the hierarchy, over more inclusive terms. Remember to check the box displaying all available terms.

Please Note:

1.  Your package will belong to only one part of *Bioconductor* Project (Software, Annotation Data, Experiment Data, Workflow), so choose only biocViews from that category.

2.  biocViews listed in your package must match exactly (e.g., spelling, capitalization) the terms in the biocViews hierarchy.

When you submit your new package for review , your package is checked and built by the *Bioconductor* Project. We check the following for biocViews:

1.  Package contributor has added biocViews.

2.  biocViews are valid.

3.  Package contributor has added biocViews from only one of the categories.

If you receive a “RECOMMENDED” direction for any of these biocViews after you have submitted your package, you can try correcting them on your own following the directions given here or ask your package reviewer for more information.

If a developer thinks a biocViews term should be added to the current acceptable list, please email <bioc-devel@r-project.org> requesting the new biocView, under which hierarchy the term should be placed, and the justification for the new term.

## Common Bioconductor Methods and Classes

We strongly recommend reusing existing methods for importing data, and reusing established classes for representing data. Here are some suggestions for importing different file types and commonly used *Bioconductor* classes. For more classes and functionality also try searching in [biocViews](#biocviews) for your data type.

### Importing

-   GTF, GFF, BED, BigWig, etc., – *[rtracklayer](https://bioconductor.org/packages/3.15/rtracklayer)* `::import()`
-   VCF – *[VariantAnnotation](https://bioconductor.org/packages/3.15/VariantAnnotation)* `::readVcf()`
-   SAM / BAM – *[Rsamtools](https://bioconductor.org/packages/3.15/Rsamtools)* `::scanBam()`, *[GenomicAlignments](https://bioconductor.org/packages/3.15/GenomicAlignments)* `::readGAlignment*()`
-   FASTA – *[Biostrings](https://bioconductor.org/packages/3.15/Biostrings)* `::readDNAStringSet()`
-   FASTQ – *[ShortRead](https://bioconductor.org/packages/3.15/ShortRead)* `::readFastq()`
-   MS data (XML-based and mgf formats) – *[Spectra](https://bioconductor.org/packages/3.15/Spectra)* `::Spectra()`, *[MSnbase](https://bioconductor.org/packages/3.15/MSnbase)* `::readMSData()`, *[Spectra](https://bioconductor.org/packages/3.15/Spectra)* `::Spectra(source = MsBackendMgf::MsBackendMgf())`, *[MSnbase](https://bioconductor.org/packages/3.15/MSnbase)* `::readMgfData()`

### Common Classes

-   Rectangular feature x sample data – *[SummarizedExperiment](https://bioconductor.org/packages/3.15/SummarizedExperiment)* `::SummarizedExperiment()` (RNAseq count matrix, microarray, …)
-   Genomic coordinates – *[GenomicRanges](https://bioconductor.org/packages/3.15/GenomicRanges)* `::GRanges()` (1-based, closed interval)
-   Genomic coordinates from multiple samples – *[GenomicRanges](https://bioconductor.org/packages/3.15/GenomicRanges)* `::GRangesList()`
-   Ragged genomic coordinates – *[RaggedExperiment](https://bioconductor.org/packages/3.15/RaggedExperiment)* `::RaggedExperiment()`
-   DNA / RNA / AA sequences – *[Biostrings](https://bioconductor.org/packages/3.15/Biostrings)* `::*StringSet()`
-   Gene sets – *[BiocSet](https://bioconductor.org/packages/3.15/BiocSet)* `::BiocSet()`, *[GSEABase](https://bioconductor.org/packages/3.15/GSEABase)* `::GeneSet()`, *[GSEABase](https://bioconductor.org/packages/3.15/GSEABase)* `::GeneSetCollection()`
-   Multi-omics data – *[MultiAssayExperiment](https://bioconductor.org/packages/3.15/MultiAssayExperiment)* `::MultiAssayExperiment()`
-   Single cell data – *[SingleCellExperiment](https://bioconductor.org/packages/3.15/SingleCellExperiment)* `::SingleCellExperiment()`
-   Mass spec data – *[Spectra](https://bioconductor.org/packages/3.15/Spectra)* `::Spectra()`, *[MSnbase](https://bioconductor.org/packages/3.15/MSnbase)* `::MSnExp()`

In general, a package will not be accepted if it does not show interoperability with the current \[*Bioconductor*\]\[\] ecosystem.

## Vignette

Every submitted Bioconductor package should have at least one Rmd (preferred) or Rnw vignette, ideally utilizing `BiocStyle::html_document` as output rendering. This should include evaluated R package code and a detailed introduction/abstract section that provides motivation for inclusion in Bioconductor and when appropriate a review and comparison to existing Bioconductor packages with similar functionality or scope. See [vignette documentation section](#vignettes) for more details.
