# utl-in-a-column-of-single-letters-find-consecutive-rows-that-spell-gloom-
    In a column of single letters find consecutive rows that spell gloom

      Three solutions


        a. Without autoexec macro variables -lag4
           4 LAG stacks might work as well as your code that has 4 additional data set i/o buffers.
           In the following random letters are selected via BYTE (only works for ascii sessions)
           Richard DeVenezia
           rdevenezia@gmail.com

        b. With autoexec macro variables - merge
           I forgot that macro variables letters and letterq were
           defined in my autoexec along with months, monthsq ,numbers, numbersq, states50 and states50q

        c. Comments and a solution by Nat and Quentin

           WILD UNFINISHED IDEA MAY RELATE TO THIS OR ANOTHER SOLUTION
           Nat Wooding
           nathani@verizon.net

           Quentin McMullen
           qmcmullen.sas@gmail.com



    * AUTOEXEC VARIABLES;

    %let lettersq="A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z";
    %let letters=A B C D E F G H I J K L M N O P Q R S T U V W X Y Z;

    %let numbersq=%str("1","2","3","4","5","6","7","8","9","10");
    %let numbers=1 2 3 4 5 6 7 8 9 10;

    %let states50= %sysfunc(compbl(AL AK AZ AR CA CO CT DE FL GA HI ID IL IN IA KS KY LA ME MD MA MI MN MS MO MT
    NE NV NH NJ NM NY NC ND OH OK OR PA RI SC SD TN TX UT VT VA WA WV WI WY)) ;

    %let states50q="AL","AK","AZ","AR","CA","CO","CT","DE","FL","GA","HI","ID","IL","IN","IA","KS","KY","LA","ME","MD","MA","MI","MN","MS","MO","MT",
    "NE","NV","NH","NJ","NM","NY","NC","ND","OH","OK","OR","PA","RI","SC","SD","TN","TX","UT","VT","VA","WA","WV","WI","WY";

    %let months= jan feb mar apr may jun jul aug sep oct nov dec;
    %let monthsq="jan","feb","mar","apr","may","jun","jul","aug","sep","oct", "nov", "dec" ;


    github
    https://tinyurl.com/twj8p7u
    https://github.com/rogerjdeangelis/utl-in-a-column-of-single-letters-find-consecutive-rows-that-spell-gloom-

    related to but slightly different

    Stackoverflow
    https://tinyurl.com/sv6dj4h
    https://stackoverflow.com/questions/59417994/identifying-a-specific-pattern-in-several-adjacent-rows-of-a-single-column-r

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    * Without autoexec macro variables;

    data have;
      call streaminit(80);
      do idx=1 to 7500;
        ltr=byte(65 + rand('uniform', 26));
        output;
      end;
      keep ltr;
    run;

    6413     A
    6414     I
    6415     E

    6416     G  * starts here
    6417     L
    6418     O
    6419     O
    6420     M

    6421     D
    6422     N
    6423     O
    6424     G


    * WITH AUTOEXEC VARIABLES;

    data have;
     call streaminit(9876);
     array ltrs[26] $1 &letters (&lettersq);
     do idx=1 to 1000;
       ltr=ltrs[int(abs(rand('normal',13,2.5)))+1];
       output;
     end;
     keep ltr;
    run;quit;

    * WITH AUTOEXEC VARIABLES;


    WORK.HAVE total obs=1,000

      Obs    LTR

        1     O
        2     K
        3     N
    ...
       38     R
       39     O
       40     M
       41     O

       42     G   * hear it is
       43     L
       44     O
       45     O
       46     M

       47     P
       48     P
       49     N
       50     P

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    * WITHOUT AUTOEXEC VARIABLES;

    * easy to add positions and text  'GLOO'
    WORK.WANT total obs=1
                   GLOOM_
                  STARTS_
    Obs    LTR       AT
     1      M        42
    * WITH AUTOEXEC VARIABLES;

    WORK.WANT total obs=1

    Obs    START    LTR    LTR1    LTR2    LTR3    LTR4

     1       42      G      L       O       O       M

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/
    ;
    * WITHOUT AUTOEXEC VARIABLES;

    data have;
      call streaminit(80);
      do idx=1 to 7500;
        ltr=byte(65 + rand('uniform', 26));
        output;
      end;
      keep ltr;
    run;

    data want;
      set have;
      if lag4(ltr) = 'G'
       & lag3(ltr) = 'L'
       & lag2(ltr) = 'O'
       & lag1(ltr) = 'O'
       &      ltr  = 'M'
      then do;
        gloom_starts_at = _n_ - 4;
        output;
      end;
    run;

    proc print data=have(firstobs=6413 obs=6424);
    run;quit;


    * WITH AUTOEXEC VARIABLES;

    data have;
     call streaminit(9876);
     array ltrs[26] $1 &letters (&lettersq);
     do idx=1 to 1000;
       ltr=ltrs[int(abs(rand('normal',13,2.5)))+1];
       output;
     end;
     keep ltr;
    run;quit;

    data want;

      retain start;

      merge
            have
            have(firstobs=2 rename=ltr=ltr1)
            have(firstobs=3 rename=ltr=ltr2)
            have(firstobs=4 rename=ltr=ltr3)
            have(firstobs=5 rename=ltr=ltr4)
      ;

      start=_n_;

      if
            ltr ='G' and
            ltr1='L' and
            ltr2='O' and
            ltr3='O' and
            ltr4='M'
      ;
    run;quit;

    *                                                  _
      ___      ___ ___  _ __ ___  _ __ ___   ___ _ __ | |_ ___
     / __|    / __/ _ \| '_ ` _ \| '_ ` _ \ / _ \ '_ \| __/ __|
    | (__ _  | (_| (_) | | | | | | | | | | |  __/ | | | |_\__ \
     \___(_)  \___\___/|_| |_| |_|_| |_| |_|\___|_| |_|\__|___/

    ;

    Assuming that we are looking at a single variable, I am starting to kick around a SASHOLE
    approach of using poke to grab the entire string, maybe stick it in a temporary file, and
    then read it with a input and trailing @@, sort of like

    Data Gloom;/* the file is this string repeated twice : abcdmoolgefghighlmnop */
    Filename txt "C:\Natsas\PGMS2019\Data\gloom.txt";infile txt lrecl = 32000 ;input @'moolg' A $1.  @@;run;
    I can't recall how to learn the location of the SAS pointer in the string.
    Nat

        On Saturday, December 28, 2019, 06:45:28 PM EST, Quentin McMullen <qmcmullen.sas@gmail.com> wrote:

     Of course where you can LAG, you can also just RETAIN, e.g.:


    data want ;
      set have ;
      retain str ;
      length str $5 ;
      str=cat(ltr,str) ;
      if str="MOOLG" then do
        gloom_starts_at = _n_ - 4;
        put gloom_starts_at = ;
        output;
      end ;
    run ;

    Kind Regards,
    --Q.
