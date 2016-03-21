# CTP - CUBRID Test Program

## Introduction
CTP as testing tool for an open source project CUBRID, which is developed based on Java language, and it integrates functional testing, non-functional testing 
and result showing into one. it is easy to execute testing with a simple configuration. 

## Requirements
* Java 1.6 or 1.7, and configure Java environment variable JAVA_HOME.
* Install CUBRID build, and make sure the related environment variable of CUBRID is configured correctly.

## Quick start


## Usage

Using CTP involves steps:

1. get CTP program
   * git clone git://github.com/.../CTP/ctp.git $HOME/CTP , then export PATH=$HOME/CTP/bin:$PATH
   * cd $HOME/CTP/bin
   * ln -s ctp.sh ctp

2. get testing scenario
   * git clone git://github.com/../xx.git $HOME/dailyqa/trunk
3. configure testing
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
     
4. execute test
   * For SQL/Medium
        * ctp sql -c $HOME/CTP/conf/sql.conf
        * ctp medium -c $HOME/CTP/conf/medium.conf
5. to view testing results
   * For SQL/Medium
        * Once testing is complete, result directory will be printed.
        * Go to result directory, and find the ralted folder for your result checking. if you want to check your results from website,
          you can start webconsole service, which will help you to show result based on website. 
             * ``ctp webconsole start ``, the command will print the URL of result view.
             * open url you get from command above "http://xxx.xxx.xx.xx:xxxx"

## License
CTP is published under the BSD 3-Cause license. See LICENSE.md for more details.
Third-party libraries used by CTP are under their own licenses. See LICENSE-3RD-PARTY.md for details on the license they use.