time psql -U postgres -d bjb_prod -c "\COPY tp_dpensiuncln TO '/root/tp_dpensiuncln_data4.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '~');"

time psql -h 10.12.4.167 -U postgres -d digimsdb -c "\COPY MBRANCH TO '/root/MBRANCH_DATA.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '#', QUOTE '\"', ESCAPE '\"');"


time psql -h 10.12.4.167 -U postgres -d digimsdb -c "\COPY MBRANCH TO '/root/MBRANCH_DATA.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '#', QUOTE '\"', ESCAPE '\"');"


[+] CEK PRIMARY KEY & FK [+]
//CEK PK

SELECT owner, table_name, constraint_name, status
FROM all_constraints
WHERE constraint_type = 'P' AND owner = 'ORCLPDB2' AND status = 'DISABLED';

//CEK FK

SELECT owner, table_name, constraint_name, status
FROM all_constraints
WHERE constraint_type = 'R' AND owner = 'ORCLPDB2' AND status = 'DISABLED';

//ENABLE NYA
ALTER TABLE ORCLPDB2.MCOURIER ENABLE CONSTRAINT MCOURIER_FK1;