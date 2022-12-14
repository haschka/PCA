# PCA

(c) Thomas Haschka 2022 
for a license please read the LICENSE (zlib license) file.

A tool to perform fast principal component analyses with millions of samples and thousands of features:

This tool works best (in termes of runtime) if samples >> features. But is not limited to such cases. It was tested with 2.5 millions of samples and up to around 16 000 features. 
A perfect setting is 30 000 samples 4000 features, where on recent machine results are obtained extremly fast. 

### Requirements
In order to compile and run this tools you need to have the lapack libraries,
installed. Further you need a suitable compilier. On a Debian based system you may
install all requirements in the following way:

```
sudo apt-get install build-essential liblapack-dev
```
### compilation
You may compile the tool (pca.c) in the following way, here gcc is assumed:
```
gcc -O2 -march=native pca.c -lm -llapack -pthread -o pca -s
```
### Usage
The tool can be run with the following self explainatory arguments:
```
 [FILE-in ] data to apply PCA on 
 [FILE-out] data projected ontoprincipal components
 [FILE-out] Eigenvalue Spectrum of the PCA 
 [FILE-out] Eigenvectors from the PCA 
 [int] dimensions - how many principal components to projectonto 
 [int] number of threads to use for this computation
```
where number of threads can be chosen by you. The number of cores of the machine
is a good guess.

The **input file** has to be formatted as a tabulator seperated file (tsv). 
The first column of the file is omitted and can hold sample names.
Each succeding column holds a feature ( floating point value ) describing the sample
Each row holds the information about one sample. *The input file requires at least
4 samples to yield correct results!* 

Here's an example:
```
sample1 1.5     10.2    .4      1.
sample2 2.4     21.9    1.0     2.
sample3 3.5     30.9    1.8     3.
sample4 4.5     40.9    2.5     4.
sample5 5.5     50.9    3.0     5.
```
(Beware that you cannot just copy paste this as it needs real tabulators not spaces. If you 
want to test this tool use the supplied test.tsv file. 

The **"data projected onto principal components"** output file contains 
the projections of each sample onto the principal components per row. 
Hence each line represents a sample and holds as much values as dimensions 
you might have chosen. The samples are in the same order as they were in the 
input file. 

The **"Eigenvalue Spectrum"** file shows the eigenvalues corresponding to all 
princpal components

The **"Eigenvectors** file holds number of dimensions eigenvectors corresponding
to the largest eigenvalues.

A sample usage with the supplied test file:
``` 
./pca test.tsv test.pca test.eval test.evec 2 1
```
