Maven Coffee Sales Analysis:

**Dataset Details:**
The dataset Coffee Shop Sales, contains sales transaction records from a coffee shop, with the following columns:
1.	transaction_id: A unique identifier for each transaction.
2.	transaction_date: The date of the transaction (spanning from January 1 to June 30, 2023).
3.	transaction_time: The time of the transaction.
4.	transaction_qty: The quantity of items purchased, ranging from 1 to 8.
5.	store_id: Identifier for the store, with values from 3 to 8.
6.	store_location: Location of the store (e.g., "Lower Manhattan," "Hell's Kitchen").
7.	product_id: Identifier for the product.
8.	unit_price: The price per unit of the product, ranging from $0.80 to $45.00.
9.	product_category: The category of the product, with values like "Coffee," "Tea," and "Drinking Chocolate."
10.	product_type: A more specific product type, such as "Gourmet brewed coffee" or "Hot chocolate."
11.	product_detail: Detailed description of the product (e.g., "Ethiopia Rg," "Spicy Eye Opener Chai Lg").


Data cleaning: 
- Clean the data set add Create a Calender table.
  
<img width="233" alt="image" src="https://github.com/user-attachments/assets/d2c514b9-7143-4e30-bc23-c7ac0908d5b7">


### **Data Modeling:**

The dataset modeling links all Sheets and Tables.

<img width="873" alt="image" src="https://github.com/user-attachments/assets/3bb67b67-ebba-4f86-9f42-006358f90b1a">



### **DAX Measure:**
1.Total sales = SUM(Transactions[Sales])

2. Total Quantity_sold = SUM(Transactions[transaction_qty])
3. Total Orders = DISTINCTCOUNT(Transactions[transaction_id])

4. Top_Product = 
CALCULATE(
    MAX('Transactions'[product_detail]),
    TOPN(
        1,
        SUMMARIZE(
            'Transactions',
            'Transactions'[product_detail],
            "TotalSales", SUM('Transactions'[transaction_qty])
        ),
        [TotalSales],
        DESC
    )
)


5. Top_Location = 
CALCULATE(
    MAX('Transactions'[store_location]),
    TOPN(
        1,
        SUMMARIZE(
            'Transactions',
            'Transactions'[store_location],
            "TotalSales", SUM('Transactions'[transaction_qty])
        ),
        [TotalSales],
        DESC
    )
)


6.PM Sales = 
CALCULATE([CM Sales],DATEADD('Calendar Table'[Date],-1,MONTH))

7. PM Quantity Orders = 
        CALCULATE([CM quantity Orders],DATEADD('Calendar Table'[Date],-1,MONTH))

8.PM Orders = 
         CALCULATE([CM Orders],DATEADD('Calendar Table'[Date],-1,MONTH))

9. placeholder label p_type = SELECTEDVALUE(Transactions[product_type]) & " | " & FORMAT([Total sales]/1000,"$0.00K")
10. OTIF value = COUNTX(
    factOrders, 
    IF(
        factOrders[On Time] == "True" && factOrders[Fill rate] == 100, 
        1, 
        BLANK()
    )
)

11. placeholder label location = SELECTEDVALUE(Transactions[store_location]) & " | " & FORMAT([Total sales]/1000,"$0.00K")
12. placeholder label category = SELECTEDVALUE(Transactions[product_category]) & " | " & FORMAT([Total sales]/1000,"$0.00K")
13. placeholder = 0

14. Mom Sales and diff Sales = 
VAR month_diff= [CM Sales]-[PM Sales]
VAR mom= month_diff / [PM Sales]
VAR Mark= IF(month_diff>0,"+","")
VAR mark_term=if(month_diff>0,"◤","◢")
RETURN
mark_term & " " & Mark & FORMAT(mom,"#0.0%" & " | " & Mark & FORMAT(month_diff/1000,"0.0K"))& " " & "VS LM"  

15.Mom Qty Orders and diff qty orders = 
VAR month_diff= [CM quantity Orders]-[PM Quantity Orders]
VAR mom= month_diff / [PM Quantity Orders]
VAR Mark= IF(month_diff>0,"+","")
VAR mark_term=if(month_diff>0,"◤","◢")
RETURN
mark_term & " " & Mark & FORMAT(mom,"#0.0%" & " | " & Mark & FORMAT(month_diff/1000,"0.0K"))& " " & "VS LM"  

16. Mom Orders and diff orders = 
VAR month_diff= [CM Orders]-[PM Orders]
VAR mom= month_diff / [PM Orders]
VAR Mark= IF(month_diff>0,"+","")
VAR mark_term=if(month_diff>0,"◤","◢")
RETURN
mark_term & " " & Mark & FORMAT(mom,"#0.0%" & " | " & Mark & FORMAT(month_diff/1000,"0.0K"))& " " & "VS LM"  
    
17. Mom growth Sales = 
VAR month_diff= [CM Sales]-[PM Sales]
VAR mom= month_diff / [PM Sales]
VAR Mark= IF(month_diff>0,"+","")
VAR mark_term=if(month_diff>0,"◤","◢")
RETURN
mark_term & " " & Mark & FORMAT(mom,"#0.0%")

18.Mom growth Orders = 
VAR month_diff= [CM Orders]-[PM Orders]
VAR mom= month_diff / [PM Orders]
VAR Mark= IF(month_diff>0,"+","")
VAR mark_term=if(month_diff>0,"◤","◢")
RETURN
mark_term & " " & Mark & FORMAT(mom,"#0.0%")

19. Mom growth growth qty = 
VAR month_diff= [CM quantity Orders]-[PM Quantity Orders]
VAR mom= month_diff / [PM Quantity Orders]
VAR Mark= IF(month_diff>0,"+","")
VAR mark_term=if(month_diff>0,"◤","◢")
RETURN
mark_term & " " & Mark & FORMAT(mom,"#0.0%")

20. Last Month sale = CALCULATE(SUM(Transactions[Sales]),SAMEPERIODLASTYEAR('Calendar Table'[Date]))

21. Increase sale = [CM Sales]-[PM Sales]

22. increase % = 
[Increase sale]/([PM Sales]+[CM Sales])

23. Daily average sales = AVERAGEX(ALLSELECTED(Transactions[transaction_date]),[Total sales])

24. Daily average qty sold = AVERAGEX(ALLSELECTED(Transactions[transaction_date]),[Total Quantity_sold])

25. Color bar for Average Sales = IF([Total sales]>[Daily average sales],"Above Average","Below Average")

26. CM Sales = 
VAR Select_month= SELECTEDVALUE('Calendar Table'[Month])
RETURN 
TOTALMTD(CALCULATE([Total sales],'Calendar Table'[Month]=Select_month),'Calendar Table'[Date])

27. CM quantity Orders = 
VAR Select_month= SELECTEDVALUE('Calendar Table'[Month])
RETURN 
TOTALMTD(CALCULATE([Total Quantity_sold],'Calendar Table'[Month]=Select_month),'Calendar Table'[Date])

28. CM Orders = 
VAR Select_month= SELECTEDVALUE('Calendar Table'[Month])
RETURN 
TOTALMTD(CALCULATE([Total Orders],'Calendar Table'[Month]=Select_month),'Calendar Table'[Date])


### **DashBoard And Visualization:**

## *Dashboard:*
- Total sales, Total Orders, Total Quantity_sold analysis.
- Sales trends Prediction analysis. 
- Top Quantity analysis.
- Location-wise analysis.
- previous and current month sales, Orders analysis.

 <img width="671" alt="image" src="https://github.com/user-attachments/assets/3903ffeb-aa8b-4076-bab1-0f1a6aec36c0">

# Hover any chart see a tooltip details:

![image](https://github.com/user-attachments/assets/2083112a-fdc4-432a-b359-2c978e5c7f6b)




## *Service level analysis:*

- ‘On-time delivery (OT) %’, ‘In-full delivery (IF) %’, and OnTime in full (OTIF) %’ Service analysis by Sales person

<img width="572" alt="2 ph" src="https://github.com/user-attachments/assets/920ab8b7-8f93-4b7d-9d37-5d34114757aa">




**Order lines at a glance:**

- Total Order Quantity 
- Order analysis in Person analysis 

 <img width="577" alt="3 pha" src="https://github.com/user-attachments/assets/afe212a4-6105-4dc9-b255-3ce2029e6cc8">


**Key Metrics:**

- On-time product analysis.
- IF%, OTIF%, LIFR%, VoFR% analysis by Product type.
- Trend Analysis.

<img width="572" alt="4 ph" src="https://github.com/user-attachments/assets/00c877ee-cf66-4b62-b7ef-b7f039197d97">

**Lead Time Analysis:**

 - Delivery Delay analysis by Product based analysis.

<img width="582" alt="5 ph" src="https://github.com/user-attachments/assets/02ab1cd1-a9a2-47ad-b57a-351fbb0feaf8">

**products at glance:**

- Product trends analysis LIFR%.
- Product trends analysis by VoFR%.

<img width="578" alt="6 ph" src="https://github.com/user-attachments/assets/e919e8bb-5a26-4396-835f-9687f5c22bf3">


## **Project Impact:**
-Improved Decision-Making: By visualizing regional and individual performance, stakeholders can address inefficiencies, allocate resources more effectively, and target high-priority issues.

-Enhanced Customer Satisfaction: Monitoring OTIF% ensures timely and complete delivery of orders, reducing customer dissatisfaction and improving reliability.

-Supply Chain Optimization: Continuous tracking of order lines, fulfillment rates, and delivery performance provides a clear view of the operational bottlenecks, enabling informed process adjustments.

-Cost Savings: Identifying inefficiencies (e.g., low LIFR or VoFR) enables pharmaceutical companies to streamline operations, reducing excess inventory and logistics costs.




