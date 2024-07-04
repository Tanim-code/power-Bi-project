AdventureWorks Sales Data Analysis and Describe this process Given below:

**Dataset Details:**

Sales data Table:
- Order Date: 2020 to 2022 Sales Data are present here by formate(mm/dd/yyyy).
- Stoke Date: 2020 to 2022 Stoke Data are presented here by format (mm/dd/yyyy) .
- Order Number: A 7-digit integral number uniquely assigned to each distinct product Order. 'SO' Serial Order Number.
- Customer Key: 5-digit numbers are presented Customer Key.
- Product Key : 3 digits of a number are presented product key.
- Territory Key: A number represents Territory key like (1 means USA) or else.
- Order Line item: An integer Number presented order line item.
- Order Quantity: An integer Number presented order line item.

Customer Lookup Table:
- Customer Key: Customer key Uses a foreign key in this Table.
- Name: Customer Names are included here.
- Age: Customer age(integer Number).
- Marital Status, Gender, income, profession, and some personal information are given here.

Categories lookup Table:
- Product Category Key: AN integer Unique Number represents the Product Key.
- Product Category name: 4 category products are present here.

Calendar Lookup Table:
- Date: 2020 to 2022 date are present. Extract the Date to convert the Start of the month, the start of the week, and Year.
- Day name: Day names are present here. Extract this data and Create a column for Weekdays or weekends.

Return Product Data:
- Return date: Return Date Are included here format (mm/dd/yyyy)
- Territory Key: This key uses integer numbers and is a foreign key.
- Return Quantity: Return Product Quantity is included by date by date.

Subcategories Lookup:
- Product Subcategory key: The product subcategory key is the identical number.
- Subcategory Name: Subcategory name of every product.
- product Key: This key links the data Product table.

Territory Lookup:
- Territory Key: This key represents territory.
- Region: 10 Regions are in this Area.
- Country: 6 countries are this sales area.
- Continent: North America, Europe, Pacific are the 3 continents of sales Area.

**Data PreProcessing Step:**

        Sales data Table:
   
        1.  Create a function: 2020,2021,2022 Datasheets are the same Column and Same data type Create a parameter function 
            Using  power Query and Combined 3sheet in One.   
        2. Formate Data Type: Formatted data type in their Perfect type.
        
       Product Lookup:
       
        1. Changed Product Key as "int" Data Type.
        2. Remove Product Description.
        3. Replace Product Size and Product Style value. if 0 Then it replaces 'NA'.  
        4. The Product SKU column Uses Text Before Delimiter (-) And Creates a new column.  
 
       Product Subcategories: 
   
        1. Change Column Data Type Like. Subcategories Key(int), Subcategories Name(text).

       Customer Lookup:
        
         1. Calculate Birth age.
         2. Extracted Email Text before delimiter(.com)
         3. Added Conditional Column: 
             = Table.AddColumn(#"Renamed Columns", "Parents", each if [Total Children] >= 1 then "Yes" else "No")

### **Data Modeling:**

The dataset modeling links all Sheets and Tables.
<img width="705" alt="1 Adventureworks Data modeling" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/fdf02ae2-5c02-4c88-947b-02b3ad809659">

### **DAX Measure:**
1. Revenue = 
    SUMX(
        'Sales Data',
        'Sales Data'[Order Quantity]
        * RELATED('Product Lookup'[Product Price])
         )

2. Total Cost = 
    SUMX(
        'Sales Data',
        'Sales Data'[Order Quantity]
        *RELATED('Product Lookup'[Product Cost])
          )

3. Total Customer = DISTINCT COUNT('Sales Data'[CustomerKey])

4. Total Order = DISTINCT COUNT('Sales Data'[Order Number])

5. Total Return = COUNTROWS('Returns Data')

6. Weekend Order = 
    CALCULATE(
        [Total Order],
        'Calendar Lookup'[week/end]="Weekend")

7. previous month Order = 
    CALCULATE(
        [Total Order],
        DATEADD(
            'Calendar Lookup'[Date],
            -1,
            MONTH))

8. previous month Profit = 
    CALCULATE(
        [profit],
        DATEADD(
            'Calendar Lookup'[Date],
            -1,
            MONTH))

9. Previous month Quantity Sold = 
    CALCULATE(
        [Quantity Sold],
        DATEADD(
            'Calendar Lookup'[Date],
            -1,
            MONTH)
    )

10. Previous month Return Quantity = 
    CALCULATE(
        [Return Quantity],
        DATEADD(
            'Calendar Lookup'[Date],
            -1,
            MONTH))

11. previous month revenue = 
    CALCULATE(
        [Revenue],
        DATEADD(
            'Calendar Lookup'[Start of Month],
            -1,
            MONTH))
12. profit = 
    [Revenue]-[Total Cost]

13. profit gap target = [profit]-[Target profit]

14. Quantity Sold = SUM('Sales Data'[Order Quantity] )

15. Return Quantity = SUM('Returns Data'[ReturnQuantity])

16. Return Rate = DIVIDE([Return Quantity],[Quantity Sold])

17. Revenue gap target = [Revenue]-[target Revenue]

18. Target Order = [previour m Order]*1.1

19. Target profit = [previous m Profit]*1.1

20. target Revenue = [previous month revenue]*1.1

21. order gap = [Total Order]-[Target Order]

22. Bike Sold = 
    CALCULATE([Quantity Sold],'Categories Lookup'[Category Name]="Bikes")

23. Bike Return = 
    CALCULATE(
        [Return Quantity],
        'Categories Lookup'[Category Name]="Bikes")

24. Avg Revenue per Customar = DIVIDE([Revenue],[Total Customar])

25. avg retail price = AVERAGE('Product Lookup'[Product Price])

26. Adjusted Revenue = 
    SUMX(
        'Sales Data',
        'Sales Data'[Order Quantity]*
        [Adjusted Price])

27. Adjusted profit = [Adjusted Revenue]-[Total Cost]

28. Adjusted Price = [avg retail price]* (1 +'Adjusted price %'[Adjasted price % Value])

29. 90 day Rolling profit = 
    CALCULATE(
        [profit],
        DATESINPERIOD(
            'Calendar Lookup'[Date],
            LASTDATE('Calendar Lookup'[Date]),
            -90,
            DAY))
30. %weekend orders = DIVIDE([Weekend Order],[Total Order],"NA")

31. %Bike return = DIVIDE([Bike Return],[Return Quantity],"No Returns")

32. Adjusted price % Value = SELECTEDVALUE('Adjusted price %'[Adjasted price %], 0)

### **DashBoard And Visualization:**

- Revenue, Total Order, Return Rate, Profit. 
- Revenue prediction.
- Revenue, profit, order comparison By previous Month.
- Most Order product and Most profit product findings.
   <img width="741" alt="2 Adventureworks" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/0e9208db-d54c-44db-ba6f-ce700f56cf7c">

- Total order By country.
- Total Order by Continent.
<img width="743" alt="3 Adventureworks" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/45e05e47-42ba-49c0-959d-56dca1537577">

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


