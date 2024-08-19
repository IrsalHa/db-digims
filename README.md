time psql -U postgres -d bjb_prod -c "\COPY tp_dpensiuncln TO '/root/tp_dpensiuncln_data4.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '~');"

time psql -h 10.12.4.167 -U postgres -d digimsdb -c "\COPY MBRANCH TO '/root/MBRANCH_DATA.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '#', QUOTE '\"', ESCAPE '\"');"
