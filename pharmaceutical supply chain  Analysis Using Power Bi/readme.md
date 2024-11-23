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

## *Orders at a glance*
- Location Based Order Analysis 
- Product-based order analysis 
- Top distributor Order analysis 
-  ‘On-time delivery (OT) %’, ‘In-full delivery (IF) %’, and OnTime in full (OTIF) %’ of the customer orders daily basis against the target service level set for each customer.

 <img width="286" alt="1 ph" src="https://github.com/user-attachments/assets/ced5a200-9bc1-4f6b-b9c5-145e0184799a">


## *Service level analysis*
- Total order By country.
- Total Order by Continent.



**Product Details:**
- Adjusted Price.
- different trends of product product, revenue, return rate, return quantity.
- Monthly Order, revenue, profit VS Target.
  <img width="740" alt="4 Adventureworks" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/c87d2abe-1233-4e1a-a742-704c44c57c2f">

**Customer details:**
- Top 50 customer performance.
- Top customer Highest Revenue.
- Comparison Trend Total customer and Average revenue per customer.
 <img width="743" alt="5 Advantureworks" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/ea4f76fb-9cfc-4cb1-8154-3ca38eec6ad5">

## **Project Impact:**
This project focused on analyzing Adventure Works' data from 2020 to 2022. The primary objective was to derive actionable insights that stakeholders can utilize to make informed, data-driven decisions aimed at enhancing and optimizing marketing strategies, as well as maximizing revenue generation.



