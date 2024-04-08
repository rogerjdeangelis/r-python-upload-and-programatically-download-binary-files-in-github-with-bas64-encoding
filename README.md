# r-python-upload-and-programatically-download-binary-files-in-github-with-bas64-encoding
R python upload and programatically download binary files in github with bas64 encoding
    %let pgm=utl-r-python-upload-and-programatically-download-binary-files-in-github-using-base64-encoding;

    R python upload and programatically download binary files in github with bas64 encoding

    It seems it is now very problematic to progranitically directly download single binary files from githib?
    I think you need a github token?
    Also I could not figure out how to programatically upload a file to github, so you have to do it manually.

    Quite easy to download text files, see related repos on end R doenload

    Two Parts
        1. python download the base64 encoded file from github and decode to mtcars.sas7bdat
        2. R and python create sas dataset convert to base6r ang manually upload to github

    See related repos on end

    * AUTOMATED DOWNLOAD AND DECODE

    github
    mtcars.b64
    https://tinyurl.com/e7etku5d
    https://raw.githubusercontent.com/rogerjdeangelis/r-python-upload-and-programatically-download-binary-files-in-github-with-bas64-encoding/main/mtcars.b64

    mtcars.sas7bdat
    https://tinyurl.com/55e3cbru
    https://github.com/rogerjdeangelis/utl-r-python-upload-and-programatically-download-binary-files-in-github-using-base64-encoding/blob/main/mtcars.sas7bdat


    /*****************************************************************************************************************************/
    /*                                                                                                                           */
    /* 1. python download the base64 encoded mtcars and decode to D:/sd1mtcarspyback.sas7bdat                                    */
    /*                                                                                                                           */
    /*---------------------------------------------------------------------------------------------------------------------------*/
    /*                              |                                                        |                                   */
    /*         INPUT                | PROCESS(Download & Convert Base64 file to sas dataset) | OUTPUT (sas dataset)              */
    /*                              |                                                        |                                   */
    /* guthub input mtcars.b64      | %utl_pybeg in;                                         | D:/sd1mtcarspyback.sas7bdat       */
    /* (we will use the tinyurl)    | parmcards4;                                            |                                   */
    /*                              | import pybase64                                        | AUTO           MPG CYL  HP    WT  */
    /* https://tinyurl.com/e7etku5d | import requests                                        |                                   */
    /*                              | url = 'https://tinyurl.com/e7etku5d'                   | Mazda RX4     21.0  6  110  2.620 */
    /*                              | response = requests.get(url)                           | Mazda RX4 Wgn 21.0  6  110  2.875 */
    /*                              | base64_data = response.text.strip()                    | Datsun 710    22.8  4   93  2.320 */
    /*                              | decoded_data = pybase64.b64decode(base64_data)         | Hornet 4      21.4  6  110  3.215 */
    /*                              | with open('d:/sd1/mtcarspyback.sas7bdat','wb') as file:| Valiant       18.1  6  105  3.460 */
    /*                              |     file.write(decoded_data)                           | Duster 360    14.3  8  245  3.570 */
    /*                              | ;;;;                                                   | Merc 240D     24.4  4   62  3.190 */
    /*                              | %utl_pyend;                                            | Merc 230      22.8  4   95  3.150 */
    /*                              |                                                        | Merc 280      19.2  6  123  3.440 */
    /*                              |                                                        | Merc 280C     17.8  6  123  3.440 */
    /*                              |                                                        | ....                              */
    /*                              |                                                        |                                   */
    /*---------------------------------------------------------------------------------------------------------------------------*/
    /*                                                                                                                           */
    /* 2. R and python create sas dataset converth to base6r anf manually upload to github                                       */
    /*                                                                                                                           */
    /*---------------------------------------------------------------------------------------------------------------------------*/
    /*                                                   |                                                    |                  */
    /* INPUT(create sas dataset for conversion to base64 |     PROCESS(conver to bas64)                       |OUTPT(for upload) */
    /*                                                   |                                                    |                  */
    /*  *                                                |                                                    |                  */
    /*  This will creates a  sas dataset without sas.    | * THIS CONVERTS MTCARS.SAS7BDAT TO BASE64 TEXT;    |Manually upload   */
    /*  You can use your own sas sample dataset instead  |                                                    |                  */
    /*  ;                                                | %utl_pybegin;                                      |d:/b64/mtcars.b64 */
    /*                                                   | parmcards4;                                        |                  */
    /*  %utlfkil(c:/temp/mtcars.sas7bdat);               | import pybase64                                    |                  */
    /*                                                   | data = open("c:/temp/mtcars.sas7bdat", "rb").read()|                  */
    /*  %utl_rbegin;                                     | encoded = pybase64.b64encode(data)                 |                  */
    /*  parmcards4;                                      | with open("d:/b64/mtcars.b64", "wb") as f:         |                  */
    /*  library(sqldf);                                  |     f.write(encoded)                               |                  */
    /*  source("c:/temp/fn_tosas9.R");                   | ;;;;                                               |                  */
    /*  data(mtcars);                                    | %utl_pyend;                                        |                  */
    /*  mtcars <- cbind(auto = rownames(mtcars),mtcars)  |                                                    |                  */
    /*  str(mtcars);                                     | * CONVERTED TO PRINTABLE TEXT;                     |                  */
    /*  mtcars <- sqldf('                                |                                                    |                  */
    /*     select                                        | D:/B64/MTCARS.B64                                  |                  */
    /*       AUTO                                        |                                                    |                  */
    /*      ,MPG                                         | --- RECORD NUMBER 1 --- RECORD LENGTH ---- 40      |                  */
    /*      ,CYL                                         |                                                    |                  */
    /*      ,HP                                          | 1...5....10...15...20...25...30...35...4           |                  */
    /*      ,WT                                          | AAAAAAAAAAAAAAAAwuqBYLMUEc+9kggACccxjBgf           |                  */
    /*    from                                           | 5474445446444444454632765644447444454434           |                  */
    /*       mtcars                                      | 553422112719111191331F9F8751147511141D82           |                  */
    /*    ');                                            |                                                    |                  */
    /*  fn_tosas9(dataf=mtcars);                         |                                                    |                  */
    /*  ;;;;                                             |                                                    |                  */
    /*  %utl_rend;                                       |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*  libname tmp "c:/temp";                           |                                                    |                  */
    /*  proc print data=tmp.mtcars;                      |                                                    |                  */
    /*  run;quit;                                        |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*  tmp.mtcars                                       |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*  AUTO           MPG CYL  HP    WT                 |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*  Mazda RX4     21.0  6  110  2.620                |                                                    |                  */
    /*  Mazda RX4 Wgn 21.0  6  110  2.875                |                                                    |                  */
    /*  Datsun 710    22.8  4   93  2.320                |                                                    |                  */
    /*  Hornet 4      21.4  6  110  3.215                |                                                    |                  */
    /*  Valiant       18.1  6  105  3.460                |                                                    |                  */
    /*  Duster 360    14.3  8  245  3.570                |                                                    |                  */
    /*  Merc 240D     24.4  4   62  3.190                |                                                    |                  */
    /*  Merc 230      22.8  4   95  3.150                |                                                    |                  */
    /*  Merc 280      19.2  6  123  3.440                |                                                    |                  */
    /*  Merc 280C     17.8  6  123  3.440                |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*  * BINARY C:/TEMP/MTCARS.SAS7BDAT 1ST 40 BYTES;   |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*  --- RECORD NUMBER 1 --- RECORD LENGTH ---- 40    |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*  1...5....10...15...20...25...30...35...4         |                                                    |                  */
    /*  PK???.?.?...!.œ×ü¨^?..<?..?.Ï?[Content_T         |                                                    |                  */
    /*  540010000000209DFA5000300010C05466766755         |                                                    |                  */
    /*  0B344060800010C7C8E100C40030F1B3FE45E4F4         |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*                                                   |                                                    |                  */
    /*******************************|********************************************************|************************************/

    /*               _   _                   _                     __   _  _    _                              _       _                 _
    / |  _ __  _   _| |_| |__   ___  _ __   | |__   __ _ ___  ___ / /_ | || |  | |_ ___    ___  __ _ ___    __| | __ _| |_ __ _ ___  ___| |_
    | | | `_ \| | | | __| `_ \ / _ \| `_ \  | `_ \ / _` / __|/ _ \ `_ \| || |_ | __/ _ \  / __|/ _` / __|  / _` |/ _` | __/ _` / __|/ _ \ __|
    | | | |_) | |_| | |_| | | | (_) | | | | | |_) | (_| \__ \  __/ (_) |__   _|| || (_) | \__ \ (_| \__ \ | (_| | (_| | || (_| \__ \  __/ |_
    |_| | .__/ \__, |\__|_| |_|\___/|_| |_| |_.__/ \__,_|___/\___|\___/   |_|   \__\___/  |___/\__,_|___/  \__,_|\__,_|\__\__,_|___/\___|\__|
        |_|    |___/
     _                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    guthub input mtcars.b64
    (we will use the tinyurl)

    file mtcars.b64 in github
    https://tinyurl.com/e7etku5d
    https://raw.githubusercontent.com/rogerjdeangelis/r-python-upload-and-programatically-download-binary-files-in-github-with-bas64-encoding/main/mtcars.b64

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    /*----  delete output dataset if it exists                               ----*/
    %utlfkil(d:/sd1/mtcarspyback.sas7bdat);

    %utl_pybegin;
    parmcards4;
    import pybase64
    import requests
    url = 'https://tinyurl.com/e7etku5d'
    response = requests.get(url)
    base64_data = response.text.strip()
    decoded_data = pybase64.b64decode(base64_data)
    with open('d:/sd1/mtcarspyback.sas7bdat','wb') as file:
        file.write(decoded_data)
    ;;;;
    %utl_pyend;

    libname sd1 "d:/sd1";
    proc print data=SD1.MTCARSPYBACK noobs;
    run;quit;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  d:/sd1/mtcarspyback.sas7bdat                                                                                          */
    /*                                                                                                                        */
    /* sd1.mtcarspyback total obs=32                                                                                          */
    /*                                                                                                                        */
    /* ROWNAMES    AUTO                    MPG    CYL     HP      WT                                                          */
    /*                                                                                                                        */
    /*     1       Mazda RX4              21.0     6     110    2.620                                                         */
    /*     2       Mazda RX4 Wag          21.0     6     110    2.875                                                         */
    /*     3       Datsun 710             22.8     4      93    2.320                                                         */
    /*     4       Hornet 4 Drive         21.4     6     110    3.215                                                         */
    /*     5       Hornet Sportabout      18.7     8     175    3.440                                                         */
    /*     6       Valiant                18.1     6     105    3.460                                                         */
    /*     7       Duster 360             14.3     8     245    3.570                                                         */
    /*     8       Merc 240D              24.4     4      62    3.190                                                         */
    /*     9       Merc 230               22.8     4      95    3.150                                                         */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___            ___                 _   _                             _          _                     __   _  _      __ _ _
    |___ \   _ __   ( _ )    _ __  _   _| |_| |__   ___  _ __    _ __ ___ | | _____  | |__   __ _ ___  ___ / /_ | || |    / _(_) | ___
      __) | | `__|  / _ \/\ | `_ \| | | | __| `_ \ / _ \| `_ \  | `_ ` _ \| |/ / _ \ | `_ \ / _` / __|/ _ \ `_ \| || |_  | |_| | |/ _ \
     / __/  | |    | (_>  < | |_) | |_| | |_| | | | (_) | | | | | | | | | |   <  __/ | |_) | (_| \__ \  __/ (_) |__   _| |  _| | |  __/
    |_____| |_|     \___/\/ | .__/ \__, |\__|_| |_|\___/|_| |_| |_| |_| |_|_|\_\___| |_.__/ \__,_|___/\___|\___/   |_|   |_| |_|_|\___|
     _                   _  |_|    |___/
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    NPUT(create sas dataset for conversion to base6

    *
    This will creates a  sas dataset without sas.
    YOU CAN USE YOUR OWN SAS SAMPLE DATASET INSTEAD
    ;

    %utlfkil(c:/temp/mtcars.sas7bdat);

    %utl_rbegin;
    parmcards4;
    library(sqldf);
    source("c:/temp/fn_tosas9.R");
    data(mtcars);
    mtcars <- cbind(auto = rownames(mtcars),mtcars)
    str(mtcars);
    mtcars <- sqldf('
       select
         AUTO
        ,MPG
        ,CYL
        ,HP
        ,WT
      from
         mtcars
      ');
    fn_tosas9(dataf=mtcars);
    ;;;;
    %utl_rend;

    libname tmp "c:/temp";
    proc print data=tmp.mtcars;
    run;quit;

    c:/temp/mtcars.sas7bdat
    tmp.mtcars

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* TMP.MTCARS total obs=32  c:/TEMP/MTCARS.SAS7BDAT                                                                       */
    /*                                                                                                                        */
    /* ROWNAMES    AUTO                    MPG    CYL     HP      WT                                                          */
    /*                                                                                                                        */
    /*     1       Mazda RX4              21.0     6     110    2.620                                                         */
    /*     2       Mazda RX4 Wag          21.0     6     110    2.875                                                         */
    /*     3       Datsun 710             22.8     4      93    2.320                                                         */
    /*     4       Hornet 4 Drive         21.4     6     110    3.215                                                         */
    /*     5       Hornet Sportabout      18.7     8     175    3.440                                                         */
    /*     6       Valiant                18.1     6     105    3.460                                                         */
    /*     7       Duster 360             14.3     8     245    3.570                                                         */
    /*     8       Merc 240D              24.4     4      62    3.190                                                         */
    /*     9       Merc 230               22.8     4      95    3.150                                                         */
    /*    10       Merc 280               19.2     6     123    3.440                                                         */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* * BINARY C:/TEMP/MTCARS.SAS7BDAT 1ST 40 BYTES;                                                                         */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* --- RECORD NUMBER 1 --- RECORD LENGTH ---- 40                                                                          */
    /*                                                                                                                        */
    /* 1...5....10...15...20...25...30...35...4                                                                               */
    /* PK???.?.?...!.œ×ü¨^?..<?..?.Ï?[Content_T                                                                               */
    /* 540010000000209DFA5000300010C05466766755                                                                               */
    /* 0B344060800010C7C8E100C40030F1B3FE45E4F4                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    /*----                                                                   ----*/
    /*----  input: c:/temp/mtcars.sas7bdat                                   ----*/
    /*----  output: d:/b64/mtcars.b64                                        ----*/
    /*----                                                                   ----*/
    input c:/temp/mtcars.sas7bdat

    %utl_pybegin;
    parmcards4;
    import pybase64
    data = open("c:/temp/mtcars.sas7bdat", "rb").read()
    encoded = pybase64.b64encode(data)
    with open("d:/b64/mtcars.b64", "wb") as f:
        f.write(encoded)
    ;;;;
    %utl_pyend;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /*----                                                                   ----*/
    /*----  C:/TEMP/MTCARS.SAS7BDAT CONVERTED TO PRINTABLE TEXT             ----*/
    /*----                                                                   ----*/

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* D:/B64/MTCARS.B64                                                                                                      */
    /*                                                                                                                        */
    /* --- RECORD NUMBER 1 --- RECORD LENGTH ---- 40                                                                          */
    /*                                                                                                                        */
    /* 1...5....10...15...20...25...30...35...4                                                                               */
    /* AAAAAAAAAAAAAAAAwuqBYLMUEc+9kggACccxjBgf                                                                               */
    /* 5474445446444444454632765644447444454434                                                                               */
    /* 553422112719111191331F9F8751147511141D82                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*        _       _           _
     _ __ ___| | __ _| |_ ___  __| |  _ __ ___ _ __   ___  ___
    | `__/ _ \ |/ _` | __/ _ \/ _` | | `__/ _ \ `_ \ / _ \/ __|
    | | |  __/ | (_| | ||  __/ (_| | | | |  __/ |_) | (_) \__ \
    |_|  \___|_|\__,_|\__\___|\__,_| |_|  \___| .__/ \___/|___/
                                              |_|
    */

    Related repos

    https://github.com/rogerjdeangelis/utl-python-base64-encode-and-decode-a-binary-execl-workbook-or-binary-file
    https://github.com/rogerjdeangelis/utl-sas-macros-to-encode-and-decode-binary-data-using-base64
    https://github.com/rogerjdeangelis/utl-zip-sas-table-encode-base64-decode-base64-unzip-all-programatically-base-sas-code

    https://github.com/rogerjdeangelis/utl-using-SAS-R-unix-commands-to-zip-and-unzip-files-on-windows-and-unix

    https://github.com/rogerjdeangelis/utl-using-cross-platform-R-or-python-to-zip-and-unzip-folders
    https://github.com/rogerjdeangelis/utl-zip-sas-table-encode-base64-decode-base64-unzip-all-programatically-base-sas-code
            uses SAS utl_bas64encode and utl_bas64dncode macros

    https://github.com/rogerjdeangelis/utl-gzip-windows-and-unix
    https://github.com/rogerjdeangelis/utl-maximum-zip-compression-for-a_13gb-sas-table
    https://github.com/rogerjdeangelis/utl-multi-platform-zip-a-file-into-multiple-pieces-then-combine-and-unzip
    https://github.com/rogerjdeangelis/utl-powershell-unzip-one-meber-of-a-winzip-archive
    https://github.com/rogerjdeangelis/utl-splitting-and-zipping-a-SAS-table-into-chunks-based-on-number-of-obs-then-unzipping-and-concate
    https://github.com/rogerjdeangelis/utl-unzipping-line-by-line-and-subsetting-without-unzipping-the-entire-file
    https://github.com/rogerjdeangelis/utl-using-SAS-R-unix-commands-to-zip-and-unzip-files-on-windows-and-unix
    https://github.com/rogerjdeangelis/utl-using-cross-platform-R-or-python-to-zip-and-unzip-folders
    https://github.com/rogerjdeangelis/utl-zip-and-unzip-a-folder-of-files-in-R-and-Python
    https://github.com/rogerjdeangelis/utl-zip-and-unzip-comparisons-using-bzip2-gzip_7-zip-and-sas-zip
    https://github.com/rogerjdeangelis/utl-zip-sas-table-encode-base64-decode-base64-unzip-all-programatically-base-sas-code
    https://github.com/rogerjdeangelis/utl_R_gzip_and_gunzip_for_cross_paltform_single_file_compression
    https://github.com/rogerjdeangelis/utl_SAS_code_to_list_3_records_from_each_file_in_a_zipped_archive
    https://github.com/rogerjdeangelis/utl_using_sas_zip_and_unzip_engines
    https://github.com/rogerjdeangelis/zip-and-unzip-using-ms-powershell

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
