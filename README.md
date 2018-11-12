# utl-using-call-missing-statement-when-merging-with-missing-values
Using a retain statement when merging with missing values
   Using a retain statement when merging with missing values

   Nice stated problem and a good question. Thanks

   Good question to demomstrate the need for call missing when merging with missing values

   github
   https://tinyurl.com/y9kjhhsd
   https://github.com/rogerjdeangelis/utl-using-call-missing-statement-when-merging-with-missing-values

   INPUT
   =====

    WORK.HAVE total obs=2

    LYN    A1    A2    A3    A4    B1    B2    B3    B4    B5

     1      1     2     3     4     4     5     5     7     3
     2      5     6     7     8     4     4     5     6     7

    RULES

    Transpose and merge

                      WANT

                   |  A  B
                   |
     A1  1  B1  4  |  1  4
     A2  2  B2  5  |  2  5
     A3  3  B3  5  |  3  5
     A4  4  B4  7  |  4  7
            B5  3  |  .  3

   EXAMPLE OUTPUT
   --------------

    WORK.WANT total obs=10

       A    B

       1    4
       2    5
       3    5
       4    7
       .    3
       5    4
       6    4
       7    5
       8    6
       .    7


   PROCESS
   =======

   proc transpose data=have out=havXpo;
   by lyn;
   var a1--b5;
   run;quit;

   data want;
     call missing(A);
     merge
       havXpo(rename=col1=B where=(substr(_name_,1,1)='B'))
       havXpo(rename=col1=A where=(substr(_name_,1,1)='A')) ;
       by lyn;
       keep a b;
   run;quit;

   *                _               _       _
    _ __ ___   __ _| | _____     __| | __ _| |_ __ _
   | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
   | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
   |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

   ;


   *****combine multiple col into one or more;
   options nodate nonumber symbolgen;
   dm 'log;clear;out;clear';
   data have;
    retain lyn;
    infile datalines;
    input A1 A2 A3 A4  b1 b2 b3 b4 b5;
    lyn=_n_;
    return;
    datalines;
    1 2 3 4 4 5 5 7 3
    5 6 7 8 4 4 5 6 7
    ;
    run;

