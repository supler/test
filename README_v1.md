# CTP - CUBRID Test Program

CTP as testing tool for an open source project CUBRID, which is developed based on Java language, and it integrates functional testing, non-functional testing 
and result showing into one. it is easy to execute testing with a simple configuration. 

## Usage

Using CTP involves four steps:

 1. Check out tool from github http://www.xxx.
 2. Configure testing
    ** For functional testing
       -- You MUST configure scenario directory under [sql] section, which will tell CTP which scenario directory your will use to execute testing.
       -- Regarding cubrid conf configuation such as port, system parameters for CUBRID .. etc, you need configure them based on [sql/cubrid.conf], 
          [sql/cubrid_ha.conf], [sql/cubrid_broker.conf/%query_editor], [sql/cubrid_broker.conf/%BROKER1] and [sql/cubrid_broker.conf/broker] section in conf file.
          You can copy/modify template conf file CTP/conf/sql.conf or create it according to your requirement following that's format.
       -- Start your testing following the usage of CTP
          a) ctp sql -c conf/sql.conf
          b) ctp medium -c conf/medium.conf
          c) ctp sql                 #use default configuration file: CTP/conf/sql.conf
          d) ctp medium              #use default configuration file: CTP/conf/medium.conf
          e) ctp sql medium          #run both sql and medium with default configuration
          f) ctp medium medium       #execute medium twice with default configuration
       
    ** For result showing(If you want to check your testing result via web page, webconsole will help you do that.)
       -- Configure port http port and functional testing result directory from CTP/conf/webconsole.conf
       -- Execute "sh CTP/bin/ctp.sh webconsole start" command to start service, and it will print the results web page url such as http://127.0.0.1:8888/
       -- Access the url you will find the results, and click each one for the details
       -- Execute "sh CTP/bin/ctp.sh webconsole stop" command to stop service.
       
    ** For non-functional testing
       - TBD
 3. Projecting a context to get the final configuration.
 4. Get values from configuration entries.


## License

Code licensed under the BSD 3-Clause license.  See LICENSE file for terms.
