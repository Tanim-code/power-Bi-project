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

![image](https://github.com/user-attachments/assets/2d6415a3-cbb1-4b79-907e-53435631ef2c)


# Hover any chart see a tooltip details:

![image](https://github.com/user-attachments/assets/2083112a-fdc4-432a-b359-2c978e5c7f6b)




## *Customer Purchase Patterns:*

- Specific analysis by Sales, Orders, and Sold quantity.
- Total sales, and orders Analysis by day. 
- Sales analysis by Hour of Day. 
- Sales by product types and product category. 

![image](https://github.com/user-attachments/assets/035a2f8f-5aff-4828-843d-dfb9e6111c81)

# Hover any chart see a tooltip details:

![image](https://github.com/user-attachments/assets/bd0145be-ade8-4274-aa0c-beb7a1da7ec1)



**Product at a glance:**

- Product type and Categories Growth analysis.
- Top sales location and Top product Findings.

![image](https://github.com/user-attachments/assets/a7de304e-d309-4e47-9212-be589c43ae20)



**Story Telling:**

![image](https://github.com/user-attachments/assets/93823080-847f-41bf-b4e6-f36ab078e0a6)



## **Project Impact:**
This project analyzes coffee shop sales patterns and customer behaviors, revealing key insights that can drive actionable business decisions. Key findings include product performance, time-based sales trends, location-specific variations, customer loyalty, and seasonal demand spikes. Recommendations are tailored to optimize the product mix, leverage weekday vs. weekend promotions, implement time-based marketing strategies, and enhance customer segmentation for personalized offerings. These strategies aim to boost sales, improve customer satisfaction, and maximize profitability.




