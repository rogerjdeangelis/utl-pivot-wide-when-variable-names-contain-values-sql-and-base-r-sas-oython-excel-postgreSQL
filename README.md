# utl-pivot-wide-when-variable-names-contain-values-sql-and-base-r-sas-oython-excel-postgreSQL
Pivot wide when variable names contain values sql and base r sas python excel postgreSQL 
    %let pgm=utl-pivot-wide-when-variable-names-contain-values-sql-and-base-r-sas-oython-excel-postgreSQL;

    %stop_submission;

    Pivot wide when variable names contain values sql and base r sas python excel postgreSQL

    This is an interesting transpose and very hard to do in sql, but doable.

    github
    Best viewed with the github compact font or sas-l text option. (these should be the default?
    https://tinyurl.com/2s92n76c
    https://github.com/rogerjdeangelis/utl-pivot-wide-when-variable-names-contain-values-sql-and-base-r-sas-oython-excel-postgreSQL

    stackoverflow
    https://tinyurl.com/ymu776ft
    https://stackoverflow.com/questions/79378851/mutating-to-return-the-average-of-a-column-in-each-row

              SOLUTIONS

                  1 sas proc transpose
                  2 r data table language
                  3 sas 1 proc sql
                  4 sas sql arrays
                  5 r sql
                  6 python sql
                  7 postgresql
                    SOAPBOX ON
                      Python is deprecating panda sql for
                      a more complex interface.
                      Moving away from tightly integrated sql
                      like sas proc sql and sqldf?
                    SOAPBOX OFF
                  8 excel sql

    FYI
     SQL Server and Oracle offer dedicated PIVOT and UNPIVOT clauses

    Related elational interfaces with r

    POSTGREQQL
    Relayted relational data
    https://github.com/rogerjdeangelis/utl-saving-and-creating-r-dataframes-to-and-from-a-postgresql-database-schema
    https://github.com/rogerjdeangelis/utl-partial-key-matching-and-luminosity-in-gene-analysis-sas-r-python-postgresql

    MYSQL
    https://github.com/rogerjdeangelis/mySQL-uml-modeling-to-create-entity-diagrams-for-sas-datasets
    https://github.com/rogerjdeangelis/utl-PASSTHRU-to-mysql-and-select-rows-based-on-a-SAS-dataset-without-loading-the-SAS-daatset-into-my
    https://github.com/rogerjdeangelis/utl-accessing-a-mysql-database-using-R-without-the-sas-access-product
    https://github.com/rogerjdeangelis/utl-mysql-queries-without-sas-using-r-python-and-wps
    https://github.com/rogerjdeangelis/utl-rename-and-cast-char-to-numeric-using-do-over-arrays-in-natve-and-mysql-wps-r-python
    https://github.com/rogerjdeangelis/utl_examples_SAS_mysql_interface_on_power_workstation
    https://github.com/rogerjdeangelis/utl_explicit_pass_through_to_mysql_to_subset_a_table_using_macro_variable_dates
    https://github.com/rogerjdeangelis/utl_exporting_a_sas_single_60k_string_to_mysql_and_reading_it_back_into_two_30k_strings
    https://github.com/rogerjdeangelis/utl_extract_from_mySQL_add_index
    https://github.com/rogerjdeangelis/utl_sql_update_master_using_a_transaction_table_mysql_database
    https://github.com/rogerjdeangelis/utl_with_a_press_of_a_function_key_convert_the_highlighted_dataset_to_a_mysql_database_table

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                  |                                           |                                         */
    /*           INPUT                  |          PROCESS                          |      OUTPUT                             */
    /*           ====                   |          =======                          |      ======                             */
    /*                                  |                                           |                                         */
    /*   ID    X_2010    X_2011         | 1 SAS TRANSPOSE                           |_NAME_ COL1 COL2 COL3 COL4 COL5 COL6     */
    /*                                  | ===============                           |                                         */
    /*    1       5         8           |                                           | ID       1    2    3    1    2    3     */
    /*    2       6         9           | data havStack;                            | YEAR  2010 2010 2010 2011 2011 2011     */
    /*    3       7        10           |                                           | X        5    6    7    8    9   10     */
    /*                                  |  set                                      |                                         */
    /*  options                         |   sd1.have (keep=id x_2010) /*t2011=0 */  |                                         */
    /*   validvarname=upcase;           |   sd1.have (keep=id x_2011 in=t2011)      |                                         */
    /*  libname sd1 "d:/sd1";           |   ;                         /*t2011=1 */  |                                         */
    /*  data sd1.have;                  |                                           |                                         */
    /*  input iD X_2010 X_2011;         |  /*---- after sets                        |                                         */
    /*  cards4;                         |    ID X_2010 X_2011                       |                                         */
    /*  1 5 8                           |     1    5      .                         |                                         */
    /*  2 6 9                           |     2    6      .                         |                                         */
    /*  3 7 10                          |     3    7      .                         |                                         */
    /*  ;;;;                            |     1    .      8                         |                                         */
    /*  run;quit;                       |     2    .      9                         |                                         */
    /*                                  |     3    .     10                         |                                         */
    /*                                  |  ----*/                                   |                                         */
    /*  proc transpose data=sd1.have    |                                           |                                         */
    /*    out=havXpo;                   |   array nam x_:;                          |                                         */
    /*  by id;                          |                                           |                                         */
    /*  run;quit;                       |   year= input(                            |                                         */
    /*                                  |      substr(vname(nam[t2011+1]),3),5.);   |                                         */
    /*                                  |   x=sum(of nam[*});                       |                                         */
    /*                                  |                                           |                                         */
    /*                                  |   drop x_:;                               |                                         */
    /*                                  | run;quit;                                 |                                         */
    /*                                  |                                           |                                         */
    /*                                  |  ID     X    YEAR                         |                                         */
    /*                                  |                                           |                                         */
    /*                                  |   1     5    2010                         |                                         */
    /*                                  |   2     6    2010                         |                                         */
    /*                                  |   3     7    2010                         |                                         */
    /*                                  |   1     8    2011                         |                                         */
    /*                                  |   2     9    2011                         |                                         */
    /*                                  |   3    10    2011                         |                                         */
    /*                                  |                                           |                                         */
    /*                                  | proc transpose data=havStack out=havxpo;  |                                         */
    /*                                  | run;quit;                                 |                                         */
    /*                                  |                                           |                                         */
    /*                                  | _NAME_ COL1 COL2 COL3 COL4 COL5 COL6      |                                         */
    /*                                  |                                           |                                         */
    /*                                  |  ID       1    2    3    1    2    3      |                                         */
    /*                                  |  YEAR  2010 2010 2010 2011 2011 2011      |                                         */
    /*                                  |  X        5    6    7    8    9   10      |                                         */
    /*                                  |                                           |                                         */
    /*                                  |-------------------------------------------------------------------------------------*/
    /*                                  |                                           |                                         */
    /*                                  | 2 R DATA TABLE LANGUAGE                   |                                         */
    /*                                  | =======================                   |                                         */
    /*                                  |                                           |                                         */
    /*                                  | res <- melt(y                             |                                         */
    /*                                  |       ,id.vars = "ID"                     |                                         */
    /*                                  |       ,variable.name = "YEAR"             |                                         */
    /*                                  |       ,value.name = "X")                  |                                         */
    /*                                  |                                           |                                         */
    /*                                  |    ID   YEAR     X                        |                                         */
    /*                                  | <num> <fctr> <num>                        |                                         */
    /*                                  |     1 X_2010     5                        |                                         */
    /*                                  |     2 X_2010     6                        |                                         */
    /*                                  |     3 X_2010     7                        |                                         */
    /*                                  |     1 X_2011     8                        |                                         */
    /*                                  |     2 X_2011     9                        |                                         */
    /*                                  |     3 X_2011    10                        |                                         */
    /*                                  |                                           |                                         */
    /*                                  | res[,                                     |                                         */
    /*                                  |    YEAR := as.integer(gsub("X_"           |                                         */
    /*                                  |    ,""                                    |                                         */
    /*                                  |    ,YEAR                                  |                                         */
    /*                                  |    ,fixed = TRUE))                        |                                         */
    /*                                  |    ]                                      |                                         */
    /*                                  |                                           |                                         */
    /*                                  |    ID  YEAR     X                         |                                         */
    /*                                  | <num> <int> <num>                         |                                         */
    /*                                  |     1  2010     5                         |                                         */
    /*                                  |     2  2010     6                         |                                         */
    /*                                  |     3  2010     7                         |                                         */
    /*                                  |     1  2011     8                         |                                         */
    /*                                  |     2  2011     9                         |                                         */
    /*                                  |     3  2011    10                         |                                         */
    /*                                  |                                           |                                         */
    /*                                  | want<-as.data.frame(t(res))               |                                         */
    /*                                  |                                           |                                         */
    /*                                  | > want                                    |                                         */
    /*                                  |        V1   V2   V3   V4   V5   V6        |                                         */
    /*                                  | ID      1    2    3    1    2    3        |                                         */
    /*                                  | YEAR 2010 2010 2010 2011 2011 2011        |                                         */
    /*                                  | X       5    6    7    8    9   10        |                                         */
    /*                                  |                                           |                                         */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    options
     validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input iD X_2010 X_2011;
    cards4;
    1 5 8
    2 6 9
    3 7 10
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  ID    X_2010    X_2011                                                                                                */
    /*                                                                                                                        */
    /*   1       5         8                                                                                                  */
    /*   2       6         9                                                                                                  */
    /*   3       7        10                                                                                                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                 _
    / | ___  __ _ ___  | |_ _ __ __ _ _ __  ___ _ __   ___  ___  ___
    | |/ __|/ _` / __| | __| `__/ _` | `_ \/ __| `_ \ / _ \/ __|/ _ \
    | |\__ \ (_| \__ \ | |_| | | (_| | | | \__ \ |_) | (_) \__ \  __/
    |_||___/\__,_|___/  \__|_|  \__,_|_| |_|___/ .__/ \___/|___/\___|
                                               |_|
    */          ;;;;%end;%mend;/*'*/ *);*};*];*/;/*"*/;run;quit;%end;end;run;endcomp;%utlfix;

    data havStack;

      set
        sd1.have (keep=id x_2010)
        sd1.have (keep=id x_2011 in=t2011)
      ;

      array nam x_:;

      year= input(
         substr(vname(nam[t2011+1]),3),5.);
      x=sum(of nam[*});

      drop x_:;
    run;quit;

    /*----
     ID     X    YEAR

      1     5    2010
      2     6    2010
      3     7    2010
      1     8    2011
      2     9    2011
      3    10    2011
    ----*/

    proc transpose data=havStack out=havxpo;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  _NAME_    COL1    COL2    COL3    COL4    COL5    COL6                                                                */
    /*                                                                                                                        */
    /*   ID          1       2       3       1       2       3                                                                */
    /*   YEAR     2010    2010    2010    2011    2011    2011                                                                */
    /*   X           5       6       7       8       9      10                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___               _       _          _        _     _
    |___ \   _ __    __| | __ _| |_ __ _  | |_ __ _| |__ | | ___
      __) | | `__|  / _` |/ _` | __/ _` | | __/ _` | `_ \| |/ _ \
     / __/  | |    | (_| | (_| | || (_| | | || (_| | |_) | |  __/
    |_____| |_|     \__,_|\__,_|\__\__,_|  \__\__,_|_.__/|_|\___|

    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(tidyverse)
    library(data.table)
    source("c:/oto/fn_tosas9x.R")
    y <- structure(list(a = 1:3, b = 5:7, c=8:10), .Names = c("ID"
    , "X_2010","X_2011")
    , row.names = c(NA, -3L)
    , class = "data.frame")
    y
    library(data.table)

    y<-read_sas("d:/sd1/have.sas7bdat")
    # convert to data.table
    setDT(y)

    # reshape wide to long
    res <- melt(y, id.vars = "ID", variable.name = "YEAR", value.name = "X")
    print(res)
    # remove X from year
    res[, YEAR := as.integer(gsub("X_", "", YEAR, fixed = TRUE)) ]

    # transpose
    want<-as.data.frame(t(res))
    res
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                      |                                                                                 */
    /*   want                               |                                                                                 */
    /*                                      |                                                                                 */
    /*         V1   V2   V3   V4   V5   V6  |  ROWNAMES      V1      V2      V3      V4      V5      V6                       */
    /*                                      |                                                                                 */
    /*  ID      1    2    3    1    2    3  |    ID           1       2       3       1       2       3                       */
    /*  YEAR 2010 2010 2010 2011 2011 2011  |    YEAR      2010    2010    2010    2011    2011    2011                       */
    /*  X       5    6    7    8    9   10  |    X            5       6       7       8       9      10                       */
    /*                                      |                                                                                 */
    /**************************************************************************************************************************/

    /*____                   _                                    _
    |___ /   ___  __ _ ___  / |  _ __  _ __ ___   ___   ___  __ _| |
      |_ \  / __|/ _` / __| | | | `_ \| `__/ _ \ / __| / __|/ _` | |
     ___) | \__ \ (_| \__ \ | | | |_) | | | (_) | (__  \__ \ (_| | |
    |____/  |___/\__,_|___/ |_| | .__/|_|  \___/ \___| |___/\__, |_|
                                |_|                            |_|
    */

    /*--- This looks lenthy but all you need to do to add year 2012
          perhaps this is the easiest to understand

    l.id  as val7
    c.id  as val8
    r.id  as val9

    l.X_2012  as val7
    c.X_2012  as val8
    r.X_2012  as val9

    ,2011  as val7
    ,2011  as val8
    ,2011  as val9

    ----*/

    proc sql;
       create
          table havXpo as
       select
          'ID' as var
          ,l.id  as val1
          ,c.id  as val3
          ,r.id  as val5
          ,l.id  as val2
          ,c.id  as val4
          ,r.id  as val6
       from
           sd1.have as l
            left join sd1.have as c on  1 = (c.id - 1)
            left join sd1.have as r on  1 = (r.id - 2)
       where
            l.id=1
       union
          all
       select
          "YEAR" as var
          ,2010  as val1
          ,2010  as val2
          ,2010  as val3
          ,2011  as val4
          ,2011  as val5
          ,2011  as val6
       from
           sd1.have as l
            left join sd1.have as c on  1 = (c.id - 1)
            left join sd1.have as r on  1 = (r.id - 2)
       where
            l.id=1
       union
          all
       select
          'X' as var
          ,l.X_2010  as val1
          ,c.X_2010  as val2
          ,r.X_2010  as val3
          ,l.X_2011  as val4
          ,c.X_2011  as val5
          ,r.X_2011  as val6
       from
           sd1.have as l
            left join sd1.have as c on  1 = (c.id - 1)
            left join sd1.have as r on  1 = (r.id - 2)
       where
            l.id=1
     ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   VAR     VAL1    VAL3    VAL5    VAL2    VAL4    VAL6                                                                 */
    /*                                                                                                                        */
    /*   ID         1       2       3       1       2       3                                                                 */
    /*   YEAR    2010    2010    2010    2011    2011    2011                                                                 */
    /*   X          5       6       7       8       9      10                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                               _
    | || |    ___  __ _ ___   ___  __ _| |   __ _ _ __ _ __ __ _ _   _ ___
    | || |_  / __|/ _` / __| / __|/ _` | |  / _` | `__| `__/ _` | | | / __|
    |__   _| \__ \ (_| \__ \ \__ \ (_| | | | (_| | |  | | | (_| | |_| \__ \
       |_|   |___/\__,_|___/ |___/\__, |_|  \__,_|_|  |_|  \__,_|\__, |___/
                                     |_|                         |___/
    */

    proc sql;
      create
         table stack as
      select
         id
        ,x_2010 as x
        ,2010 as year
      from
         sd1.have
      union
         all
      select
         id
         ,x_2011 as x
         ,2011 as year
      from
         sd1.have
          )
    ;quit;

    /*---
       ID     X    YEAR

        1     5    2010
        2     6    2010
        3     7    2010
        1     8    2011
        2     9    2011
        3    10    2011
    ---*/

    %array(cid,values=1-6);
    %array(ids,values=1 2 3 1 2 3);
    %array(yrs,values=2010 2010 2010 2011 2011 2011);

    %put &=ids3;

    proc sql;

     create
         table want as
     select
        'X' as grp,
         %do_over(ids cid yrs,phrase=
         %str(
           max(case
           when year=?yrs and id =?ids then x end) as col?cid)
             ,between=comma)
     from
          stack
     union
          all
     select
        'ID' as grp,
         %do_over(ids cid yrs,phrase=
         %str(
           max(case
           when year=?yrs and id =?ids then id end) as col?cid)
             ,between=comma)
     from
          stack
     union
          all
     select
        'YEAR' as grp,
         %do_over(ids cid yrs,phrase=
         %str(
           max(case
           when year=?yrs and id =?ids then year end) as col?cid)
             ,between=comma)
     from
          stack
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  GRP     COL1    COL2    COL3    COL4    COL5    COL6                                                                  */
    /*                                                                                                                        */
    /*  X          5       6       7       8       9      10                                                                  */
    /*  ID         1       2       3       1       2       3                                                                  */
    /*  YEAR    2010    2010    2010    2011    2011    2011                                                                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                     _
    | ___|   _ __   ___  __ _| |
    |___ \  | `__| / __|/ _` | |
     ___) | | |    \__ \ (_| | |
    |____/  |_|    |___/\__, |_|
                           |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(sqldf)
    library(haven)
    have<-read_sas("d:/sd1/have.sas7bdat")
    want<-sqldf("
       select
          'ID' as var
          ,l.id  as val1
          ,l.id  as val2
          ,c.id  as val3
          ,c.id  as val4
          ,r.id  as val5
          ,r.id  as val6
       from
           have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
       union
          all
       select
          'YEAR' as var
          ,2010  as val1
          ,2010  as val2
          ,2010  as val3
          ,2011  as val4
          ,2011  as val5
          ,2011  as val6
       from
           have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
       union
          all
       select
          'X' as var
          ,l.X_2010  as val1
          ,c.X_2010  as val2
          ,r.X_2010  as val3
          ,l.X_2011  as val4
          ,c.X_2011  as val5
          ,r.X_2011  as val6
       from
           have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
       ")
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;


    proc print data=sd1.want;
    run;quit;


    /**************************************************************************************************************************/
    /*                                    |                                                                                   */
    /*  var val1 val2 val3 val4 val5 val6 |  ROWNAMES      V1      V2      V3      V4      V5      V6                         */
    /*                                    |                                                                                   */
    /*   ID    1    1    2    2    3    3 |    ID           1       2       3       1       2       3                         */
    /* YEAR 2010 2010 2010 2011 2011 2011 |    YEAR      2010    2010    2010    2011    2011    2011                         */
    /*    X    5    6    7    8    9   10 |    X            5       6       7       8       9      10                         */
    /*                                    |                                                                                   */
    /**************************************************************************************************************************/

    /*__                 _   _                             _
     / /_    _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
    | `_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
    | (_) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
     \___/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    print(have)
    want=pdsql("""
       select                                          \
          'ID' as var                                  \
          ,l.id  as val1                               \
          ,l.id  as val2                               \
          ,c.id  as val3                               \
          ,c.id  as val4                               \
          ,r.id  as val5                               \
          ,r.id  as val6                               \
       from                                            \
          have as l                                    \
            left join have as c on  1 = (c.id - 1)     \
            left join have as r on  1 = (r.id - 2)     \
       where                                           \
            l.id=1                                     \
       union                                           \
          all                                          \
       select                                          \
          'YEAR' as var                                \
          ,2010  as val1                               \
          ,2010  as val2                               \
          ,2010  as val3                               \
          ,2011  as val4                               \
          ,2011  as val5                               \
          ,2011  as val6                               \
       from                                            \
           have as l                                   \
            left join have as c on  1 = (c.id - 1)     \
            left join have as r on  1 = (r.id - 2)     \
       where                                           \
            l.id=1                                     \
       union                                           \
          all                                          \
       select                                          \
          'X' as var                                   \
          ,l.X_2010  as val1                           \
          ,c.X_2010  as val2                           \
          ,r.X_2010  as val3                           \
          ,l.X_2011  as val4                           \
          ,c.X_2011  as val5                           \
          ,r.X_2011  as val6                           \
       from                                            \
           have as l                                   \
            left join have as c on  1 = (c.id - 1)     \
            left join have as r on  1 = (r.id - 2)     \
       where                                           \
            l.id=1
       """)
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                           |                                                            */
    /*      var    val1    val2    val3    val4    val5    val6  |  VAR     VAL1    VAL2    VAL3    VAL4    VAL5    VAL6      */
    /*                                                           |                                                            */
    /*  0    ID     1.0     1.0     2.0     2.0     3.0     3.0  |  ID         1       1       2       2       3       3      */
    /*  1  YEAR  2010.0  2010.0  2010.0  2011.0  2011.0  2011.0  |  YEAR    2010    2010    2010    2011    2011    2011      */
    /*  2     X     5.0     6.0     7.0     8.0     9.0    10.0  |  X          5       6       7       8       9      10      */
    /*                                                           |                                                            */
    /**************************************************************************************************************************/

    /*____                          __                     _
    |___  |  _ __   ___  ___  __ _ / _|_ __ ___  ___  __ _| |
       / /  | `_ \ / _ \/ __|/ _` | |_| `__/ _ \/ __|/ _` | |
      / /   | |_) | (_) \__ \ (_| |  _| | |  __/\__ \ (_| | |
     /_/    | .__/ \___/|___/\__, |_| |_|  \___||___/\__, |_|
            |_|              |___/                      |_|
                         _             _ _           _        _
      ___ _ __ ___  __ _| |_ ___    __| | |__   __  _| |_ __ _| |__
     / __| `__/ _ \/ _` | __/ _ \  / _` | `_ \  \ \/ / __/ _` | `_ \
    | (__| | |  __/ (_| | ||  __/ | (_| | |_) |  >  <| || (_| | |_) |
     \___|_|  \___|\__,_|\__\___|  \__,_|_.__/  /_/\_\\__\__,_|_.__/

    */

    %utl_rbeginx;
    parmcards4;
    # Load the necessary libraries
    library(RPostgres)
    library(DBI)
    library(haven)
    con <- dbConnect(RPostgres::Postgres(),
                dbname = "postgres",  # Use the default 'postgres' database
                host = "localhost",   # Replace with your PostgreSQL server address if not local
                port = 5432,          # Default PostgreSQL port
                user = "postgres",
                password = "12345678")
    dbExecute(con, "CREATE DATABASE xtab")
    dbDisconnect(con)
    ;;;;
    %utl_rendx;

    /*                   _                  _
      ___ _ __ ___  __ _| |_ ___   ___  ___| |__   ___ _ __ ___   __ _   ___ _ __ ___  ___ ___
     / __| `__/ _ \/ _` | __/ _ \ / __|/ __| `_ \ / _ \ `_ ` _ \ / _` | / __| `__/ _ \/ __/ __|
    | (__| | |  __/ (_| | ||  __/ \__ \ (__| | | |  __/ | | | | | (_| || (__| | | (_) \__ \__ \
     \___|_|  \___|\__,_|\__\___| |___/\___|_| |_|\___|_| |_| |_|\__,_| \___|_|  \___/|___/___/

    */

    %utl_rbeginx;
    parmcards4;
    # Load the necessary libraries
    library(RPostgres)
    library(DBI)
    library(haven)
    con <- dbConnect(RPostgres::Postgres(),
                dbname = "devel",
                host = "localhost",
                port = 5432,
                user = "postgres",
                password = "12345678")
    dbExecute(con, "CREATE SCHEMA prod")
    dbDisconnect(con)
    ;;;;
    %utl_rendx;

    /*__ _             _
     / _(_)_ __   __ _| |  _ __  _ __ ___   ___ ___  ___ ___
    | |_| | `_ \ / _` | | | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    |  _| | | | | (_| | | | |_) | | | (_) | (_|  __/\__ \__ \
    |_| |_|_| |_|\__,_|_| | .__/|_|  \___/ \___\___||___/___/
                          |_|
    */


    /*---- postgresql requires lover case column names ----*/
    options
     validvarname=v7;
    libname sd1 "d:/sd1";
    data sd1.have;
    input id x_2010 x_2011;
    cards4;
    1 5 8
    2 6 9
    3 7 10
    ;;;;
    run;quit;


    %utl_rbeginx;
    parmcards4;
    # Load the necessary libraries
    library(RPostgres)
    library(DBI)
    library(haven)
    source("c:/oto/fn_tosas9x.R");
    have<-read_sas("d:/sd1/have.sas7bdat")

    con <- dbConnect(RPostgres::Postgres(),
                dbname = "devel",  # Use the default 'postgres' database
                host = "localhost",   # Replace with your PostgreSQL server address if not local
                port = 5432,          # Default PostgreSQL port
                user = "postgres",
                password = "12345678")
    dbExecute(con, "SET search_path TO prod")
    dbWriteTable(con, "have", have, row.names = FALSE, overwrite = TRUE)

    query <- "
       select
          'ID' as var
          ,l.id  as val1
          ,l.id  as val2
          ,c.id  as val3
          ,c.id  as val4
          ,r.id  as val5
          ,r.id  as val6
       from
          have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
       union
          all
       select
          'YEAR' as var
          ,2010  as val1
          ,2010  as val2
          ,2010  as val3
          ,2011  as val4
          ,2011  as val5
          ,2011  as val6
       from
           have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
       union
          all
       select
          'X' as var
          ,l.X_2010  as val1
          ,c.X_2010  as val2
          ,r.X_2010  as val3
          ,l.X_2011  as val4
          ,c.X_2011  as val5
          ,r.X_2011  as val6
       from
           have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
    "
    df <- dbGetQuery(con, query)
    print(df)
    dbListObjects(con, Id(schema = 'cross'))
    dbDisconnect(con)
    df;
    fn_tosas9x(
          inp    = df
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         );
    ');
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                    |                                                                                   */
    /*  var val1 val2 val3 val4 val5 val6 |  ROWNAMES      V1      V2      V3      V4      V5      V6                         */
    /*                                    |                                                                                   */
    /*   ID    1    1    2    2    3    3 |    ID           1       2       3       1       2       3                         */
    /* YEAR 2010 2010 2010 2011 2011 2011 |    YEAR      2010    2010    2010    2011    2011    2011                         */
    /*    X    5    6    7    8    9   10 |    X            5       6       7       8       9      10                         */
    /*                                    |                                                                                   */
    /**************************************************************************************************************************/

    /*___                      _             _
     ( _ )    _____  _____ ___| |  ___  __ _| |
     / _ \   / _ \ \/ / __/ _ \ | / __|/ _` | |
    | (_) | |  __/>  < (_|  __/ | \__ \ (_| | |
     \___/   \___/_/\_\___\___|_| |___/\__, |_|
    /*                   _                |_|         _    _                 _
      ___ _ __ ___  __ _| |_ ___  __      _____  _ __| | _| |__   ___   ___ | | __
     / __| `__/ _ \/ _` | __/ _ \ \ \ /\ / / _ \| `__| |/ / `_ \ / _ \ / _ \| |/ /
    | (__| | |  __/ (_| | ||  __/  \ V  V / (_) | |  |   <| |_) | (_) | (_) |   <
     \___|_|  \___|\__,_|\__\___|   \_/\_/ \___/|_|  |_|\_\_.__/ \___/ \___/|_|\_\

    */
    options
     validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input iD X_2010 X_2011;
    cards4;
    1 5 8
    2 6 9
    3 7 10
    ;;;;
    run;quit;

    %utlfkil(d:/xls/wantx.xlsx);

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    library(haven)
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    wb <- createWorkbook()
    addWorksheet(wb, "have")
    writeData(wb, sheet = "have", x = have)
    saveWorkbook(
        wb
       ,"d:/xls/wantx.xlsx"
       ,overwrite=TRUE)
    ;;;;
    %utl_rendx;

    /*                          _
    __  _____  __ _   ___  __ _| |
    \ \/ / _ \/ _` | / __|/ _` | |
     >  <  __/ (_| | \__ \ (_| | |
    /_/\_\___|\__, | |___/\__, |_|
                 |_|         |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
     wb<-loadWorkbook("d:/xls/wantx.xlsx")
     have<-read.xlsx(wb,"have")
     addWorksheet(wb, "want")
     want<-sqldf("
       select
          'ID' as var
          ,l.id  as val1
          ,l.id  as val2
          ,c.id  as val3
          ,c.id  as val4
          ,r.id  as val5
          ,r.id  as val6
       from
           have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
       union
          all
       select
          'YEAR' as var
          ,2010  as val1
          ,2010  as val2
          ,2010  as val3
          ,2011  as val4
          ,2011  as val5
          ,2011  as val6
       from
           have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
       union
          all
       select
          'X' as var
          ,l.X_2010  as val1
          ,c.X_2010  as val2
          ,r.X_2010  as val3
          ,l.X_2011  as val4
          ,c.X_2011  as val5
          ,r.X_2011  as val6
       from
           have as l
            left join have as c on  1 = (c.id - 1)
            left join have as r on  1 = (r.id - 2)
       where
            l.id=1
       ")
     print(want)
     writeData(wb,sheet="want",x=want)
     saveWorkbook(
         wb
        ,"d:/xls/wantx.xlsx"
        ,overwrite=TRUE)
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* -----------------------+                                                                                               */
    /* | A1  | fx    |ROWNAME |                                                                                               */
    /* ------------------------------------------------------------------------------------+                                  */
    /* [_]|    A     |    B    |    C    |    E    |    F    |    G    |    H    |    I    |                                  */
    /* ------------------------------------------------------------------------------------|                                  */
    /*  1 |ROWNAME   |   VAR   |   VAL1  |  VAL2   |  VAL3   |  VAL4   |  VAL5   |   VAL6  |                                  */
    /* -- |----------+---------+---------+---------+---------+---------+---------+---------|                                  */
    /*  2 | 1        |ID       |1        |1        |2        |2        |3        |3        |                                  */
    /* -- |----------+---------+---------+---------+---------+---------+---------+---------|                                  */
    /*  3 | 2        |YEAR     |2010     |2010     |2010     |2011     |2011     |2011     |                                  */
    /* -- |----------+---------+---------+---------+---------+---------+---------+---------|                                  */
    /*  4 | 3        |X        |5        |6        |7        |8        |9        |10       |                                  */
    /* -- |----------+---------+---------+---------+---------+---------+---------+---------|                                  */
    /* [WANT}                                                                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
