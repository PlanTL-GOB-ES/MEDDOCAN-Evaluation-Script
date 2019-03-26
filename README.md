# MEDDOCAN: Evaluation Script

## Digital Object Identifier (DOI)


## Introduction
------------

This script is distributed as apart of the Medical Document Anonymization
(MEDDOCAN) task. It is based on the evaluation script from the i2b2 2014
Cardiac Risk and Personal Health-care Information (PHI) tasks. It is
intended to be used via command line:

<pre>
$> python evaluate.py [i2b2|brat] [ner|spans] GOLD SYSTEM
</pre>

It produces Precision, Recall and F1 (P/R/F1) and leak score measures for
the NER subtrack and P/R/F1 for the SPAN subtrack. The latter includes an
additional metric where the spans are merged if only non-alphanumerical
characters are found between them.

SYSTEM and GOLD may be individual files or also directories in which case
all files in SYSTEM will be compared to files the GOLD directory based on
their file names.


## Prerequisites
-------------

This software requires to have Python 3 installed on your system.


## Directory structure
-------------------

<pre>
annotated_corpora/
This directory contains files with annotations Brat annotation format. It may contain
different sub-directories for different annotation levels: tokens, sentence splitting,
part-of-speech taggin, etc. The sub-directory `sentence_splitting` is mandatory to 
compute the `leak score` evaluation metric. These files must be stored with `.ann` 
suffix.

gold/
This directory contains the gold standard files in `brat` and `i2b2` format. Each
sub-directory may contain different sub-directories for each data set: sample, train,
development, test, etc. Files in the latter directories must be in the
appropriate format: `.ann` and `.txt` for `brat`, and `.xml` for `i2b2`. 

system/
This directory contains the submission files in `brat` and `i2b2` format. Each
sub-directory may contain different sub-directories for each data set: sample, train,
development, test, etc. Each of the previous directories may contain any number of
folders, one for each system run. Files in the latter directories must be in the
appropriate format: `.ann` and `.txt` for `brat`, and `.xml` for `i2b2`. 
</pre> 


## Usage
-----


It is possible to configure the behavior of this software using the different options.


  - The `i2b2` and `brat` options allow to select the input format of the files.

  - The `ner` and `spans` options allow to select the sub-track.

  - The `gs_dir` and `sys_dir` options allow to select folders.
  
  - `Verbose` option allow to control the verbosity level.


The user can select the different options using the command line:

<pre>
usage: evaluate.py [-h] [-v]
                   {i2b2,brat} {ner,spans} gs_dir sys_dir [sys_dir ...]

Evaluation script for the MEDDOCAN track.

positional arguments:
  {i2b2,brat}    Format
  {ner,spans}    Subtrack
  gs_dir         Directory to load GS from
  sys_dir        Directories with system outputs (one or more)

optional arguments:
  -h, --help     show this help message and exit
  -v, --verbose  List also scores for each document
</pre>


## Examples

Basic Examples:

Evaluate the single system output file '01.xml' against the gold standard
file '01.xml' NER subtrack. Input files in i2b2 format:

<pre>
$> python evaluate.py i2b2 ner gold/01.xml system/run1/01.xml

Report (SYSTEM: run1):
------------------------------------------------------------
Document ID                        Measure        Micro
------------------------------------------------------------
01                                 Leak           1.462
                                   Precision      0.3333              
                                   Recall         0.1364              
                                   F1             0.1935              
------------------------------------------------------------
</pre>


Evaluate the single system output file '01.ann' against the gold standard
file '01.ann' NER subtrack. Input files in BRAT format:

<pre>
$> python evaluate.py brat ner gold/01.ann system/run1/01.ann

Report (SYSTEM: run1):
------------------------------------------------------------
Document ID                        Measure        Micro
------------------------------------------------------------
01                                 Leak           1.462
                                   Precision      0.3333              
                                   Recall         0.1364              
                                   F1             0.1935              
------------------------------------------------------------
</pre>

Evaluate the set of system outputs in the folder system/run1 against the
set of gold standard annotations in gold/ using the SPANS subtrack. Input
files in i2b2 format.

<pre>
$> python evaluate.py i2b2 spans gold/ system/run1/

Report (SYSTEM: run1):
------------------------------------------------------------
SubTrack 2 [strict]                Measure        Micro
------------------------------------------------------------
Total (15 docs)                    Precision      0.3468
                                   Recall         0.1239              
                                   F1             0.1826              
------------------------------------------------------------


                                                                      
Report (SYSTEM: run1):
------------------------------------------------------------
SubTrack 2 [merged]                Measure        Micro
------------------------------------------------------------
Total (15 docs)                    Precision      0.469
                                   Recall         0.1519              
                                   F1             0.2294              
------------------------------------------------------------
</pre>

Evaluate the set of system outputs in the folder system/run1, system/run2
and in the folder system/run3 against the set of gold standard annotations
in gold/ using the NER subtrack. Input files in BRAT format.

<pre>
$> python evaluate.py brat ner gold/ system/run1/ system/run2/ system/run3/

Report (SYSTEM: run1):
------------------------------------------------------------
SubTrack 1 [NER]                   Measure        Micro
------------------------------------------------------------
Total (15 docs)                    Leak           1.369
                                   Precision      0.3258              
                                   Recall         0.1239              
                                   F1             0.1795              
------------------------------------------------------------


                                                                      
Report (SYSTEM: run2):
------------------------------------------------------------
SubTrack 1 [NER]                   Measure        Micro
------------------------------------------------------------
Total (15 docs)                    Leak           1.462
                                   Precision      0.3333              
                                   Recall         0.1364              
                                   F1             0.1935              
------------------------------------------------------------


                                                                      
Report (SYSTEM: run3):
------------------------------------------------------------
SubTrack 1 [NER]                   Measure        Micro
------------------------------------------------------------
Total (15 docs)                    Leak           1.6
                                   Precision      0.4              
                                   Recall         0.1429              
                                   F1             0.2105              
------------------------------------------------------------

</pre>


## Contact
------

Aitor Gonzalez-Agirre (aitor.gonzalez@bsc.es)


## License
-------

Copyright (c) 2019 Secretaría de Estado para el Avance Digital (SEAD)

Permission is hereby granted, free of charge, to any person obtaining a 
copy of this software and associated documentation files (the "Software"), 
to deal in the Software without restriction, including without limitation 
the rights to use, copy, modify, merge, publish, distribute, sublicense, 
and/or sell copies of the Software, and to permit persons to whom the 
Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included 
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS 
OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE 
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER 
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, 
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN 
THE SOFTWARE.

