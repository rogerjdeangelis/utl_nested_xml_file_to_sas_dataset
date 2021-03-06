# utl_nested_xml_file_to_sas_dataset
 Nested XML file to SAS dataset.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    SAS Forum: Nested XML file to SAS dataset

    SAS/IML/R or WPS/PROC R

      Two Solutions
         1. Output R list direcly
         2. Parse inside R

    The list data structure in Python. R and Perl make this possible?

    github
    https://github.com/rogerjdeangelis/utl_nested_xml_file_to_sas_dataset/tree/master

    see
    https://goo.gl/5WNLkL
    https://communities.sas.com/t5/Base-SAS-Programming/SAS-code-for-XML-file/m-p/428825


    INPUT
    =====

        "d:/xml/nhl.xml"

         <?xml version="1.0" encoding="iso-8859-1" ?>
         <NHL>
           <CONFERENCE> Eastern
             <DIVISION> Southeast
               <TEAM name="Thrashers"  abbrev="ATL" />
            </DIVISION>
          </CONFERENCE>
         </NHL>


    WORKING CODE  (WPS/PROC R)
    ===========================


      1. Output R list direcly

         lst<-xmlToList("d:/xml/nhl.xml");
         import r=lst data=wrk.wantlst;

      2. Parse inside R

         lst<-xmlToList("d:/xml/nhl.xml");  * load into an R list

         l1<-lst$CONFERENCE$text;           * extract from tree - could unlist to a file;
         l2<-lst$CONFERENCE$DIVISION$text;
         l3<-lst$CONFERENCE$DIVISION$TEAM;

         want<-as.data.frame(cbind(l1,l2,l3)); * create dataframe;
         import r=want data=wrk.wantlst;

    OUTPUT (two solutions)
    ======

     WORK.WANT total obs=2

                      CONFERENCE_    CONFERENCE_    CONFERENCE_
       CONFERENCE_     DIVISION_      DIVISION_      DIVISION_     CONFERENCE_
          TEXT           TEXT           TEAM          TEXT_1         TEXT_1

         Eastern       Southeast      Thrashers
         Eastern       Southeast      ATL


      WORK.WANT total obs=2

           L1          L2        L3

         Eastern    Southeast    Thrashers
         Eastern    Southeast    ATL

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data _null_;
    file "d:/xml/nhl.xml";
    input;
    put _infile_;
    cards4;
    <?xml version="1.0" encoding="iso-8859-1" ?>
    <NHL>
      <CONFERENCE> Eastern
        <DIVISION> Southeast
          <TEAM name="Thrashers"  abbrev="ATL" />
       </DIVISION>
     </CONFERENCE>
    </NHL>
    ;;;;
    run;quit;

    *_     _               _       _   _
    / |___| |_   ___  ___ | |_   _| |_(_) ___  _ __
    | / __| __| / __|/ _ \| | | | | __| |/ _ \| '_ \
    | \__ \ |_  \__ \ (_) | | |_| | |_| | (_) | | | |
    |_|___/\__| |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;


    %utl_submit_wps64('
    libname sd1 sas7bdat "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(rvest);
    library(tidyr);
    library(SASxport);
    require(XML);
    lst<-xmlToList("d:/xml/nhl.xml");
    endsubmit;
    import r=lst data=wrk.wantlst;
    run;quit;
    ');

    data want;
      set wantwps;
      format _character_ $18.;
    run;quit;

    proc print data=want width=min;
    run;quit;


    *____            _             _       _   _
    |___ \ _ __   __| |  ___  ___ | |_   _| |_(_) ___  _ __
      __) | '_ \ / _` | / __|/ _ \| | | | | __| |/ _ \| '_ \
     / __/| | | | (_| | \__ \ (_) | | |_| | |_| | (_) | | | |
    |_____|_| |_|\__,_| |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname sd1 sas7bdat "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(rvest);
    library(tidyr);
    library(SASxport);
    require(XML);
    lst<-xmlToList("d:/xml/nhl.xml");
    l1<-lst$CONFERENCE$text;
    l2<-lst$CONFERENCE$DIVISION$text;
    l3<-lst$CONFERENCE$DIVISION$TEAM;
    want<-as.data.frame(cbind(l1,l2,l3));
    endsubmit;
    import r=want data=wrk.wantwps;
    run;quit;
    ');

    data wantfix;
      length l1 l2 l3 $18.;
      set wantwps;
    run;quit;

