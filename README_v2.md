# CTP - CUBRID Test Program

## Introduction
CTP as testing tool for an open source project CUBRID, which is developed based on Java language, and it integrates functional testing, non-functional testing 
and result showing into one. it is easy to execute testing with a simple configuration. 

## Requirements
* Java 1.6 or higher, and configure Java environment variable "export JAVA_HOME=$java_installation_path".
* Install CUBRID build, and make sure the related environment variable of CUBRID is configured correctly.
* To get CTP program from github via command `git clone git://github.com/.../CTP/ctp.git $HOME/CTP`

## Quick start
* Execute sample testing with the below commands:

``` 

    cd $HOME/CTP
    sh bin/ctp.sh sql -c conf/sample.conf
    
 ```
 
* Once the testing is finished, and then to start webconsole to see result
    
```

    cd $HOME/CTP
    sh bin/ctp.sh webconsole start
    
```
    
* You will see the information as below
    
```
    [user@abcdefg CTP]$ sh bin/ctp.sh webconsole start
	Config: /home/user/CTP/conf/webconsole.conf
	Web Root: /home/user/CTP/sql/webconsole
	Begin to start ...

	Done
	URL:  http://127.0.0.1:8888    
	  
```
	
* Copy URL into browser, and you will see the result list
        
## Building from source

In most cases, you don't need to build CTP from source, unless you make some changes for source codes. If you want to build source, you need install ant first, and then to execute the following commands to build source codes.

```
   cd $HOME/CTP
   ant clean
   ant dist
```

You can find the final jars from directory CTP/sql/lib/cubridqa-cqt.jar.

   
## Usage

Using CTP involves steps:

* get testing scenario
   * git clone git://github.com/../xx.git $HOME/dailyqa/trunk
* configure testing
   * prepare configuration file for the functional testing, such as port, CUBRID system parameters for your testing,
     the below is an example for your reference
     ```yaml
     
      cat $HOME/CTP/conf/sql.conf
      scenario=${HOME}/dailyqa/trunk/scenario/sql
      config_file=test_default.xml
      db_charset=en_US
         
      [sql/cubrid.conf]
      java_stored_procedure=yes
      test_mode=yes
      max_plan_cache_entries=1000
      unicode_input_normalization=no
      cubrid_port_id=1822
      ha_mode=yes
      lock_timeout=10sec
         
      [sql/cubrid_ha.conf]
      ha_mode=yes
      ha_apply_max_mem_size=300
         
      [sql/cubrid_broker.conf/%query_editor]
      SERVICE=OFF
         
      [sql/cubrid_broker.conf/%BROKER1]
      BROKER_PORT=33120
      APPL_SERVER_SHM_ID=33120
         
      [sql/cubrid_broker.conf/broker]
      MASTER_SHM_ID=33122
     ```
     
* execute test
   * For SQL/Medium
        * ctp sql -c $HOME/CTP/conf/sql.conf
        * ctp medium -c $HOME/CTP/conf/medium.conf
* view testing results
   * For SQL/Medium
        * Once testing is complete, result directory will be printed.
        * Go to result directory, and find the ralated folder for your result checking. if you want to check your results from website,
          you can start webconsole service, which will help you to show result based on website. 
             * ``ctp webconsole start ``, the command will print the URL of result view.
             * open url you get from command above "http://xxx.xxx.xx.xx:xxxx"
            
## How to write test script
* If you want to write test script based on CTP tool, you must follow the below rules, only that kind of script which meets the specification of CTP will be recognized and executed correctly.
  * the extension of script file must be .sql, and the answer of script which will be used to examine success or fail must be end with .answer
  * the structure of script must same as the below

```
	 _08_primary_foreign_key
	                       /cases
	                             /int_primary_key_test.sql
	                       /answers
	                             /int_primary_key_test.answer
```
NOTE: 
  * the keywork "cases" and "answers" must be included
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
           

## License
CTP is published under the BSD 3-Cause license. See LICENSE.md for more details.
Third-party libraries used by CTP are under their own licenses. See LICENSE-3RD-PARTY.md for details on the license they use.
