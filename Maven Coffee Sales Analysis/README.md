Pharmaceutical Supply Chain Management:

**Dataset Details:**
- 10 tables present in the dataset 
1. Brands
2. Calender
3. Categories
4. Customers
5. Customers_Type 
6. Products 
7. Sales_person 
8. Sales_team 
9. OrdersItem
10. Orders





### **Data Modeling:**

The dataset modeling links all Sheets and Tables.
<img width="592" alt="image" src="https://github.com/user-attachments/assets/ef8abb8d-9e90-4c52-8bd5-191e21ced99a">


### **DAX Measure:**
1. % of total order = DIVIDE(
    [Total Order Lines], 
    CALCULATE(
         [Total Order Lines], REMOVEFILTERS(dimCustomers[Name])
    )
)

2. IF = 
 VAR InFull = 
    COUNTX(
        factOrders,
    IF(
        factOrders[Fill rate] >= 100, 
        1,
        BLANK()
    )
)

RETURN
    DIVIDE(
        InFull, [Total Order Lines]
    )
3. IF% = DIVIDE([Total Order Lines Fully Filled],[Total Order Lines])

4. Lifr = AVERAGE(factOrders[Fill rate])

5. LIFR % = DIVIDE(SUM(factOrderItems[infull]), COUNTA(factOrderItems[infull]))

6.OT = 
VAR OnTime =
    COUNTX(
    factOrders, 
    IF(
        factOrders[On Time] == "True", 
        1, 
        BLANK()
    )
)

RETURN
    DIVIDE(
        OnTime, [Total Order Lines]
    )

7. OT value = COUNTX(
    factOrders, 
    IF(
        factOrders[On Time] == "True", 
        1, 
        BLANK()
    ))


8.OT% = DIVIDE([OT value],[Total Order Lines])

9. OTIF = 
VAR OnTimeInFull = 
    COUNTX(
    factOrders, 
    IF(
        factOrders[On Time] == "True" && factOrders[Fill rate] == 100, 
        1, 
        BLANK()
    )
)

RETURN
    DIVIDE(
        OnTimeInFull, [Total Order Lines]
    )
10. OTIF value = COUNTX(
    factOrders, 
    IF(
        factOrders[On Time] == "True" && factOrders[Fill rate] == 100, 
        1, 
        BLANK()
    )
)

11. OTIF% = DIVIDE([OTIF value],[Total Order Lines])
12.Total Order Line QTY = SUM(factOrders[Total Order Amount])
13. Total Order Lines = COUNTROWS(factors)

14. Total Order Lines Fully Filled = COUNTX(
    factOrders,
    IF(
        factOrders[Fill rate] >= 100, 
        1,
        BLANK()
    ))

15. Total Order QTY Delivered = 
    CALCULATE(
        SUM(
            'factOrderItems'[Quantity]),
            NOT(ISBLANK('factOrderItems'[Delivery Date])
            )
            )

16. VoFR % = DIVIDE(SUM(factOrderItems[delivery_quantity]), SUM(factOrderItems[Quantity]))
    


### **DashBoard And Visualization:**

## *Orders at a glance:*
- Location Based Order Analysis 
- Product-based order analysis 
- Top distributor Order analysis 
-  ‘On-time delivery (OT) %’, ‘In-full delivery (IF) %’, and OnTime in full (OTIF) %’ of the customer orders daily basis against the target service level set for each customer.

 <img width="286" alt="1 ph" src="https://github.com/user-attachments/assets/ced5a200-9bc1-4f6b-b9c5-145e0184799a">


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




