# DICOM Tag Sniffer Documentation

## Disclaimer
The overview below  and the documentation in [Detailed Overview](detailed-overview) are based on opinions gained from reviewing DICOM metadata and using software to de-identify and/or anonymize DICOM files.
We are describing a software tool that can be used as part of a process.
The software tool is not guaranteed to make sure that you identify all Protected Health Information found in your DICOM files.
The assumptions that drive the design and use of the tool are the author's opinions and have not been published for peer review.

## Overview
### Audiences
There are at least two categories of individuals who might use the Tag Sniffer tool.
There is overlap in the duties of the individuals.

| General Title        | Responsibilities |
|----------------------|------------------|
| Anonymization Author | The Anonymization Author is responsible for configuring/running any software that is used to remove PHI from DICOM files. This person might be in the organization that owns the original, or this person might be in a separate organization that runs a site that deidentifies the DICOM files as part of accepting the data during an upload process. We make no assumptions about the tools used or the process taken to run that software. |
| Data Analyst         | The Data Analyst reviews metadata extracted from DICOM files as part of a manual quality control process. This assumes that DICOM metadata is reviewed by a human. |


### Theory
Different types of risks are faced when performing human review of DICOM metadata for both single and multiple imaging studies.
These risks can be reduced by formatted reports that include subsets of the data for review.
Specifically, the tool only reports unique data values and also reduces the number of values reported for some elements.
More detail is found in [Detailed Overview](detailed-overview) including the different ways that metadata values are processed and presented to reduce fatigue during human review.

See [DICOM Tag Sniffer Overview](./detailed-overview.md) for more details, including instructions on running the application.
|------------------|-------------|

## Assumptions / Gaps

1. Some software will create file names that include a DICOM UID. We have seen instances where that UID includes the date of the imaging session.
This software ignores folder and file names.
2. We observed data in private elements from Siemens that appear to include text encoded as 16 bit characters.
The software and some of our post processing steps assume 8 bit characters.

## Status
