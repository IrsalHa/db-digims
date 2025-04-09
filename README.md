time psql -U postgres -d bjb_prod -c "\COPY tp_dpensiuncln TO '/root/tp_dpensiuncln_data4.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '~');"

time psql -h 10.12.4.167 -U postgres -d digimsdb -c "\COPY MBRANCH TO '/root/MBRANCH_DATA.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '#', QUOTE '\"', ESCAPE '\"');"


time psql -h 10.12.4.167 -U postgres -d digimsdb -c "\COPY MBRANCH TO '/root/MBRANCH_DATA.csv' WITH (FORMAT CSV, HEADER, NULL 'NULL', DELIMITER '#', QUOTE '\"', ESCAPE '\"');"

[+] PSQL [+] INI BENER
time psql -h 10.12.4.164 -U postgres -d digimsdb2 -c "\COPY MBRANCH TO '/root/MBRANCH_DATA.csv' WITH (FORMAT CSV, HEADER FALSE, DELIMITER '#', QUOTE '$',  FORCE_QUOTE *);"

[+] SQLLDR [+] INI BENER
sqlldr userid=ORCLPDB1/ORCLPDB1@localhost:1521/XEPDB1 control=MBRANCH.ctl  bad=MBRANCH.bad

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

//Select Trim Copy

DO $$
DECLARE
    r RECORD;
    query TEXT := 'SELECT ';
    tablename TEXT := 'tembossdata';
BEGIN
    -- Menghasilkan query SELECT dengan TRIM pada kolom teks dan tanpa TRIM pada kolom lainnya
    FOR r IN (
        SELECT column_name, data_type
        FROM information_schema.columns
        WHERE table_name = tablename
        ORDER BY ordinal_position
    ) LOOP
        IF r.data_type IN ('character varying', 'text', 'character') THEN
            query := query || 'TRIM(' || r.column_name || ') AS ' || r.column_name || ', ';
        ELSE
            query := query || r.column_name || ', ';
        END IF;
    END LOOP;

    -- Menghapus koma terakhir dan menambahkan FROM clause
    query := left(query, length(query) - 2) || ' FROM ' || tablename;

    -- Menampilkan query untuk debugging
    RAISE NOTICE 'Generated Query: %', query;
END $$;

//untuk install posgres di sql developer * SQL*Developer Java lib/ext directory.

//Untuk drop table // select 'drop table '||table_name||' cascade constraints;' from user_tables;

//Untuk drop selain table // select 'drop '||object_type||' '|| object_name || ';' from user_objects where object_type in ('VIEW','PACKAGE','SEQUENCE', 'PROCEDURE', 'FUNCTION', 'INDEX')
