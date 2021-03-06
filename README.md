# ILIA - Invalid Length - Invalid ASN

This project, utilises a full routing bviews dump from RIPE to query the APNIC RPKI validator to find invalid routes.

To run the tool.
## Getting Started

### Prerequisites
before running this code, you have to install this:
```
pip install future
```

## running
```
1) Download a full bviews data file from the RIPE Data BGP Archivies, we used http://data.ris.ripe.net/rrc00/2019.02/bview.20190209.0000.gz for our test.
2) unzip the file 
3) Utilising the MRT python BGP dump tool execute: 

python3 mrt2bgpdump.py ../bview.20190223.0800 | awk 'BEGIN{FS="|"} { print $6 " " $7}' | awk '{ print $1 " " $NF }' | sort -u > sorted-route-asn.txt

4) mkdir data1
5) cd data1
6) split -l 60 ../sorted-route-asn.txt #Note this will generate approximately 14500 files.
7) Run python3 retrieveroa.py
```
This will produce an output of validity_reason1.csv once it finds invalid routes.

In this code, the 50 files read and processing at the same time. If you want to change num of file, 
change the code in retrieveroa.py's pool Count

Also once the script has run, you can query the data with some simple awk commands to get a total number of INVALID route entries returned (regardless of it is a length or origin AS error) via the RPKI Validator when provided with the output of RIPE's BGP tools aggregated per ASN
```
cat validity_reason1.csv | awk '{ print $2 }' | awk 'BEGIN{FS=":"} { print $2 }' | awk 'BEGIN{FS=","} { print $1 }' | sort | uniq -c | sort -r
    338 AS12552
     62 AS14080
     54 AS17552
     46 AS3549
     45 AS50673
     26 AS9873
     11 AS6584
     10 AS15802
      9 AS37900
      9 AS18001
      8 AS45774
      8 AS27826
      8 AS12302
      7 AS28118
      6 AS394497
      6 AS202940
      5 AS8386
      4 AS8075
      4 AS42962
      4 AS38193
      4 AS27843
      4 AS27725
      4 AS265751
      4 AS199455
      4 AS199419
      4 AS18259
      4 AS137693
      4 AS134419
      3 AS56099
      3 AS55862
      3 AS53371
      3 AS52444
      3 AS50434
      3 AS395851
      3 AS2
      2 AS7473
      2 AS62240
      2 AS5483
      2 AS49344
      2 AS47215
      2 AS46562
      2 AS46261
      2 AS44491
      2 AS42010
      2 AS38713
      2 AS37989
      2 AS3326
      2 AS3255
      2 AS28086
      2 AS27953
      2 AS27901
      2 AS265719
      2 AS264609
      2 AS262589
      2 AS24989
      2 AS202623
      2 AS20207
      2 AS18678
      2 AS138466
      2 AS10753
      1 AS9541
      1 AS9498
      1 AS9484
      1 AS9329
      1 AS8897
      1 AS8068
      1 AS7604
      1 AS6648
      1 AS65552
      1 AS64252
      1 AS62597
      1 AS62031
      1 AS62021
      1 AS60721
      1 AS60300
      1 AS58779
      1 AS57219
      1 AS52308
      1 AS52255
      1 AS52145
      1 AS51957
      1 AS50289
      1 AS49666
      1 AS49425
      1 AS48374
      1 AS4134
      1 AS394308
      1 AS394183
      1 AS36667
      1 AS3598
      1 AS35328
      1 AS34636
      1 AS3320
      1 AS32604
      1 AS3257
      1 AS3215
      1 AS32034
      1 AS30781
      1 AS27650
      1 AS27471
      1 AS265816
      1 AS265758
      1 AS265515
      1 AS264686
      1 AS263771
      1 AS263756
      1 AS263223
      1 AS24651
      1 AS22411
      1 AS21349
      1 AS210126
      1 AS209737
      1 AS20940
      1 AS204813
      1 AS19916
      1 AS198605
      1 AS19429
      1 AS19120
      1 AS1881
      1 AS134766
      1 AS133585
      1 AS131155
      1 AS12969
      1 AS12735
      1 AS1221
```

Also to see the number of invalid routes based on ORIGIN AS vs LENGTH
```
cat validity_reason1.csv | awk '{ print $5 }' | awk 'BEGIN{FS=":"} { print $2 }' | awk 'BEGIN{FS="}"} { print $1 }' | sort | uniq -c

    178 as
    430 length
```

ROA count by prefix tool

This tool counts the number of IPv4 and IPv6 ROA entries by prefix.

To run the tool, simply run "python3 prefix.py"
```
ubuntu@ip-172-31-1-149:~/invalid-roa$ python3 prefix.py

Querying APNIC LABS

IPv4 /1 Routes:  0
IPv4 /2 Routes:  0
IPv4 /3 Routes:  0
IPv4 /4 Routes:  0
IPv4 /5 Routes:  0
IPv4 /6 Routes:  0
IPv4 /7 Routes:  0
IPv4 /8 Routes:  1
IPv4 /9 Routes:  1
IPv4 /10 Routes:  13
IPv4 /11 Routes:  25
IPv4 /12 Routes:  101
IPv4 /13 Routes:  127
IPv4 /14 Routes:  300
IPv4 /15 Routes:  422
IPv4 /16 Routes:  1829
IPv4 /17 Routes:  1124
IPv4 /18 Routes:  1423
IPv4 /19 Routes:  2941
IPv4 /20 Routes:  3467
IPv4 /21 Routes:  3903
IPv4 /22 Routes:  8709
IPv4 /23 Routes:  5843
IPv4 /24 Routes:  33796
IPv4 /25 Routes:  62
IPv4 /26 Routes:  61
IPv4 /27 Routes:  32
IPv4 /28 Routes:  33
IPv4 /29 Routes:  94
IPv4 /30 Routes:  22
IPv4 /31 Routes:  2
IPv4 /32 Routes:  234

Total IPv4 Routes:  64565

---

IPv6 /24 Routes:  7
IPv6 /25 Routes:  2
IPv6 /26 Routes:  13
IPv6 /27 Routes:  10
IPv6 /28 Routes:  29
IPv6 /29 Routes:  985
IPv6 /30 Routes:  83
IPv6 /31 Routes:  39
IPv6 /32 Routes:  2829
IPv6 /33 Routes:  65
IPv6 /34 Routes:  41
IPv6 /35 Routes:  56
IPv6 /36 Routes:  379
IPv6 /37 Routes:  77
IPv6 /38 Routes:  249
IPv6 /39 Routes:  274
IPv6 /40 Routes:  924
IPv6 /41 Routes:  25
IPv6 /42 Routes:  55
IPv6 /43 Routes:  61
IPv6 /44 Routes:  386
IPv6 /45 Routes:  75
IPv6 /46 Routes:  463
IPv6 /47 Routes:  138
IPv6 /48 Routes:  3819
IPv6 /49 Routes:  9
IPv6 /50 Routes:  6
IPv6 /51 Routes:  5
IPv6 /52 Routes:  9
IPv6 /53 Routes:  4
IPv6 /54 Routes:  4
IPv6 /55 Routes:  3
IPv6 /56 Routes:  15
IPv6 /57 Routes:  7
IPv6 /58 Routes:  5
IPv6 /59 Routes:  6
IPv6 /60 Routes:  9
IPv6 /61 Routes:  6
IPv6 /62 Routes:  10
IPv6 /63 Routes:  14
IPv6 /64 Routes:  247
IPv6 /65 Routes:  1
IPv6 /104 Routes:  1
IPv6 /128 Routes:  1
Total IPv6 Routes:  11446 
```
