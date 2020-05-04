
======================================
= README
======================================

*DBGen-A Probabilistic Data Generation for
Deduplication and Data Linkage*

Author: Sepideh Mosaferi; Aug 1, 2016


Contents
1. Sub-folder "dbgen"
2. output.db file
3. 4 csv files

======================================
1. Sub-folder "dbgen"
======================================
This sub-folder is the main dbgen program. One can run the main data generator 
code from the folder called "zipfgen_dbgen.c" to see how the program works.

======================================
2. output.db file
======================================
This is the output of dbgen program after running.

======================================
3. 4 csv files
======================================
dbgen_synthpop.csv: We have transfered the output.db file to a csv file which
is more convenient to work with across different software.
   
dbgen_Shuffle.csv: This is the data that we have considered for de-duplication 
via Febrl. But before using it in the Febrl, we have permuted the data based on 
the R code.

dbgen_Split1.csv and dbgen_Split2.csv: We have splitted the dbgen_Shuffle
dataset into 2 datasets which could be used for the record linkage purpose.

======================================
Introduction
======================================

Currently, comparison among record linkage software becomes a critical issue when agencies
are interested to know which software is more reliable, fast, and efficient for their particular
application. But usually, each researcher or organization uses their own data set which
might not be in the public domain and does not contain enough attributes. These make the
comparison of software almost infeasible. Therefore, there is a huge demand for producing
an artificially generated data as an alternative source which can mimic the interested real
data in the aspects of population shape and characteristics, duplications, typographical
errors, etc. The existence of such a data brings an opportunity for the researchers in each
sector of science including survey methodology, medical cares among others to decide which
record linkage software works more beneficially and appropriately in terms of time, cost,
and evaluation criteria, distance measure, string comparator for their goals. Here,
we introduce a data generator software with open source code, called DBGen.

The DBGen software so-called UIS Database Generator allows the creation of data which
contains duplicates. The program was written by Mauricio Hernandez in C in 1998. Peter
Christen and Ajus Pudjijuno extended Hernandez’s work in the Febrl. The Febrl data
generator allows one to create template records that are parts of groups, such as families.
It also allows one to define detailed statistical models for correlations between the field
values of records that belong to the same group 1.

An improved version of the DBGen software generator produced by Bertolazzi et al. (2003)
allows for missing attribute values and increased variability in the set of possible generated
values. But unfortunately, the source is not publicly available. 

======================================
DBGEN Installation and Use
======================================
How to run the software?

After downloading the software from http://www.cs.utexas.edu/users/ml/riddle/data.html, 
we need to read the README and README.v2 carefully. There are 2 C codes in
the folder. The zipfgen-dbgen.c is the new C code that we implement and explain here.
This code uses two zip files ”mnames” and ”zipcodes” for generating the data set. However,
we can substitute our own name and address files instead of them to run the program.

Under Windows System

To run the program, we need to have PuTTY which is connected to the LINUX package
and compile the program there. Another way is installing a virtual machine (e.g., Oracle 
VM VirtualBox Manager) and creating a Linux Box and running the program there.
Unfortunately, this takes a lot of computer power and decreases the speed of the machine.

Under Linux System

We can simply run the program from the Linux terminal. As we used the Linux system
to run the program, here we exclusively explain the related command lines for the Linux
users. To do so, we need to execute particular command lines (often familiar to C programmers). 
However, these commands can be easily executed by the Windows system under the programs that 
we have already mentioned.

After entering the codes, a page shows up that ask you to do some changes or enter your
preferences to generate the data.
Some of the software requirements are as follows:
 • needs a list of names, cities, states, and postcodes (original data).
 • needs a large number of parameters including
   * the size of data base to be generated,
   * percentage and distribution of duplicates (probability distribution of how many du-
     plicates need to be created), and
   * the amount and types of errors should be introduced (maximum number of errors to
     be introduced into one attribute in a record and into a record as a whole).


The software only uses two parameters to control the size and distribution of the output:
  * n : The (approximate) number of records to be generated.
  * c : The number of clusters (i.e., persons in the data) to be distributed among the n
        records.


The user can select three distributions to generate the data:
  * uniform (default): c clusters will be formed. The average number of records per
    cluster will be n/c.
  * poisson: The n records are distributed into c clusters using a poisson distribution
    where n/c is the mean.
  * zipf: The n records are distributed into c clusters using a zipf distribution. The user
    must provide the zipf parameter “theta” (0 ≤ theta ≤ 1). theta=1 should give you a
    uniform distribution. As you make theta smaller, the “skewness” of the distribution
    will increase (i.e., you will start getting a small number of clusters that contain a
    large number of duplicates and a large number of clusters with only 1 or 2 records).


Examples
1. For the poisson distribution 2 :
./dbgen -poisson -n 100 -c 10 -q -i -o output.db
2. For the Zipf (theta=0.25) distribution:
./dbgen -z 0.25 -n 100 -c 10 -q -i -o output.db


======================================
Software Output
======================================
The software produces a set of records with 12 information on them:
  • CUID : Cluster Unique ID. This is a unique number for each cluster of records.
    Basically, this is the “key” of the data (you can cluster the duplicate records if you
    sort the database using this field).
  • RUID : Record Unique ID. A unique id for each record. This is a counter that goes
    from 0 to N-1 where N is the number of records.
  • SSN : A 9-digit social security number. Invalid or NULL SSNs are represented by
    ‘000000000’.
  • FNAME: The first name.
  • MINIT: A middle initial.
  • LNAME: The last name.
  • STNUM: The street number part of the address. This field is empty in case of P.O.
    Boxes.
  • STADD: The street name or the P.O. Box.
  • APMT : The apartment number, if applicable.
  • CITY : The name of a city.
  • STATE: A two-letter code for the State.
  • ZIP : A 5-digit zipcode.

If we consider the following options

Generate SSN= 100.00%
Error Probability= 0.00%

we can assign unique social security numbers to the individuals.
To this end, in order to be able to run the program, we need to keep the following items

Insertions
Deletions
Replacements
Swappings

for both street names and person names in a way to add up to 100%; otherwise, the
program does not work, and an error message comes up.


======================================
Experimental Study
======================================
In order to evaluate the ability of DBGen for generating the data set, we have implemented
an experimental study. In this study, we use an un-official list of baby girl and boy names
with their frequencies in California: 2015. We realized that using the DBGen mnames zip
file itself might not be appropriate enough for evaluating the software as each name has
the frequency of one. However, after our study, we realized that the zipcode file under the
DBGen is good enough for generating the addresses. The original data set related to the
names that we considered here has Total Births: Girls=240311 and Boys=252972 comes
from an Automated Vital Statistics System under the Institute for Social, Behavioral, and
Economic Research at the University of California, Santa Barbara, CA 93106. The web
page is: http://www.avss.ucsb.edu/NameGB.HTM.

Among these names, we considered 1,938 unique names with a total frequency of 379,109To
evaluate the software; we follow Christen’s paper in 2005. We consider all three available
distributions (Uniform, Zipf, and Poisson) to generate the data. We should pay attention
to have the true population; n should be equal to c (n=c). Since we do not have the list
of addresses, and we only have the list of names, we need to generate the true population
based on our real data name file and DBGen available address file under each distribution
separately. These created true data sets might be different from one distribution to another.
Therefore, it is not appropriate to have a comparison among them, and we need to compare
them separately under its associated distribution.

For generating the true population, we considered n=c, and all of the error equal to zero
except the Insertions, Deletions, Replacements, and Swappings in the street names and
person names sections (since they should add up to 100.00%). 
Then to evaluate the duplicated data set under the Uniform distribution, we considered
%5 duplication (c=360,154), %10 duplication (c=341,198), %20 duplication (c=303,287),
and %40 duplication (c=227,465). 
We followed the information for generating the data under the Uniform distribution for
the Zipf distribution.

For the Poisson distribution, we considered a true population with the size n=1000. There-
fore, we randomly select 1000 names from our list and consider it as the input of mnames
zip file. To generate the real data set, we need to attach the addresses to the names and
consider n=c. But unfortunately, we were unable to generate the true data set as we did
not have enough memory in our personal PC. Therefore, we would recommend a user to
consider enough memory for generating the data set based on the Poisson distribution.
However, we considered %80, %90, and %95 duplications, where c is 200, 100, and 50,
respectively. 
Finally, the results for the average frequencies and standard deviations of selected attribute
values in the data set (original and generated data with given percentage of duplicates)
based on each distribution are calculated.











