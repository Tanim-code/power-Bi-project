### **Dataset Details Information:**

 **InvoiceNo**: A 6-digit integral number uniquely assigned to each.
 
transaction. If the code starts with the letter 'c', it indicates a cancellation.

 **StockCode**: A 5-digit integral number uniquely assigned to each distinct product.
 
**Description**: The name of the product/item.

**Quantity**: The quantity of each product/item per transaction (numeric).

**InvoiceDate**: The date and time when each transaction was generated.

**UnitPrice**: The unit price of the product, in sterling.

**CustomerID**: A 5-digit integral number uniquely assigned to each customer.

**Country**: The name of the country where each customer resides.

### **Data Preprocessing steps:**

1. Removed canceled orders (invoices starting with 'C') from InvoiceNo.
2. Removed empty values from CustomerID.
3. Created a "sales" column by multiplying Quantity and UnitPrice columns.
4. Created a "sales" column by multiplying Quantity and UnitPrice columns.
5. Created a "month since first transaction" column by                      

```
    month since first transaction = 
    DATEDIFF(RELATED(DimCustomer[First Transacition Month]),
    RELATED(DimDate[Date]),
    MONTH
    )
```
6. Created a "Week since first transaction" column by                                            
```
   week since frist transaction = 
   DATEDIFF(
        RELATED(DimCustomer[Frist Transaction week]),
        RELATED(DimDate[Date]),
        WEEK
   )
```
### **Additional Tables Created**
Two additional tables were created for the project:

-  DimCustomer:

Contains CustomerID, First Transaction Month, and First Transaction Week.

-  DimDate:

Contains all unique dates from 12/1/2010 to 12/9/2011.
Includes Start of Month and Start of Week attributes.


### **Data Modeling**
<img width="455" alt="Data modeling cohort" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/ed9e61bb-1c8d-4954-b298-0c561b009733">


### DAX Measure for this Project: 
There are lot of Measures Uses for individual calculation

**General Measure:**
1.  ```
                   Avg Order Value= DIVIDE(
                                        [Revenue],
                                      [Total Order ]   )
```

2.
                        Avg Revenue per Customar =DIVIDE([Revenue],[Total Customar])


3.                                             unit sold = 
                                              SUM(FactSales[Quantity])



4. 
                     Total Order = 
                         DISTINCTCOUNT(
                                        FactSales[Invoice]
 

5. ```
                         Total Customar = 
                                           COUNTROWS(DimCustomer)
```

6. ```
           Revenue = 
                         SUMX(
                                    FactSales,
                                   FactSales[Quantity]*
                                     FactSales[Price]
                                   )
```

7.  ```
                          Rev growth % = 
                           VAR up_arrow=UNICHAR(129137)
                           VAR down_arrow=UNICHAR(129139)

                            VAR Current_revenue=[Revenue]
                        VAR pm_revenue=[previous month revenue]

                       VAR MOM_growth=DIVIDE(Current_revenue - pm_revenue,pm_revenue)

                      RETURN
                       IF(
	            MOM_growth < 0, ROUND(MOM_growth*100, 1) &"% "& down_arrow,
	           ROUND(MOM_growth*100, 1) &"% "& up_arrow
                    )
```

8.
                                              previous month revenue = 
                                                 CALCULATE(
                                                          [Revenue],
                                                                DATEADD(
                                                            DimDate[Date],
                                                               -1,
                                                         MONTH
                                                      ))


9.
     ```
                                                previous month Orders = 
                                                  CALCULATE( 
                                                  [Total Order],
                                                  DATEADD(DimDate[Date],
                                                 -1,
                                                  MONTH))

    ```



**Cohort Measure:**


1. ``` 
                                      Active Customer = 
                                                  COUNTROWS(
                                                 VALUES(FactSales[Customer ID])
                                                  )
```

2. ```
                                       churned customer = 
                                        SWITCH(
                                      TRUE(),
                                    ISBLANK([Retantion Rate]),   
                                       BLANK(),
                                        [New Customar]-[Cohort performance]
                                      )
```

3. ```
                                         churned rate = 
                              [churned customer]/[New Customar]
```

4.   ```
                                          Cohort performance = 
                              VAR mindate=MIN(DimDate[Start of Month])
                              VAR maxdate=MAX(DimDate[Start of Month])

                           RETURN
                                  CALCULATE(
                                        [Active Customer],
                               REMOVEFILTERS(DimDate[Start of Month]),
                                RELATEDTABLE(DimCustomer),
                                DimCustomer[First Transacition Month]>=mindate
                                 &&
                               DimCustomer[First Transacition Month]<=maxdate)
```
5.  ```
                              difference = 
                           [Active Customer]-[validation]
```

6. ```
                          lost customer = 
	                        VAR currentmonth_customer=VALUES(FactSales[Customer ID])
	                        VAR previousmonth_customer=
                                 CALCULATETABLE(
                                  VALUES(FactSales[Customer ID]),
                                 PREVIOUSMONTH(DimDate[Start of Month])
                               )
	                  VAR lost_customner=
     	                 EXCEPT(previousmonth_customer,currentmonth_customer)

	                RETURN
                        COUNTROWS(lost_customner)
```

7.   ```
                   New Customar = 
                         CALCULATE(
                       [Active Customer],
                        FactSales[month since first transaction]=0
                        )
```

    
8. ```
                                        Recovered Customers = 

 	VAR _CustomersThisMonth =
        VALUES(FactSales[Customer ID])

	VAR _CustomersLastMonth = 
        CALCULATETABLE(
        VALUES(FactSales[Customer ID]),
        PREVIOUSMONTH(DimDate[Start of Month])
        )

	VAR _NewCustomers =
          CALCULATETABLE(
        VALUES(FactSales[Customer ID]),
        FactSales[Month Since First Transaction] = 0
        )
  
	VAR _RecoveredCustomers = 
           EXCEPT(
             EXCEPT(
            _CustomersThisMonth, _CustomersLastMonth
        ),  -- REMOVE LAST MONTH'S CUSTOMERS
        _NewCustomers
    ) -- REMOVE NEW CUSTOMERS

	RETURN(
    COUNTROWS(_RecoveredCustomers)
  )
```

9. ```
                            Retaind Customer = 
         VAR currentmonth_customer= VALUES(FactSales[Customer ID])
        VAR previousmonth_customer= CALCULATETABLE(
        VALUES(FactSales[Customer ID]),
        PREVIOUSMONTH(DimDate[Start of Month])
    )

         VAR retain_customer= INTERSECT(currentmonth_customer,previousmonth_customer)

         RETURN
        COUNTROWS(retain_customer)

```

10. ```
              Retention Rate = 
               DIVIDE([Cohort performance],[New Customar])
```

11.    ```
             validation = 
	      [New Customar]+[Retaind Customer]+[Recovered Customers]


### **DashBoard And Visualization**

 - monthly cohort Based New customer and Churned Customer Analysis:

<img width="649" alt="1 cohort" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/0bb39d09-97b3-4733-9ede-d152880b54d9">

- Churned Customer Trend Analysis:                                                                                         <img width="649" alt="2 cohort" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/1336fed6-a758-4570-962b-c7f0cee50c8e">   
                              
- Lost customer and Recoverd Customer analysis
<img width="649" alt="3 cohort" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/011f25a4-0926-4afe-bd74-7bb597ddab69">


### Project Impact:
The project equips stakeholders with actionable insights Derived from cohort analysis, enabling data-driven decision-making to enhance customer retention, optimize marketing strategies, and maximize revenue generation.



