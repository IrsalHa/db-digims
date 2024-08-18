time psql -U postgres -d bjb_prod -c "\COPY tp_dpensiuncln TO '/root/tp_dpensiuncln_data4.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '~');"
