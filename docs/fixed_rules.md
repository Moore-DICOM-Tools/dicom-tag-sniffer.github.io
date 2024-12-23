# Fixed Rules

The Tag Sniffer software contains hard-coded rules that are designed to reduce the output for some DICOM elements.
These rules would be better specified in a file that is included at runtime; they remain hard-coded for now.
Some of the elements that are mentioned below contain time stamps but not dates.
This might not be the behavior you desire if you are remapping time stamps.

## Truncated Element Values
The baseline behavior of the Tag Sniffer software is to record the unique values associated with each DICOM element
and report those unique values in the output.
For the sets of DICOM elements described below, the software records the Tag Sniffer software records only the first 10 unique values and reports those.
Any other unique values are discarded.
This gives the viewer a chance to see a biased sample of the element values for elements that should be well understood.

### Truncated Acquisition Elements
These elements are included in the hard-coded rules that limit the number of values reported.
These are loosely categorized as acquisition elements.

| DICOM Tag | Element Name |
|-----------|--------------|
| 0020,0013 | Instance Number           |
| 0018,5100 | Patient Position          |
| 0020,0030 | Image Position            |
| 0020,0032 | Image Position Patient    |
| 0028,0200 | Image Location            |
| 0020,0035 | Image Orientation         |
| 0020,0037 | Image Orientation Patient |
| 0020,0050 | Location                  |
| 0020,1041 | Slice Location            |
| 0028,1050 | Window Center             |
| 0028,1051 | Window Width              |

### Truncated Pixel and Visualization Elements
These elements are related to pixel data and other elements that might be used for visualization or other processing.

| DICOM Tag | Element Name |
|-----------|--------------|
| 7fe0,0010 | Pixel Data                                  |
| 0028,0106 | Smallest Image Pixel Value                  |
| 0028,0107 | Largest Image Pixel Value                   |
| 0028,1101 | Red Palette Color Lookup Table Descriptor   |
| 0028,1201 | Red Palette Color Lookup Table Data         |
| 0028,1102 | Green Palette Color Lookup Table Descriptor |
| 0028,1202 | Green Palette Color Lookup Table Data       |
| 0028,1103 | Blue Palette Color Lookup Table Descriptor  |
| 0028,1203 | Blue Palette Color Lookup Table Data        |


### TruncatedTime Elements
These elements contain time of day values.

| DICOM Tag | Element Name |
|-----------|--------------|
| 0008,0030 | Study Time                        |
| 0008,0031 | Series Time                       |
| 0032,1041 | Study Arrival Time                |
| 0032,1051 | Study Completion Time             |
| 0008,0013 | Instance Creation Time            |
| 0070,0083 | Presentation Creation Time        |
| 0008,0032 | Acquisition Time                  |
| 0008,0033 | Content Time                      |
| 0018,1060 | Trigger Time                      |
| 0018,1201 | Time Of Last Calibration          |
| 0018,700E | Time Of Last Detector Calibration |

