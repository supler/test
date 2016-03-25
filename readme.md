# CTP - CUBRID Test Program

## Introduction
CTP as testing tool for an open source project CUBRID, which is developed based on Java language, and it integrates functional testing and result showing into one suite. It is easy to execute testing with a simple configuration. 

## Requirements
* Java 1.6 or higher, and configure Java environment variable "export JAVA_HOME=$java_installation_path".
* Install CUBRID build, and make sure the related environment variable of CUBRID is configured correctly.

## Quick start
* Execute sample testing with the below commands:

    ``` 

        sh bin/ctp.sh sql -c conf/sample.conf
    
     ```
 
* Once the testing is finished, and then to start webconsole to see result
    
    ```

    sh bin/ctp.sh webconsole start
    
    ```
    
* You will see the information as below
    
    ```
    $ sh bin/ctp.sh webconsole start
	Config: /home/user/CTP/conf/webconsole.conf
	Web Root: /home/user/CTP/sql/webconsole
	Begin to start ...

	Done
	URL:  http://127.0.0.1:8888    
	  
    ```
	
* Copy URL into browser, and you will see the result list

## How to run

Using CTP involves steps:

#### Prepare
* Checkout scenario from github to local.
* Install CUBRID build which will be used to execute testing, and make sure your environment variables of CUBRID is configured correctly($CUBRID environment will be used within CTP).
* Configure CTP conf files
  * If you are executing SQL testing, you can modify parameters within conf/sql.conf according to your requirement. 
    * scenario in [sql] section will be used to configure where the path of scenario tested is.
    * config_file in [sql] section will be used to the client charset, connection parameters(auto_commit=true/false), you can find this file from sql/configuration/test_config to reference or change
    * db_charset in [sql] section will be used to set database charset during db creation.
    * test_mode=yes and java_stored_procedure=yes must be configured during executing SQL testing, you can read the comment in sql.conf for the intention.
    * other parameters in conf/sql.conf are in order to avoid ports conflict or improve the speed of testing execution, so they can be configured as optional   
    
  * If you are executing Medium testing, you can modify parameters within conf/medium.conf according to your requirement. 
    * scenario in [sql] section will be used to configure where the path of scenario tested is.
    * config_file in [sql] section will be used to the client charset, connection parameters(auto_commit=true/false), you can find this file from sql/configuration/test_config to reference or change
    * db_charset in [sql] section will be used to set database charset during db creation.
    * data_file in [sql] section will be used to the path of medium data file, since Medium testing will be executed based on an existing databases, so you must configure it.
    * test_mode=yes and java_stored_procedure=yes must be configured during executing SQL testing, you can read the comment in sql.conf for the intention.
    * other parameters in conf/sql.conf are in order to avoid ports conflict or improve the speed of testing execution, so they can be configured as optional 

#### Run Tests
* For SQL/Medium
  * sh bin/ctp.sh sql -c conf/sql.conf
  * sh bin/ctp.sh medium -c conf/medium.conf
  
#### Review Results
* Once testing is complete, result directory will be printed.

    ```
	-----------------------
	Fail:0
	Success:1
	Total:1
	Elapse Time:193
	Test Result Directory:/home/user/CTP/sql/result/y2016/m3/schedule_linux_sql_64bit_24202122_10.0.0_1376
	Test Log:/home/user/CTP/sql/log/sql_10.0.0.1376_1458818452.log
	-----------------------
	
	-----------------------
	Testing End!
	-----------------------
    ```
* Go to Test Result Directory, and find the detailed results
* If you want to check result from web browser, you can start webconsole service from CTP
  * ``sh bin/ctp.sh webconsole start``, the URL of results will be printed as the below

    ```
          Config: /home/user/CTP/conf/webconsole.conf
          Web Root: /home/user/CTP/sql/webconsole
          Begin to start ...
          
          Done
          URL:  http://127.0.0.1:8887
```
  * open URL and click the related result for details
  
        
## How to build

In most cases, you don't need to build CTP from source, unless you make some changes for source codes. If you want to build source, you need install ant first, and then to execute the following commands to build source codes.

```
   ant clean dist
```

You can find the final jars from directory CTP/sql/lib/cubridqa-cqt.jar.

             
## How to write test script
If you want to write test script based on CTP tool, you must follow the below rules, only that kind of script which meets the specification of CTP will be recognized and executed correctly：
* the extension of script file must be .sql, and the answer of script which will be used to examine success or fail must be end with .answer
* the structure of script must same as the below

    ```
	 _08_primary_foreign_key
	                       /cases
	                             /int_primary_key_test.sql
	                       /answers
	                             /int_primary_key_test.answer
    ```
  * the keyword "cases" and "answers" must be included
  * the root name of script(we suggest you give a meaningful root name, it will help reader to understand the intent of your script)
  * when you write one new script, answer file may be difficult to generate it, but you don't need worry about it, you just need touch one blank answer file which has same name as case's, but the extension of answer file must be ".answer", and then to execute
           your script by using CTP tool,CTP tool will generate the related result file in cases folder which has same name as case's, but the extension of result should be ".result". And now you can examine the result contents are expected or not, once all contents 
           are confirmed, you can just copy result file contents to answer file as the expected results for future reference in testing.   
  * CTP supports all SQL statements which are supported by CUBRID, but I would like to clarify for some special syntax.
  * prepare statement, you just need write script as the below format, and CTP will do value binding 
           
    ```
           prepare st from 'select trunc(?,?)'
           execute st using date'2001-10-11',1;
           deallocate prepare st;
    ```
           
  * user defined variables
           
    ```
           set @v1=1;
           select @v1;
           drop variable @v1;
    ```

  * for I18N scenario, you can set charset into script. If you want to run all cases with the different database charset and client charset, you just need configure the conf file you used for db_charset and client charset file under CTP/conf/,
             the value of db_charset will be used to db creation, and client charset files are based on CTP/sql/configuration/test_config/ directory, you can choose one that meets your required or create new one for your test.
           
    ```
           set names iso88591 collate iso88591_en_ci;
           execute st1 using 'helloAaaAAaa', 'a', 2;
    ```

* for queryPlan, if you want to examine queryPlan from testing result, you will have two options to configure that, and once it is configured, the related SQL statement will print query plan data with results
  * touch one blank file name as case_name.queryPlan to save it into same directory with case, so the results of all queries statement will print query plan data.

    ```
	 _08_primary_foreign_key
	                       /cases
	                             /int_primary_key_test.sql
	                             /int_primary_key_test.queryPlan
	                       /answers
	                             /int_primary_key_test.answer
    ```

   * or add --@queryplan in the above statement, the following statement of flag will print query plan data. 
    
        ```
	--@queryplan
	select /*+ recompile */ median(a) from x;
	select /*+ recompile */ median(b) from x;
        ```

* for the transaction isolation level, you can set it in your case script as the below syntax

    ```
        SET TRANSACTION ISOLATION LEVEL 1;
        select /*+ recompile */ median(a) from x;
        select /*+ recompile */ median(b) from x;

        SET TRANSACTION ISOLATION LEVEL 2;
        select /*+ recompile */ median(a) from x;
        select /*+ recompile */ median(b) from x;

        SET TRANSACTION ISOLATION LEVEL 3;
        select /*+ recompile */ median(a) from x;
        select /*+ recompile */ median(b) from x;
    ```

   that will make the following statements are executed with the corresponding transaction isolation level.

## License
CTP is published under the BSD 3-Cause license. See LICENSE.md for more details.
Third-party libraries used by CTP are under their own licenses. See LICENSE-3RD-PARTY.md for details on the license they use.
