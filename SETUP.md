# 1. Pull the Oracle XE Docker image
docker pull gvenzl/oracle-xe

# 2. Run Oracle XE container
docker run -d \
  --name oracle-xe \
  -p 1521:1521 \
  -p 8080:8080 \
  -e ORACLE_PASSWORD=YourStrongPassword \
  gvenzl/oracle-xe

# 3. Access SQL*Plus as system user
docker exec -it oracle-xe sqlplus system/YourStrongPassword@//localhost:1521/XEPDB1

# 4. Create a new user
CREATE USER myuser IDENTIFIED BY mypass;

# 5. Grant basic privileges
GRANT CONNECT, RESOURCE TO myuser;

# 6. Optional: Grant explicit session permission
GRANT CREATE SESSION TO myuser;

# 7. Exit SQL*Plus
EXIT;
