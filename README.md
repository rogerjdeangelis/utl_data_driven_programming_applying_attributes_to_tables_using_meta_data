# utl_data_driven_programming_applying_attributes_to_tables_using_meta_data
Data driven programming: applying attributes to columns using meta data.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Data driven programming: applying attributes to columns using meta data

    see github
    https://tinyurl.com/yaa8ltch
    https://github.com/rogerjdeangelis/utl_data_driven_programming_applying_attributes_to_tables_using_meta_data

    see
    https://communities.sas.com/t5/SAS-Programming/how-to-assign-attributes/m-p/495899

    I downloaded your workbook and renamed it,

       d:/xls/utl_data_driven_programming_applying_attributes_to_tables_using_meta_data.xlsx

    INPUT
    =====

     ** Your excel worknook **;
     libname xel "d:/xls/utl_data_driven_programming_applying_attributes_to_tables_using_meta_data.xlsx";

     Sheet with meta data with new attributes for WORK.HAVE

     XEL.'SHEET1$'N total obs=2
     --------------------------
         Obs    VARIABLE    LENGTH    LABEL

           1    NAME           6      SUBJECT NAME
           2    COUNTRY        4      NAME OF NATIVE PALCE

     WORK.HAVE total obs=3
     ----------------------
        NAME    AGE    COUNTRY

         A       22      USA
         B       23      UK
         C       23      USA

     proc contents data=xel.'Sheet1$'n;
     run;quit;

     CONTENTS                              |  RULES (New Attributes)
     WORK.HAVE Variables in Creation Order |
                                           |
       Variable    Type    Len             |  Type    Len    Label
                                           |
       NAME        Char      8             |  Char      6    SUBJECT NAME
       AGE         Num       8             |  Char      4    NAME OF NATIVE PALCE
       COUNTRY     Char      8             |


    EXAMPLE OUTPUT
    --------------

     WORK.LOG total obs=1

                   STATUS

       Meta data extraction successful


     WORK.WANT attributes

                 Variables in Creation Order

      Variable    Type    Len    Label

      NAME        Char      6    SUBJECT NAME
      COUNTRY     Char      4    NAME OF NATIVE PALCE
      AGE         Num       8


    PROCESS
    =======

    libname xel "d:/xls/utl_data_driven_programming_applying_attributes_to_tables_using_meta_data.xlsx";

    * not needed used for development;
    %symdel lbls1 lbls2  lblsn
            lens1 lens2  lensn
            lbls1 lbls2  lblsn /nowarn;

    data log(keep=status) want(drop=status);

      if _n_=0 then do; %let rc=%sysfunc(dosubl('
         proc sql;
           select
              variable
             ,length
             ,quote(label)
           into
             :vars1-
            ,:lens1-
            ,:lbls1-
         from
           xel."sheet1$"n
         ;quit;

         %let cc=&syserr;
         %let varsn = &sqlobs;
         %let lensn = &sqlobs;
         %let lblsn = &sqlobs;

         '));
      end;

      %do_over(vars lens lbls,phrase=%str(
             attrib ?vars label=?lbls length=$?lens ;
      ))

      set have end=dne;

      output want;

      if dne then do;
         if symgetn('cc')=0 then status="Meta data extraction successful";
         else status="Meta data extraction failed";
         output log;
      end;

    run;quit;


    OUTPUT
    ======
     WORK.LOG total obs=1

                   STATUS

       Meta data extraction successful


     WORK.WANT attributes

                 Variables in Creation Order

      Variable    Type    Len    Label

      NAME        Char      6    SUBJECT NAME
      COUNTRY     Char      4    NAME OF NATIVE PALCE
      AGE         Num       8


    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    download and rename excel workbook


