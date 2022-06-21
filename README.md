# AbqAq_Parser
Parser for Abq, N.M. Air Quality data format


## Abstract

This file: abqaq.py will parse a file downloaded from Abq N.M.'s web site for air quality readings

It currently just checks the syntax of the file.
If the data file parses correctly, it will output a badly formed AST (list of
lists) and exit with a 0 status code.


If there is some fault in the data file or the program itself, it prints an error message
on stderr and exits with a status code of 1.

## Setup

Note: If running on a Debian-derived system, replace the python and pips with python3 and pip3.

This program works best with Python version 3.10. It may work with older versions, but they have not been tested.

Note: This step is optional. If you do not use virtual environment, 'parsy'
will be installed in your current Python gloval packages.

0. Create a virtual environment.

```bash
python -m venv .venv
source ./.venv/bin/activate
```

Note: You can deactivate the virtual environment anytime by:

```bash
deactivate
```


1.  Install the dependancies

```bash
pip install -r requirements.txt
```


## Running the parser

2. Dowload a data file

```bash
curl http://data.cabq.gov/airquality/aqindex/history/042222.0017 > data.dat
```


3. Run the program

```bash
python abqaq.py  data.dat
```

Note: data.dat can also be redirected to stdin. Or you can use curl:

```bash
curl http://data.cabq.gov/airquality/aqindex/history/042222.0017 > | python abqaq.py
```



## Sample output data


### IR (intermediate Representation) from first stage parser
[data.ir](data.ir)

### Transformed dictionary after second phase

[data.dct](data.dct)

### Final YAML output

[data.yml](data.yml)

## Data Sources

### City of Albur, New Mexico website

- [https://www.cabq.gov](https://www.cabq.gov)

### Historical air quality data directory

- [http://data.cabq.gov/airquality/aqindex/history/](http://data.cabq.gov/airquality/aqindex/history/)


Note: Only 7 files from this directory have been checked.
Various changes have been noted and been addressed by making the parser more forgiven.
Most of these items have been due to either differences in the format of data section
value lists or some garbage data.

E.g. 
Instead of '0.9923', a single field might have '.9923'.


## Grammar

The grammar was reversed engineered from the sample files.  This is not ideal due
to unforseen changes in the actual data in other files. See note above.

This grammar is the 3rd attempt, at least. The grammar is represented in Extended
Backus Naur Format or EBNF. The varient used is the type that uses operators from
RegExpLand. E.g. '*', '+', '?' and parens for grouping.

This grammar is extracted from the file: 'abqaq.py'. It employs the the strategy
capturing all the terminals (that are not commaa or line endings) in elements
of a list and capturing the nonterminals as lists or lists of lists.


```EBNF
/* terminals are listed first and lowercase. Names should be self-explanatory
*/

<KeyValueOpt> ::= 

<DataLine ::=> 

<DataSection> ::= 

<GroupSection> ::= 

<GroupSectionList> ::= <GroupSection>+

<FileSection> ::= 
```


### BNF Playground

This site was used to develop the EBNF used above.

- [https://bnfplayground.pauliankline.com/](https://bnfplayground.pauliankline.com/)



