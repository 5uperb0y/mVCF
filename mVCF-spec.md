# The Multiverse Variant Call Format Specification
mVCFv0.1

2024-01-05

## The mVCF overview
mVCF stores significant deviations of events (i.e. "history variants") at specific moments across multiple timelines in comparison with a standard timeline of a universe. mVCF applies a similar concept of genome VCF to chronology,treating time as a linear sequence where events can vary across different timelines and universes.

mVCF is formatted in plain text for ease of use and accessibility. It consists of three primary components:

- Meta-information lines: Prefixed with `##`, these lines provide essential metadata about the mVCF file, including details about its format, source, and the reference timeline used for comparison.

- Header line: Prefixed with `#`, this line names the fixed, mandatory columns in the mVCF file. These columns represent key data points for each event, including its position on the timeline, the nature of the event, and its quality or impact.

- Entry lines: These lines contain the core data of the mVCF file. Each entry line details a specific event at a given point in time (position) across different universes or timelines. The format of these lines allows for a clear depiction of how events in various timelines deviate from the standard or reference timeline. The entry lines are tab-separated, ensuring organized and readable data representation.

In mVCF, zero-length fields are not permitted. Instead, a dot (`.`) is used to represent the absence of data or a null value. This approach ensures that every field in the mVCF file contains meaningful information or a clear indication of missing data.

## An example
```
##fileformat=mVCFv0.1
##fileDate=20240105
##source=BiblioMapperv1.0.7
##eventEncoding=CEES27
##reference=file:///chronology/100000HumanHistory-pilot.tl
##INFO=<ID=EF,Number=A,Type=Float,Description="Alternative Event Frequency">
##INFO=<ID=DP,Number=A,Type=Integer,Description="Read Depth, number of bibliography supporting alternate events">
##INFO=<ID=DESCRIB,Number=R,Type=String,Description"Event Description">
#COSMOS	POS	ID	REF	ALT	QUAL	FILTER	INFO	FORMAT	MITHC
U34	1933	.	P	ρ	.	.	EF=0.5;DP=1;DESCRIB="Mayor cermak is assassination","President Roosevelt is assassinated"	.	.
U34	1933	.	E	EEEEEEEE	.	.	EF=0.5;DP=1;DESCRIB="US Great Depression","US Great Depression"	.	.
U34	1945	.	W	WWW	.	.	EF=0.5;DP=1;DESCRIB="WWII","WWII"	.	.
U34	1945	.	P	.	.	.	EF=0.5;DP=1;DESCRIB="United Nations Founds"	.	.
```
This example shows (in order): at 1933, a simple SEV (single event variation) with the assassination of US President Roosevelt. As only the book "The Man in the High Castle" supports this observation, it has a read depth of 1 in the history mapping. As the death of Roosevelt, the Great Depression and WWII are longer, marking the CNV (copy number variation) at 1933 and 1945. In the alternative timeline, the United Nations do not found in 1945, representing a deletion in the event chain.

## Meta-information lines

File meta-information lines start with `##` and must appear first in the mVCF file, before the header line and entry lines. A meta-information line with a *key-value* pair (in `##key=value` format) are *structured*.

All structured lines require an ID which must be unique within their type. For all  of the structured lines, extra fields can be included after the default fields. For example.

```
##key=<key=value,key=value,key=value,...>
```

*Unstructured* lines usually contains obvious information such as `##fileformate` and `##fileDate`. Notably, the event encoding system (`##eventEncode`) is typically included and varied among mVCF for diffent domains.

## Header line syntax
The header line names the 8 fixed, mandatory columns. These columns are as follows:
```
#COSMOS	POS	ID	REF	ALT	QUAL	FILTER	INFO
```
If eventype data is present in the file, these are followed by a FORMAT column header, then an arbitrary number of entity IDs. Duplicate sample IDS are not allowed. The header line is tab-delimited and there must be no tab characters at the end of the line.


## Entry lines
Each entry line in an mVCF file represents specific event variations at a given moment in a timeline, detailing the differences between observed timelines and a standard reference. 

All entry lines are tab-delimited with no tab character at the end of the line. The last data line must end with a line separator. In all cases, missing values are specified with a dot (`.`).

- **COSMOS (universe)**: unique identifier for the specific universe in which the timeline is situated. Each universe is characterized by its distinct physical constants and principles.
- **POS (position)**: the reference time point, with the 1st time unit having position 1. Positions are sorted numerically, in increasing order, within each refernece timeline COSMOS. It is permitted to have multiple records with the same POS, enabling the representation of concurrent events.
- **ID (identifier)**: semicolon-separated list of unique identifiers for the event, ensuring unambiguous identification. Each identifier is exclusive to a single record within the mVCF file.
- **REF (reference event)**: the expected event at the given time point in the standard timeline, encoded using a predefined single-character system for brevity and standardization.
- **ALT (alternate event)**:  the event or state in the alternate universe or timeline that diverges from the standard reference.
- **QUAL (quality)**: quantiy score assessing the reliability of the alternate event assertion made in ALT.
- **FILTER (filter status)**:  filtering status of each event. "PASS" denotes that the event has met all filtering criteria, while a semicolon-separated list of failed filter codes is provided for events not passing all filters.
- **INFO (additional information)**: extended information in a key-value pair format, separated by semicolons. This field is adaptable for including diverse additional data relevant to the event, such as bibliographic references or descriptive annotations.