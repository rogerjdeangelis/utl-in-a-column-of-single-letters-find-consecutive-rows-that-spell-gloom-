# utl-in-a-column-of-single-letters-find-consecutive-rows-that-spell-gloom-
In a column of single letters find consecutive rows that spell gloom

    StackOverflow: In a column of single letters find consecutive rows that spell gloom

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

    data have;
     call streaminit(9876);
     array ltrs[26] $1 &letters (&lettersq);
     do idx=1 to 1000;
       ltr=ltrs[int(abs(rand('normal',18,2.5)))+1];
       output;
     end;
     keep ltr;
    run;quit;

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

    Up to 40 obs from WANT total obs=1

    Obs    START    LTR    LTR1    LTR2    LTR3    LTR4

     1       42      G      L       O       O       M

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

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


