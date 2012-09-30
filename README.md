Oracle-Database
===============
Oracle PL/SQL

/* The business problem is a simple one to state. Don't return a result set unless the number of rows to be 
   returned meets some threshold. For example ... If there are fewer than 15 rows ... return NULL.
   Here is one solution. 

   The DDL to create the AIRPLANES table and load it can be found here:
   http://www.morganslibrary.org/files/airplanes.sql
*/

DECLARE
 TYPE ProgID IS TABLE OF airplanes.program_id%TYPE
 INDEX BY BINARY_INTEGER;
 pid_t ProgID;

 TYPE OrdDat IS TABLE OF airplanes.order_date%TYPE
 INDEX BY BINARY_INTEGER;
 ord_t OrdDat;
BEGIN
  SELECT program_id, order_date
  BULK COLLECT INTO pid_t, ord_t
  FROM airplanes
  WHERE program_id = '737'
  AND customer_id = 'DAL'
  AND line_number LIKE '%47%';

  IF pid_t.COUNT < 400 THEN
    FOR i IN 1..pid_t.COUNT LOOP
      dbms_output.put_line(ord_t(i));
    END LOOP;
  ELSE
    NULL;
  END IF;
END;
/