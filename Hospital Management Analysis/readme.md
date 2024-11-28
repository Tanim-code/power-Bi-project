Hospital Management Analysis:

**Dataset Details:**
1. Admission Table 
2. Calendar Table 
3. Department Table 
4. Encounter Table
5. Fact_finance Table
6. fact_procedure Table
7. fact_satisfaction Table
8. project patient Table
9. Staff Table 



### **Data Modeling:**

The dataset modeling links all Sheets and Tables.

![image](https://github.com/user-attachments/assets/04000f12-dbbd-473d-b813-2bcec72b1dd6)




### **DAX Measure:**
1. Total Treatment Cost = 
SUM(fact_procedures[BASE_COST])

2. Treatment cost OutPatients = 
CALCULATE(
    [Total Treatment Cost],
     encounters[ADMISSION_ID] = "No Admissions" 
)
3. Treatment cost InPatients = 
CALCULATE(
    [Total Treatment Cost],
     encounters[ADMISSION_ID] <> "No Admissions" 
)

4. Satisfaction Rate = [Average Satisfaction Score]

5. Salary Disbursed = SUM(fact_finance[ Salary Disbursed ])

6. Revenue = 
CALCULATE(
    SUM(encounters[TOTAL_CLAIM_COST]),
    FILTER(
        encounters,
        encounters[ID] IN VALUES(fact_procedures[ENCOUNTER])
    )
)

7. Readmissions Rate = 
VAR readmissionCount =
CALCULATE(
    DISTINCTCOUNT('fact_procedures'[PATIENT]),
    FILTER(
        'admissions',
        'admissions'[ADMISSION_TYPE] = "Readmissions"
    ),
    RELATEDTABLE('encounters'),
    RELATEDTABLE('fact_procedures')
)


RETURN
CALCULATE(
readmissionCount / COUNTROWS(admissions)
)

8. ReadmissionCount = 
CALCULATE(
    DISTINCTCOUNT('fact_procedures'[PATIENT]),
    FILTER(
        'admissions',
        'admissions'[ADMISSION_TYPE] = "Readmissions"
    )
)

9. Previous Salary = CALCULATE(
    [Salary Disbursed],
    DATEADD('calendar'[Date],-1,YEAR)
)
10.Previous Revenue = CALCULATE(
    [Revenue],
    DATEADD('calendar'[Date],-1,YEAR)
)
11. Previous Budget = CALCULATE(
    [Budget Performance],
    DATEADD('calendar'[Date],-1,YEAR)
)
12. Previous Base cost = CALCULATE(
    [Total Treatment Cost],
    DATEADD('calendar'[Date],-1,YEAR)
)
13. Number of patients Last year = 
    CALCULATE(
        [Number of patients],
        DATEADD('calendar'[Date],-1,YEAR))
14. Number of patients by Risk Level = 
CALCULATE(
    DISTINCTCOUNT(fact_procedures[PATIENT]),
    FILTER(fact_procedures,
        RELATED(encounters[RISKLEVEL]) = SELECTEDVALUE(encounters[RISKLEVEL])
        )

)
15.Number of patients = 
    COUNTROWS(VALUES(fact_procedures[PATIENT])) 

16. Number of outPatients = 
CALCULATE(
    COUNTROWS(VALUES(fact_procedures[PATIENT])),
    encounters[ADMISSION_ID] = "No Admissions" 
) 
    
17. Number of inPatients = 
  CALCULATE(
    COUNTROWS(VALUES(fact_procedures[PATIENT])),
     encounters[ADMISSION_ID] <> "No Admissions" 
)
18. No.Of Male = CALCULATE(COUNTROWS('project patients'), 'project patients'[GENDER]= "M")

19. No.Of FeMale = CALCULATE(COUNTROWS('project patients'), 'project patients'[GENDER]= "F")
20. Mortality Rate = 
VAR numDeaths =  // counts the rows of procedures where procedure date = death date of patient where it is not blank
    CALCULATE (
    COUNTROWS ( 'fact_procedures' ),
    FILTER (
        'fact_procedures',
        'fact_procedures'[START - DATE] = RELATED('project patients'[DEATH_DATE]) 
                                                && NOT ISBLANK(RELATED('project patients'[DEATH_DATE]))
    )
)
VAR numPatients = COUNTROWS(VALUES(fact_procedures[PATIENT])) // counts the unique numbers of patients

RETURN
IF(
    NOT ISBLANK(numDeaths / numPatients),// if numDeath is not 0
        numDeaths / numPatients,
        0
)

21. LOS = 
CALCULATE(
    AVERAGE(admissions[ACTUAL_LENGTH_OF_STAY_HRS]),
    FILTER(
        encounters,
        encounters[ID] IN VALUES(fact_procedures[ENCOUNTER])
    )
)
22. Expenses = 
CALCULATE(
    SUM(encounters[BASE_ENCOUNTER_COST]),
    FILTER(
        encounters,
        encounters[ID] IN VALUES(fact_procedures[ENCOUNTER])
    )
)

23. Data Check = 
   VAR TotalRows = COUNTROWS(encounters)
   VAR BlankAdmissions = COUNTBLANK(encounters[ADMISSION_ID])
   VAR DistinctPatients = DISTINCTCOUNT(encounters[PATIENT])
   RETURN
   "Total Rows: " & TotalRows & 
   " Blank Admissions: " & BlankAdmissions & 
   " Distinct Patients: " & DistinctPatients

24. Budget Performance = SUM(fact_finance[Budget Allocated])

25. Current Value = 
SWITCH(
    TRUE(),
    SELECTEDVALUE('KPI'[Metric Name]) ="Budget Performance", [Budget Performance],
    SELECTEDVALUE('KPI'[Metric Name]) = "Salary Disbursement", [Salary Disbursed],
    SELECTEDVALUE('KPI'[Metric Name]) = "Total base cost", [Total Treatment Cost],
    SELECTEDVALUE('KPI'[Metric Name]) = "Generated Revenue", [Revenue]
)

26. AverageWaitingTime = 
CALCULATE(
    AVERAGE('encounters'[WAITING_TIME_MIN]),
    FILTER(
        'encounters',
        'encounters'[ID] IN VALUES('fact_procedures'[ENCOUNTER])
    )
)

27. Average Treatment Cost = AVERAGE(fact_procedures[BASE_COST])

28. Average Satisfaction Score = AVERAGE(fact_satisfaction[OVERALL_SCORE])

29. Age category = 
SWITCH(
    TRUE(),
    MAX('project patients'[Age]) <= 5, "0-5",
    MAX('project patients'[Age]) <= 12, "6-12",
    MAX('project patients'[Age]) <= 19, "13-19",
    MAX('project patients'[Age]) <= 29, "20-29",
    MAX('project patients'[Age]) <= 39, "30-39",
    MAX('project patients'[Age]) <= 49, "40-49",
    MAX('project patients'[Age]) <= 59, "50-59",
    MAX('project patients'[Age]) <= 69, "60-69",
    MAX('project patients'[Age]) <= 79, "70-79",
    MAX('project patients'[Age]) <= 89, "80-89",
      
    "90+"
)


### **DashBoard And Visualization:**

## *Intro:*

![image](https://github.com/user-attachments/assets/ad83977a-c94d-406f-977f-1424c08b45e8)


## *Overview:*

- KPI: No.of the patient, Outpatient, in-patient, average satisfaction score, average cost.
- Risk Level analysis. 
- Daywise patient analysis. 
- Location analysis. 

![image](https://github.com/user-attachments/assets/ce358ce5-7cbc-4c34-9a1d-836ec1c26469)




**Financial Analysis:**

- Department-based Budget and Revenue analysis.
- Admission Cost, Treatment cost analysis.

![image](https://github.com/user-attachments/assets/ab612048-ffb8-4730-86d0-5802001f4f62)


**Risk Level Analysis:**
- Risk level analysis Age group.
- Risk level analysis by admission source.

![image](https://github.com/user-attachments/assets/5ad1d01c-bbb1-4678-9ca2-7f0a4a70f4e7)


**Doctor performance analysis:**
- Tracking individual Doctor performance.
- Doctor Satisfaction, treatment Cost, Risk level analysis.

![image](https://github.com/user-attachments/assets/b47edddf-f0c8-45db-b7e0-9e467d886d99)

**Division analysis:**
- Waiting Time analysis.
- Risk level, treatment cost analysis.

![image](https://github.com/user-attachments/assets/782f7b7b-dde7-4808-b366-0181d7a79afe)

**Patient analysis:**
-age based paitent analysis.
-waiting time analysis.
-patient details analysis.

![image](https://github.com/user-attachments/assets/c0bbe9d7-8757-4791-93cd-a53a4cc9002e)

**Satisfaction analysis:**
-Service analysis by Patient.

![image](https://github.com/user-attachments/assets/1ca6a12c-4407-4847-b824-2174372eb326)

**Date time analysis:**
-inpatient, outpatient, emergency, and Ambulatory patient time analysis.

![image](https://github.com/user-attachments/assets/81261285-76bc-443e-a8cc-0fbe9fa077ce)

**Scored matrics analysis:**
-final matrics, treatment performance, Workforce matrics KPI analysis.

![image](https://github.com/user-attachments/assets/755e2701-5811-4484-a1ae-82d391946cc4)




## **Project Impact:**
This hospital management data analysis project provides a comprehensive overview of key performance indicators (KPIs) and critical aspects of healthcare services, financial performance, patient experience, and operational efficiency. Below is the impact of the analysis, categorized by focus areas:





