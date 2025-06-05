# Class 6: R Functions
Claire Lua A16922295

- [1. Function Basics](#1-function-basics)
- [2. Generate DNA Function](#2-generate-dna-function)
- [3. Generate Protein Function](#3-generate-protein-function)

## 1. Function Basics

Let’s start writting our first silly function to add some numbers:

Every R function has 3 things: - name (we get to pick this) - input
arguments (there can be loads of these separated by a comma) - the body
(the R code that does the work)

``` r
add <- function(x, y=100, z=0) {
  x + y + z
}
```

I can just use this function like any other function as long as R knows
about it (i.e. run the code chunk)

``` r
add(1, 100)
```

    [1] 101

``` r
add(x=c(1,2,3,4), y=100)
```

    [1] 101 102 103 104

``` r
add(1)
```

    [1] 101

Functions can have “required” input arguments and “optional” input
arguments. The optional arguments are defined with an equals default
value (`y=0`) in the function definition

``` r
add(1,100,10)
```

    [1] 111

## 2. Generate DNA Function

> Q. Write a function to return a DNA sequence of a user specified
> length. Call it `generate_dna()`

The `sample()` function can help here

``` r
generate_dna <- function(size=5) {
  
}

students <- c("jeff", "jeremy", "peter")

sample(students, size = 5, replace = TRUE)
```

    [1] "jeremy" "jeremy" "jeff"   "peter"  "jeff"  

Now work with `bases` rather than `students`

``` r
bases <- c("A", "C", "G", "T")
sample(bases, 10, TRUE)
```

     [1] "G" "T" "A" "T" "A" "G" "T" "A" "T" "T"

Now I have a working `snippet` of code, I can use this as the body of my
first function version here:

``` r
generate_dna <- function(size=5) {
  bases <- c("A", "C", "G", "T")
  sample(bases, size, TRUE)
}
```

``` r
generate_dna(100)
```

      [1] "T" "T" "C" "C" "A" "C" "C" "G" "G" "A" "A" "C" "C" "T" "C" "C" "G" "T"
     [19] "C" "G" "A" "T" "G" "A" "C" "T" "G" "G" "C" "C" "T" "T" "G" "G" "T" "T"
     [37] "A" "T" "G" "A" "T" "T" "C" "C" "G" "A" "A" "C" "G" "T" "A" "T" "A" "C"
     [55] "T" "T" "G" "T" "G" "G" "G" "A" "A" "G" "T" "T" "G" "A" "G" "A" "G" "A"
     [73] "A" "A" "A" "G" "G" "C" "G" "A" "A" "C" "C" "T" "A" "T" "T" "C" "C" "A"
     [91] "G" "C" "T" "C" "G" "C" "G" "G" "T" "G"

I want the ability to return a sequence like “AGTACCTG” i.e. a one
element vector where the bases are all together.

``` r
generate_dna <- function(size=5, together=TRUE) {
  bases <- c("A", "C", "G", "T")
  sequence <- sample(bases, size, TRUE)
  if(together){
    sequence <- paste(sequence, collapse="")
  } 
  return(sequence)
}
```

``` r
generate_dna(100)
```

    [1] "ACATCACCTAAAAGCCCACTTCTGCTACATGGACAATCGGAGCTCTTAGTGCGGATAAAATCACACTACTTATTGAATCTTCACAGATTTGTAGGAATGG"

## 3. Generate Protein Function

> Q. Write a protein sequence generating function that will return
> sequences of a user specified length

We can get the set of 20 natural amino-acids from the **bio3d** package.

``` r
generate_protein <- function(size=6, together=TRUE){
  ##Get the 20 amino acid
  aa <- bio3d::aa.table$aa1[1:20]
  sequence <- sample(aa, size, TRUE)
  
  ##Optionally return a single element string
  if(together){
    sequence <- paste(sequence, collapse="")
  }
  return(sequence)
}
```

``` r
generate_protein(10)
```

    [1] "CEPCATSEQW"

> Q. Generate random protein sequences of length 6 to 12 amino acids

``` r
##generate_protein(6:12)
```

We can fix this inability to generate multiple sequences by either
editing and adding to the function body code (e.g. a for loop) or by
using the R **apply** family of utility functions

``` r
ans <- sapply(6:12, generate_protein)
```

It would be cool and useful if I could get FASTA format output

``` r
id.line <- paste(">ID.", 6:12, sep="")
seq.line <- paste(id.line, ans, sep="\n")
cat(seq.line, sep="\n")
```

    >ID.6
    EATKNH
    >ID.7
    QYMGWHN
    >ID.8
    HPWVLQID
    >ID.9
    NPMNWGPHY
    >ID.10
    VYCGHDHNCN
    >ID.11
    CKPWPRLCDNA
    >ID.12
    QCHDHDHNTDVV

> Q. Determine if these sequences can be found in nature or are they
> unique? Why or why not?

I BLASTp searched my FASTA format sequences against NR and found that
length 6, 7, and 8 are not unique and can be found in the databases with
100% coverage and 100% identity.

Random sequences of length 9 and above are unique and can’t be found in
the databases.
