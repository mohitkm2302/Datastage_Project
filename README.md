# Datastage Project
DataStage is an ETL tool which is used to Extract the data from different data source, Transform the data as per the business requirement and Load into the target database. The data source can be of any type like Relational databases, files, external data sources, etc.

- ## Important Clients Interface

- #### Three components comprise the DataStage client: DataStage Administrator, DataStage Designer, DataStage Director.

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/IMPS.PNG)


- ## RESPONSIBILITIES FOR Datastage Administrator:
Developing new tools and processes to ensure effective use of DataStage product. The position will also be responsible for administrating and maintaining a DataStage shared environment. This includes: Designing and sizing the environment.


- ## RESPONSIBILITIES FOR Datastage Designer:
The DataStage Designer is the primary interface to the metadata repository and provides a graphical user interface that enables you to view, edit, and assemble DataStage objects from the repository needed to create an ETL job. An ETL job should include source and target stages.


- ## RESPONSIBILITIES FOR Datastage Director:
DataStage Director has three view options: The Status view displays the status, date and time started, elapsed time, and other run information about each job in the selected repository category. The Schedule view displays job scheduling details. The Log view displays all of the events for a particular run of a job.

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/DIRECTOR_CLIENT.PNG)


- ## Oracle connector 
Oracle connector  is used to access Oracle database systems and perform various read, write, and load functions. Setting required user privileges.

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/ORACLE_DATA_CONN.PNG)


- ## Parameter sets 
Parameter sets enable you to expose different parameters to the user. And, to return different information based on the parameters specified by the user. You can only use one parameter set at a time.

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/GLOBAL_PARAMETER.PNG)


########################################################################################

## Project consists of 4 major jobs-

```
ORA - data from Oracle Database
DS - Dataset Files
FILE - Sequential Files
AGG - Aggregation
SHARED - Shared Containers
TRANSFORMER
MERGE
JOIN
LOOKUP
FUNNEL
SORT
FILTER
REMOVE_DUPLICATES
ORACLE_DATABASE
EXECUTE_COMMANDS
```


### - ( 1 ) Requirement: SOURCE  -->  TARGET

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB1_SCHEMA.PNG)

### Implementation:

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB1_UI.PNG)



### - ( 2 ) Requirement: SOURCE  -->  TARGET

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB2_SCHEMA.PNG)

### Implementation:

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB2_UI.PNG)

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB2_UI_CONTAINER.PNG)


### - ( 3 ) Requirement: SOURCE  -->  TARGET

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB3_SCHEMA.PNG)

### Implementation:

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB3_UI.PNG)

### - ( 4 ) Requirement: SOURCE  -->  TARGET

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB4_SCHEMA.PNG)

### Implementation:

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/JOB4_UI.PNG)


## SEQUENCIAL JOBS
Sequence job, that you use to specify a sequence of parallel jobs or server jobs to run. You specify the control information, such as the different courses of action to take depending on whether a job in the sequence succeeds or fails.

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/SEQUENTIAL_JOB.PNG)

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/DC_SEQ_LOGS.PNG)


########################################################################################

## Some more Jobs-

```

1] SRC :- EMP(ORA)
   TRG(FILE) :- EMPNO,ENAME,SAL,SEQ_NUM
   DATA :- ONLY FOR SAL >= 1000 & GENERATE THE SEQ_NUM.

2] SRC :- EMP(ORA), DEPT_(ORA)
   TRG(DS) :- RANK_NUM,EMPNO,ENAME,SAL,DEPTNO,DNAME,LOC
   RANK_NUM = SEQUENCE NUMBERS
   DATA :-TOP 3 SAL RECORDS

3] SRC :- EMP
   TRG(DS) :- EMPNO,ENAME,SAL,COMM,TSAL,DEPTNO,RANK_NUM
   RANK_NUM = SEQUENCE NUMBERS

   DATA :- TSAL = SAL + COMM[HANDLE NULL]
	   BOTTOM 5 TSAL RECORDS.

4] SRC :- EMP
   TRG[2 ORA TABLES] :- EMPNO,ENAME,SAL,TAX,SEQ_NUM
   SEQ_NUM = SEQUENCE NUMBERS

   DATA :- TAX = SAL * 0.10

	   DATA INTO THE TRGS MUST BE LOADED IN AN ALTERNATE WAY.


@PARTITIONNUM + ((@INROWNUM -1) * @NUMPARTITIONS) +1


-------------------------------------------------------------

1] SRC :- EMP(ORA),DEPT(ORA)
   TRG(2) :- DEPTNO,DNAME,LOC,SUM_SAL,TAX_SUM_SAL

	TRG1(ORA) :- 
	SUM(SAL) GROUP BY DEPTNO 
	TAX_SUM_SAL = SUM_SAL * 0.50 IF DEPTNO = 10
	TAX_SUM_SAL = SUM_SAL * 0.25 IF DEPTNO = 20
	TAX_SUM_SAL = SUM_SAL * 0.10 IF DEPTNO = 30
		
	DATA FOR TRG1(DS) :- (DEPTNO = 10 OR DEPTNO = 20) AND TAX_SUM_SAL >= 2000
		   
	TRG2(FILE) :- SUM_SAL BETWEEN 4000 & 15000 

2] SRC :- EMP(ORA)
   TRG(DS) :- EMPNO,ENAME,SAL,DEPTNO

	DEPTNO = P1(INTEGER) & SAL >= P2(INTEGER)

3] SRC :- EMP(ORA),DEPT(ORA)
   TRG() :- 
	CREATE A PARAMETER SET WITH REQUIRED PARAMETERS & USE IN THIS JOB :-

	TRG1(FILE) :- EMPNO,ENAME,SAL,COMM,TSAL,TAX,TRATE,NETSAL
	
	TSAL = SAL + COMM[HANDLE NULL]
	TRATE = P1(FLOAT/DOUBLE)
	TAX = TSAL * TRATE
	NETSAL = TSAL - TAX

	DATA ONLY FOR NETSAL >= P2(FLOAT/DOUBLE)

	TRG2(DS) :- SEQ_NUM,DEPTNO,DNAME,SUM_SAL,TAX_SUM_SAL,TAX_RATE
	TAX_RATE = P3(FLOAT/DOUBLE)
	SUM_SAL = SUM(SAL) GROUP BY DEPTNO 
	TAX_SUM_SAL = SUM_SAL * TAX_RATE

	SEQ_NUM = GENERATE SEQUENCE NUMBER


```

### - ( 1 )

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/J1_UI.PNG)

### - ( 2 )

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/J2_UI.PNG)

### - ( 3 )

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/J3_UI.PNG)

### - ( 4 )

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/J4_UI.PNG)

### - ( 5 )

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/J5_UI.PNG)

### - ( 6 )

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/J6_UI.PNG)

### - ( 7 )

![alt text](https://github.com/mohitkm2302/Datastage_Project/blob/main/PROJECT/pics/J7_UI.PNG)




## Made with 💖 & 🔥 by MKM.
