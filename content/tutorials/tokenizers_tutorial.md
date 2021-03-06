---
title: tokenizers tutorial
package_version: 0.1.4
---



In natural language processing, tokenization is the process of breaking human-readable text into machine readable components. The most obvious way to tokenize a text is to split the text into words. But there are many other ways to tokenize a text, the most useful of which are provided by this package.

The tokenizers in this package have a consistent interface. They all take either a character vector of any length, or a list where each element is a character vector of length one. The idea is that each element comprises a text. Then each function returns a list with the same length as the input vector, where each element in the list contains the tokens generated by the function. If the input character vector or list is named, then the names are preserved, so that the names can serve as identifiers.


### Installation


```r
install.packages("tokenizers")
```

Or development version from GitHub


```r
install.packages("devtools")
devtools::install_github("ropensci/tokenizers")
```


```r
library("tokenizers")
```


## Usage


Using the following sample text, the rest of this vignette demonstrates the different kinds of tokenizers in this package.


```r
library(tokenizers)
options(max.print = 25)

james <- paste0(
  "The question thus becomes a verbal one\n",
  "again; and our knowledge of all these early stages of thought and feeling\n",
  "is in any case so conjectural and imperfect that farther discussion would\n",
  "not be worth while.\n",
  "\n",
  "Religion, therefore, as I now ask you arbitrarily to take it, shall mean\n",
  "for us _the feelings, acts, and experiences of individual men in their\n",
  "solitude, so far as they apprehend themselves to stand in relation to\n",
  "whatever they may consider the divine_. Since the relation may be either\n",
  "moral, physical, or ritual, it is evident that out of religion in the\n",
  "sense in which we take it, theologies, philosophies, and ecclesiastical\n",
  "organizations may secondarily grow.\n"
)
```

### Characters


```r
tokenize_characters(james)[[1]]
#>  [1] "t" "h" "e" "q" "u" "e" "s" "t" "i" "o" "n" "t" "h" "u" "s" "b" "e"
#> [18] "c" "o" "m" "e" "s" "a" "v" "e"
#>  [ reached getOption("max.print") -- omitted 517 entries ]
```

You can also tokenize character-based shingles.


```r
tokenize_character_shingles(james, strip_non_alphanum = FALSE)[[1]][1:20]
#>  [1] "the" "he " "e q" " qu" "que" "ues" "est" "sti" "tio" "ion" "on "
#> [12] "n t" " th" "thu" "hus" "us " "s b" " be" "bec" "eco"
```


### Words and word stems


```r
tokenize_words(james)
#> [[1]]
#>  [1] "the"       "question"  "thus"      "becomes"   "a"        
#>  [6] "verbal"    "one"       "again"     "and"       "our"      
#> [11] "knowledge" "of"        "all"       "these"     "early"    
#> [16] "stages"    "of"        "thought"   "and"       "feeling"  
#> [21] "is"        "in"        "any"       "case"      "so"       
#>  [ reached getOption("max.print") -- omitted 87 entries ]
```

Word stemming is provided by the [SnowballC](https://cran.r-project.org/package=SnowballC) package.


```r
tokenize_word_stems(james)
#> [[1]]
#>  [1] "the"      "question" "thus"     "becom"    "a"        "verbal"  
#>  [7] "one"      "again"    "and"      "our"      "knowledg" "of"      
#> [13] "all"      "these"    "earli"    "stage"    "of"       "thought" 
#> [19] "and"      "feel"     "is"       "in"       "ani"      "case"    
#> [25] "so"      
#>  [ reached getOption("max.print") -- omitted 87 entries ]
```

You can also provide a vector of stop words. A default is provided for several languages by the `stopwords()` function.


```r
tokenize_words(james, stopwords = stopwords())
#> [[1]]
#>  [1] "question"    "thus"        "becomes"     "verbal"      "one"        
#>  [6] "again"       "our"         "knowledge"   "all"         "early"      
#> [11] "stages"      "thought"     "feeling"     "any"         "case"       
#> [16] "so"          "conjectural" "imperfect"   "farther"     "discussion" 
#> [21] "would"       "worth"       "while"       "religion"    "therefore"  
#>  [ reached getOption("max.print") -- omitted 47 entries ]
```

### Sentences and paragraphs


```r
tokenize_sentences(james)
#> [[1]]
#> [1] "The question thus becomes a verbal one again; and our knowledge of all these early stages of thought and feeling is in any case so conjectural and imperfect that farther discussion would not be worth while."                                               
#> [2] "Religion, therefore, as I now ask you arbitrarily to take it, shall mean for us _the feelings, acts, and experiences of individual men in their solitude, so far as they apprehend themselves to stand in relation to whatever they may consider the divine_."
#> [3] "Since the relation may be either moral, physical, or ritual, it is evident that out of religion in the sense in which we take it, theologies, philosophies, and ecclesiastical organizations may secondarily grow."
tokenize_paragraphs(james)
#> [[1]]
#> [1] "The question thus becomes a verbal one again; and our knowledge of all these early stages of thought and feeling is in any case so conjectural and imperfect that farther discussion would not be worth while."                                                                                                                                                                                                                                                                   
#> [2] "Religion, therefore, as I now ask you arbitrarily to take it, shall mean for us _the feelings, acts, and experiences of individual men in their solitude, so far as they apprehend themselves to stand in relation to whatever they may consider the divine_. Since the relation may be either moral, physical, or ritual, it is evident that out of religion in the sense in which we take it, theologies, philosophies, and ecclesiastical organizations may secondarily grow. "
```

### N-grams and skip n-grams


```r
tokenize_ngrams(james, n = 5, n_min = 2)
#> [[1]]
#>  [1] "the question"                   "the question thus"             
#>  [3] "the question thus becomes"      "the question thus becomes a"   
#>  [5] "question thus"                  "question thus becomes"         
#>  [7] "question thus becomes a"        "question thus becomes a verbal"
#>  [9] "thus becomes"                   "thus becomes a"                
#> [11] "thus becomes a verbal"          "thus becomes a verbal one"     
#> [13] "becomes a"                      "becomes a verbal"              
#> [15] "becomes a verbal one"           "becomes a verbal one again"    
#> [17] "a verbal"                       "a verbal one"                  
#> [19] "a verbal one again"             "a verbal one again and"        
#> [21] "verbal one"                     "verbal one again"              
#> [23] "verbal one again and"           "verbal one again and our"      
#> [25] "one again"                     
#>  [ reached getOption("max.print") -- omitted 413 entries ]
tokenize_skip_ngrams(james, n = 5, k = 2)
#> [[1]]
#>  [1] "the becomes one our all"            
#>  [2] "question a again knowledge these"   
#>  [3] "thus verbal and of early"           
#>  [4] "becomes one our all stages"         
#>  [5] "a again knowledge these of"         
#>  [6] "verbal and of early thought"        
#>  [7] "one our all stages and"             
#>  [8] "again knowledge these of feeling"   
#>  [9] "and of early thought is"            
#> [10] "our all stages and in"              
#> [11] "knowledge these of feeling any"     
#> [12] "of early thought is case"           
#> [13] "all stages and in so"               
#> [14] "these of feeling any conjectural"   
#> [15] "early thought is case and"          
#> [16] "stages and in so imperfect"         
#> [17] "of feeling any conjectural that"    
#> [18] "thought is case and farther"        
#> [19] "and in so imperfect discussion"     
#> [20] "feeling any conjectural that would" 
#> [21] "is case and farther not"            
#> [22] "in so imperfect discussion be"      
#> [23] "any conjectural that would worth"   
#> [24] "case and farther not while"         
#> [25] "so imperfect discussion be religion"
#>  [ reached getOption("max.print") -- omitted 287 entries ]
```

### Miscellaneous


```r
tokenize_lines(james)
#>  [1] "The question thus becomes a verbal one"                                   
#>  [2] "again; and our knowledge of all these early stages of thought and feeling"
#>  [3] "is in any case so conjectural and imperfect that farther discussion would"
#>  [4] "not be worth while."                                                      
#>  [5] "Religion, therefore, as I now ask you arbitrarily to take it, shall mean" 
#>  [6] "for us _the feelings, acts, and experiences of individual men in their"   
#>  [7] "solitude, so far as they apprehend themselves to stand in relation to"    
#>  [8] "whatever they may consider the divine_. Since the relation may be either" 
#>  [9] "moral, physical, or ritual, it is evident that out of religion in the"    
#> [10] "sense in which we take it, theologies, philosophies, and ecclesiastical"  
#> [11] "organizations may secondarily grow."                                      
#> [12] ""
tokenize_regex(james, pattern = "[,.;]")
#> [[1]]
#>  [1] "The question thus becomes a verbal one\nagain"                                                                                                                     
#>  [2] " and our knowledge of all these early stages of thought and feeling\nis in any case so conjectural and imperfect that farther discussion would\nnot be worth while"
#>  [3] "\n\nReligion"                                                                                                                                                      
#>  [4] " therefore"                                                                                                                                                        
#>  [5] " as I now ask you arbitrarily to take it"                                                                                                                          
#>  [6] " shall mean\nfor us _the feelings"                                                                                                                                 
#>  [7] " acts"                                                                                                                                                             
#>  [8] " and experiences of individual men in their\nsolitude"                                                                                                             
#>  [9] " so far as they apprehend themselves to stand in relation to\nwhatever they may consider the divine_"                                                              
#> [10] " Since the relation may be either\nmoral"                                                                                                                          
#> [11] " physical"                                                                                                                                                         
#> [12] " or ritual"                                                                                                                                                        
#> [13] " it is evident that out of religion in the\nsense in which we take it"                                                                                             
#> [14] " theologies"                                                                                                                                                       
#> [15] " philosophies"                                                                                                                                                     
#> [16] " and ecclesiastical\norganizations may secondarily grow"                                                                                                           
#> [17] "\n"
```


### Citing

To cite `tokenizers` in publications use:

<br>

> Lincoln Mullen (2016). tokenizers: A Consistent Interface to Tokenize
  Natural Language Text. R package version 0.1.4.
  https://CRAN.R-project.org/package=tokenizers


### License and bugs

* License: [MIT](http://opensource.org/licenses/MIT)
* Report bugs at [our Github repo for tokenizers](https://github.com/ropensci/tokenizers/issues?state=open)

[Back to top](#top)
