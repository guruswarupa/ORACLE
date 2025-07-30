# 1. Open SQL*Plus from Start Menu
SQL> 

# 2. Connect as SYSDBA
CONNECT / AS SYSDBA;

# 3. Create a New User
CREATE USER myuser IDENTIFIED BY mypass;

# 4. Grant All Privileges to the User
GRANT ALL PRIVILEGES TO myuser;

# 5. Grant Roles to the User
GRANT CONNECT, RESOURCE, DBA TO myuser;
