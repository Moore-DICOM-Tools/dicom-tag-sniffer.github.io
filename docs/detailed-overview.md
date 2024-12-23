# DICOM Tag Sniffer Overview

## Syntax for Invoking Tag Sniffer Jar File

Invoke the Tag Sniffer as follows:

```
java -jar $TAGSNIFFER_JAR -i $INPUT_FOLDER -m $MANIFEST $OUTPUT_FOLDER [$RULES_FILE]
```
* If specified with the -i flag, $INPUT_FOLDER provides the baseline input folder.
If the -i flag is not included, the Tag Sniffer scans from the folder where it was invoked.

* If specified with the -m flag, $MANIFEST points to a list of files to scan.
The path of each file in the list is relative to the baseline folder.

* $OUTPUT_FOLDER specifies the folder where output is written.
This will be a combination of folders and files.

* The $RULES_FILE is optional.
It provides the path to an XML file that adds rules at runtime that will reduce output.
The syntax of the rules and the software operations are discussed in the next section.

## Runtime Tag Sniffer Rules File
The rules file is an XML file with this structure:
```
<?xml version="1.0" encoding="UTF-8"?>
<scanDefinitions>
    <version date="20190512" release="0.1"/>
    <rules>
        <standardElements>
            <rule tag="00200012" pattern="Regular Expression" action="replace" replace="Replacement string"/>
            <rule vr="DA"        pattern="Regular Expression" action="replace" replace="Replacement string" />
        </standardElements>
        
    </rules>
</scanDefinitions>
```

The Tag Sniffer processes each DICOM element in the input file in order.
For each element, the software first attempts to apply a rule where a tag is specified.
If the tag in the rule matches the tag of the input element and the Regular Expression in the rule matches the element value,
the action of the rule is applied.
In the current software, the only supported action is "replace".
The software replaces the value of the element with the fixed value in the *replace* attribute.

If no tag rules are triggered, the software tries to apply vr (Value Representation) rules.
If the Value Representation in the rule matches that of the element and the Regular Expression in the rule matches the element value,
the action of the rule is applied.
As above, the only action that is supported is "replace".

After the runtime rules are applied, the software applie a set of [fixed rules](./fixed_rules.md).

Below is a set of example rules that we have applied to datasets.

* The rules for Acquisition Number replace 2 digit and 3 digit values with the fixed string: "Acquisition Number dd found" or "Acquisition Number ddd found"
* The rules for vr="DA" replace dates by decade with a fixed string
* The rules for vr="DT" replace Date/Time Strings by century with a fixed string

The rules for Acquisition Number are innocuous as Acquisition Number is not expected to contain PHI.

The rules for DA and DT were written for a case where we were using the data for internal purposes only.
There was no plan to publish the data, so there was no need to review and update Date and Date/Time elements.
We would eliminate these DA and DT rules or write new ones to review data destined for public access.

```
<standardElements>
  <!-- 0020, 0012 Acquisition Number -->
  <rule tag="00200012" pattern="^\d\d$"   action="replace" replace="Acquisition Number dd found"/>
  <rule tag="00200012" pattern="^\d\d\d$" action="replace" replace="Acquisition Number ddd found"/>
  
  <rule vr="DA"        pattern="^195\d[0-1]\d[0-3]\d$" action="replace" replace="Date 195xMMDD found" />
  <rule vr="DA"        pattern="^196\d[0-1]\d[0-3]\d$" action="replace" replace="Date 196xMMDD found" />
  <rule vr="DA"        pattern="^197\d[0-1]\d[0-3]\d$" action="replace" replace="Date 197xMMDD found" />
  <rule vr="DA"        pattern="^198\d[0-1]\d[0-3]\d$" action="replace" replace="Date 198xMMDD found" />
  <rule vr="DA"        pattern="^199\d[0-1]\d[0-3]\d$" action="replace" replace="Date 199xMMDD found" />
  <rule vr="DA"        pattern="^200\d[0-1]\d[0-3]\d$" action="replace" replace="Date 200xMMDD found" />
  <rule vr="DA"        pattern="^201\d[0-1]\d[0-3]\d$" action="replace" replace="Date 201xMMDD found" />
  <rule vr="DA"        pattern="^202\d[0-1]\d[0-3]\d$" action="replace" replace="Date 202xMMDD found" />
  <rule vr="DA"        pattern="^203\d[0-1]\d[0-3]\d$" action="replace" replace="Date 203xMMDD found" />
  <rule vr="DA"        pattern="^204\d[0-1]\d[0-3]\d$" action="replace" replace="Date 204xMMDD found" />
  
  <rule vr="DT"        pattern="^19\d\d[0-1][0-9][0-3][0-9]\d\d\d\d\d\d$" action="replace" replace="DateTime 19xxmmddssss found" />
  <rule vr="DT"        pattern="^19\d\d[0-1][0-9][0-3][0-9]\d\d\d\d\d\d\.\d*$" action="replace" replace="DateTime 19xxmmddssss found" />
  <rule vr="DT"        pattern="^20\d\d[0-1][0-9][0-3][0-9]\d\d\d\d\d\d$" action="replace" replace="DateTime 20xxmmddssss found" />
  <rule vr="DT"        pattern="^20\d\d[0-1][0-9][0-3][0-9]\d\d\d\d\d\d\.\d*$" action="replace" replace="DateTime 20xxmmddssss found" />
</standardElements>

```

# Implications for the XNAT Containerized version

A dockerized version exists that can be run by hand or using the XNAT Container Service.
When running in XNAT, the configuration for this command gives you the option of a small number of rules files.
These rules files are stored in the Docker image; you do not have the ability to substitute a different rules file on an ad hoc basis.
You can work with the developer of this container to add other rules files
or to design a model to allow the user to specify a custom rules file that is not part of the Docker image.
