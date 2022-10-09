# SQL_Machine_Idle_Sums
For this example, assume a company has 4 machines that occasionally have idle time. To record and analyze this idle time, the machine keeps track of when it is not operating. When coming out of idle, the HMI on the machine prompts the operator for input on why it was idle. This information is then stored directly into a SQL table.

The files here create a sample database of machine idle events, and reformats the data for easier viewing of the data per machine.

Create_Sample_Database creates the sample database. This database has the following format:
     -        |    -     |      -     |       -    |     -      |      -            |
-------------|---------|-----------|-----------|-----------|------------------|
Column name: | date    |  time     |  machine  |  reason   |  elapsed_seconds |
Data type:   | datetime|time       | int       |varchar(11)|int               |
Description: |Date of event|Time of Event|Machine ID #|Idle Reason|Elapsed Seconds of Event|
Example Row: |2022-01-13 00:00:00.000|	02:05:24.0000000	|1	|changeover|	149|

SQL_Pivot_Method creates a single pivot table, aggregating on the sum of the idle times per each machine.
Expected output of sum pivot table:

Idle Reason | Machine 1 | Machine 2 | Machine 3 | Machine 4|
------------|-----------|-----------|-----------|----------|
break       |   1287    |    953    |   1697    |   654    |
changeover	|  1450	    |  705	    |    1396	  |    734   |
downtime	  |  (null)	  |  (null)	  |  1798	    |  (null)  |
maintenance	|  763	    |    (null)	|    (null)	|    1578  |
other	      |  693	    |    (null)	|    (null)	|    (null)|

Joining_Multiple_Pivots creates three pivot tables, aggregating on the sum, average, and total instances of idle events for each machine. These three tables are then joined together using 'reason' column in each pivot table.
Outcome of the three pivot tables joined together:

Idle_Reason	| Machine_1_Sum	| Machine_1_Average	| Machine_1_Count	| Machine_2_Sum	| Machine_2_Average |	Machine_2_Count	| Machine_3_Sum |	Machine_3_Average	| Machine_3_Count	| Machine_4_Sum	| Machine_4_Average	| Machine_4_Count|
------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
break	      | 1287|	643 |	2   |	 953|	 476|	2 	|1697 |	424	|4	  |654	|327	|2    |
changeover	|1450	|362	|4	  |705	| 705	|1    |	1396|465  |	3	  |734	|367	|2    |
downtime	  | NULL|	NULL|	0   |	NULL|	NULL|	0   |	1798|	899 |	2   |	NULL|	NULL|	0   |
maintenance	|763	|763	|1	  | NULL|	NULL|0	  |NULL	|NULL	|0	  |1578	|526	|3    |
other	      |693	|693	|1	  |NULL	|NULL	|0	  |NULL	|NULL	|0	  |NULL	|NULL	|0    |
