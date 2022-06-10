# FreePBX CDR cyrillic fix

    rpm -e --nodeps `rpm -qa | grep mysql-connector-odbc` && yum -y install mariadb-connector-odbc && fwconsole restart
  
