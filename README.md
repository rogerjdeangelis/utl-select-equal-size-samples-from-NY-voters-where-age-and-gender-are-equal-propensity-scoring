# utl-select-equal-size-samples-from-NY-voters-where-age-and-gender-are-equal-propensity-scoring
Select equal size samples from NY voters where age and gender are equal propensity scoring
    %let pgm=utl-select-equal-size-samples-from-NY-voters-where-age-and-gender-are-equal-propensity-scoring;

    Select equal size samples from NY voters where age and gender are equal propensity scoring

    We want to adjust the urban vote count for rural
    voters with the same age and female to male ratio
    as rural voters

    For input see votrs.csv in theis repo 

    github
    http://tinyurl.com/yc5w9t4d
    https://github.com/rogerjdeangelis/utl-select-equal-size-samples-from-NY-voters-where-age-and-gender-are-equal-propensity-scoring

    related repo                                                                            
    https://github.com/rogerjdeangelis/utl_propensity_for_students_to_be_in_the_same_classes
    
    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                |                                                       */
    /*                        INPUT                                   |       OUTPUT PROPENSITY SCORING 250 SUB SAMPLE        */
    /*                                                                |                                                       */
    /*   ALL NEW YORK VOTERS 1,250 VOTERS                             | We want to adjust the urban vote count for rural      */
    /*                                                                | voters with the same age and female to male ratio     */
    /*                                                                | as rural voters                                       */
    /*                                                                |                                                       */
    /*  Ratio of females to males is 30:70 in Rural and 50:50 in Urban| Where urban demographics match rural,Biden still wins */
    /*  We need Urban Ratio to to the same as Rural (30:70)           |                                                       */
    /*                                                                |                           VOTES MATCHED DEMOGRAPHICS  */
    /*  SEX       SAMPLE   Percent                                    |                           ==========================  */
    /*  ----------------------------                                  |                  CNT   CNT       CNT   CNT            */
    /*                                                                | SAMPLE AVG(AGE) FEMALE MALE     BIDEN TRUMP           */
    /*  Female    URBAN     50.00   Females to males 50:50            |                                                       */
    /*  Male      URBAN     50.00                                     | RURAL   53.712    77   173       152    98            */
    /*                                                                | URBAN   53.660    77   173       139   111            */
    /*  Female    RURAL     30.80   Females to males 30:70            |                                                       */
    /*  Male      RURAL     69.20                                     |                                                       */
    /*                                                                | Ratio of Female/Male is the same in Urban and Rura    */
    /*  Need Urban age of 53.7                                        |                                                       */
    /*                                                                |                                                       */
    /*                N                                               |   SEX       SAMPLE    Frequency     Percent           */
    /*      SAMPLE  Obs     Mean                                      |   -------------------------------------------         */
    /*  ------------------------                                      |   Female    URBAN           77       30.80            */
    /*  AGE RURAL   250   53.712    Rural Mean age is 54.7            |   Male      URBAN          173       69.20            */
    /*  AGE URBAN  1000   48.922    Urban Mean age is 48.9            |                                                       */
    /*  ------------------------                                      |   Female    RURAL           77       30.80            */
    /*                                                                |   Male      URBAN          173       69.20            */
    /*                                                                |                                                       */
    /*                                                                |                                                       */
    /*                                                                |   Mean Age is the same in Urban and Rural             */
    /*                                                                |                                                       */
    /*                                                                |   SAMPLE         Obs        Mean                      */
    /*                                                                |   ------------------------------                      */
    /*                                                                |   AGE  RURAL     250  53.7120000                      */
    /*                                                                |   AGE  URBAN     250  53.6600000                      */
    /*                                                                |   ------------------------------                      */
    /*                                                                |                                                       */
    /*                                                                |                                                       */
    /*                                                                |  Where urban demographics match rural,Biden still wins*/
    /*                                                                |                                                       */
    /*                                                                |                    CNT   CNT   CNT   CNT              */
    /*                                                                |   SAMPLE AVG(AGE) FEMALE MALE BIDEN TRUMP             */
    /*                                                                |                                                       */
    /*                                                                |   RURAL   53.712    77   173   152    98              */
    /*                                                                |   URBAN   53.660    77   173   139   111              */
    /*                                                                |                                                       */
    /*                                                                |                                                       */
    /*                                                                |                                                       */
    /*                                                                |                                                       */
    /*                                                                |                                                       */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    /**************************************************************************************************************************/*/
    /*                                                                                                                        */*/
    /* d:/csv/voters.csv   28,672 bytes  1,250 records                                                                        */*/
    /*                                                                                                                        */*/
    /* SAMPLE,GROUP,SEX,AGE,SAT                                                                                               */*/
    /* RURAL,1,Male,35,41                                                                                                     */
    /* RURAL,1,Male,60,20                                                                                                     */
    /* RURAL,1,Male,59,19                                                                                                     */
    /* RURAL,1,Male,60,0                                                                                                      */
    /* RURAL,1,Male,72,23                                                                                                     */
    /* RURAL,1,Male,61,12                                                                                                     */
    /* RURAL,1,Male,30,19                                                                                                     */
    /* RURAL,1,Male,41,6                                                                                                      */
    /* RURAL,1,Male,62,34                                                                                                     */
    /* RURAL,1,Male,55,33                                                                                                     */
    /* RURAL,1,Female,63,35                                                                                                   */
    /* RURAL,1,Male,56,0                                                                                                      */
    /* RURAL,1,Male,43,24                                                                                                     */
    /* ...                                                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %utl_submit_r64x('
    library(dplyr);
    library(ggplot2);
    library(MatchIt);
    library(haven);
    library(sqldf);
    unlink("d:/rds/voters.rds");
    mydata<-read.csv("d:/csv/voters.csv");
    head(mydata);
    matchem <- matchit(GROUP ~ AGE + SEX, data = mydata, method="nearest", ratio=1);
    summary(matchem);
    match <- match.data(matchem)[1:ncol(mydata)];
    votematch=sqldf("
      select
         sample
        ,avg(age)
        ,sum(sex=\"Female\") as cnt_female
        ,sum(sex=\"Male\")   as cnt_male
        ,sum(candidate=\"BIDEN\") as cnt_biden
        ,sum(candidate=\"TRUMP\")   as cnt_trump
      from
        match
      group
        by sample
    ");
    votematch;
    saveRDS(votematch,"d:/rds/votematch.rds");
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* 250 sub matched subsample is stored in a R rdw file                                                                    */
    /*                                                                                                                        */
    /* d:/rds/votematch.rds                                                                                                   */
    /*                                                                                                                        */
    /*   SAMPLE avg(age) cnt_female cnt_male cnt_biden cnt_trump                                                              */
    /* 1  RURAL   53.712         77      173       152        98                                                              */
    /* 2  URBAN   53.660         77      173       139       111                                                              */
    /*                                                                                                                        */
    /* Call:                                                                                                                  */
    /* matchit(formula = GROUP ~ AGE + SEX, data = mydata, method = "nearest",                                                */
    /*     ratio = 1)                                                                                                         */
    /*                                                                                                                        */
    /* Summary of Balance for All Data:                                                                                       */
    /*           Means Treated Means Control Std. Mean Diff. Var. Ratio eCDF Mean                                             */
    /* distance         0.2276        0.1931          0.4837     0.9161    0.1356                                             */
    /* AGE             53.7120       48.9220          0.3451     0.5709    0.0821                                             */
    /* SEXFemale        0.3080        0.5000         -0.4159          .    0.1920                                             */
    /* SEXMale          0.6920        0.5000          0.4159          .    0.1920                                             */
    /*           eCDF Max                                                                                                     */
    /* distance     0.289                                                                                                     */
    /* AGE          0.201                                                                                                     */
    /* SEXFemale    0.192                                                                                                     */
    /* SEXMale      0.192                                                                                                     */
    /*                                                                                                                        */
    /* Summary of Balance for Matched Data:                                                                                   */
    /*           Means Treated Means Control Std. Mean Diff. Var. Ratio eCDF Mean                                             */
    /* distance         0.2276        0.2274          0.0019     1.0015    0.0005                                             */
    /* AGE             53.7120       53.6600          0.0037     1.0004    0.0010                                             */
    /* SEXFemale        0.3080        0.3080          0.0000          .    0.0000                                             */
    /* SEXMale          0.6920        0.6920          0.0000          .    0.0000                                             */
    /*           eCDF Max Std. Pair Dist.                                                                                     */
    /* distance     0.012          0.0021                                                                                     */
    /* AGE          0.020          0.0043                                                                                     */
    /* SEXFemale    0.000          0.0000                                                                                     */
    /* SEXMale      0.000          0.0000                                                                                     */
    /*                                                                                                                        */
    /* Sample Sizes:                                                                                                          */
    /*           Control Treated                                                                                              */
    /* All          1000     250                                                                                              */
    /* Matched       250     250                                                                                              */
    /* Unmatched     750       0                                                                                              */
    /* Discarded       0       0                                                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*    _               _                 _                       _       _              _
      ___| |__   ___  ___| | __ __   _____ | |_ ___ _ __ ___   __ _| |_ ___| |__   _ __ __| |___
     / __| `_ \ / _ \/ __| |/ / \ \ / / _ \| __/ _ \ `_ ` _ \ / _` | __/ __| `_ \ | `__/ _` / __|
    | (__| | | |  __/ (__|   <   \ V / (_) | ||  __/ | | | | | (_| | || (__| | | || | | (_| \__ \
     \___|_| |_|\___|\___|_|\_\   \_/ \___/ \__\___|_| |_| |_|\__,_|\__\___|_| |_||_|  \__,_|___/

    */

    %utl_submit_r64x('
    readRDS("d:/rds/votematch.rds");
    votematch;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* > readRDS("d:/rds/votematch.rds");votematch;                                                                           */
    /*   SAMPLE avg(age) cnt_female cnt_male cnt_biden cnt_trump                                                              */
    /* 1  RURAL   53.712         77      173       152        98                                                              */
    /* 2  URBAN   53.660         77      173       139       111                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
