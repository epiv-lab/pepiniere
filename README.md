# Pépinière

Pépinière is an attempt to automate dataset clean up and initial tree building for influenza datasets. When provided with sequences downloaded from [GISAID](https://gisaid.org/) or [BVBRC](https://www.bv-brc.org/) alongside their metadata files, Pépinière will de-duplicate the dataset, filter through (and correct if needed) the metadata, trim ORFs, align them (using MAFFT or MUSCLE), build an initial tree (using FastTree) and use the metadata and TimeTree to obtain a time-scaled tree alongside some premade iTOL annotation files.

## Requirements

Pépinière is a Jupyter notebook that makes use of the following non-standard packages:
* [Biopython](https://github.com/biopython/biopython) is required as it is used to read through fasta files and for translating DNA sequences

Optional, if these packages are available Pépinière will use them to plot some statistics about your dataset:
* [Pandas](https://github.com/pandas-dev/pandas) to prepare dataframes that will be used by
* [plotly](https://github.com/plotly/plotly.py) to actually plot the data

The more fancy steps of aligning, tree building, and time-scaling make use of:
* Either [MAFFT](https://gitlab.com/sysimm/mafft) or [MUSCLE](https://github.com/rcedgar/muscle) for aligning. MUSCLE v5 is required as the super5 algorithm is used.
* [FastTree](http://www.microbesonline.org/fasttree/) to build the initial tree.
* [TreeTime](https://github.com/neherlab/treetime) to leverage the metadata and make a time-scaled tree.

All these packages or programs are available via conda for easy installation on Linux/MacOS.

## Input Data

Pépinière was written to work with data downloaded from GISAID or BVBRC and therefore data must follow either of these formats. For sequence data downloaded from GISAID, the following identifier format must be used:`Type|  DNA INSDC   | Isolate name   | DNA Accession no.`.  Date format should be YYYY-MM-DD and both space replacement/removal should be checked. 

Sequence data needs to be provided in fasta format and metadata in tab-separated text files. GISAID metadata is provided as Excel file and needs to first be converted to a tab-separated value text file. Both Numbers and Excel allow export as tab-separated values. Data and metadata from BVBRC can be used as is.

The dataset (both sequence data and metadata) can be split into any number of separate files that will then be concatenated by Pépinière during processing.

## Optional ORF filtering

Pépinière can be used to filter ORFs when assembling the final dataset, some of these filters are optional and can be toggled in the notebook.
* Filter out ORFs that do not end in a stop codon (default is True)
* Filter out ORFs without metadata or with unknown date of isolation (default is True)
* Filter out 100% identical ORFs as they are not informative for tree building (default is True)
* Fulter out ORFs with a high ratio of indeternimate positions (default is False)

## Reporting

By default Pépinière generates extensive report files detailing the steps taken to filter/de-duplicate sequences or correct metadata. It wil also generate log files containing the output of the alignment/tree building software. This allows you to retrace the steps taken and understand why your favourite sequence might not have made it through.
