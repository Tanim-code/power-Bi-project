AdventureWorks Sales Data Analysis and Describe this process Given below:
Dataset Details:
Sales data Table:
- Order Date: 2020 to 2022 Sales Data are present here by formate(mm/dd/yyyy).
- Stoke Date: 2020 to 2022 Stoke Data are presented here by format (mm/dd/yyyy) .
- Order Number: A 7-digit integral number uniquely assigned to each distinct product Order. 'SO' Serial Order Number.
- Customer Key: 5-digit numbers are presented Customer Key.
- Product Key : 3 digits of a number are presented product key.
- Territory Key: A number represent Territory key like (1 means USA) or else.
- Order Line item: An integer Number presented order line item.
- Order Quantity: An integer Number presented order line item.

Customer Lookup Table:
- Customer Key : Customer key Uses foreign key in this Table.
- Name : Customer Name are include here.
- Age: Customer age(integer Number).
- Marital Staus, Gender, income, profession and some personal information given here.

Categories lookup Table:
- Product Category Key: AN integer Unique Number are represent Product Key.
- Product Category name: 4 category product are present here.

Calender Lookup Table:
- Date: 2020 to 2022 date are present. Extract Date to convert Start of the month, start of the week, Year.
- Day name: Day name are present here. Extract this data and Create a column Weekday or weekend.

Return Product Data:
- Return date: Return Date Are include here formate(mm/dd/yyyy)
- Territory Key: This key use integer number and it is use as a foreign key.
- Return Quantity: Return Product Quantity are include by date by date.

Subcategories Lookup:
- Product Subcategory key: product subcategories key are the identitycal number.
- Subcategory Name: Subcategory name of every product.
- product Key: This key link the data Product table.

Territory Lookup:
- Territory Key: This key is the represent territory.
- Region: 10 Region are in this Area.
- Country: 6 country are this sales area.
- Continent: North America, Europe, Pacific are the 3 continents of sales Area.

Data PreProcessing Step:
        **Sales data Table:**
   
        1.  Create a function : 2020,2021,2022 Datasheet are same Column and Same data type Create a parameter function Using 
              power Query and Combined 3sheet in One.   
        2. Formate Data Type : Formatted data type in there Perfect type.
        
       **Product Lookup:**
       
        1. Changed Product Key as "int" Data Type.
        2. Remove product Description.
        3. Replace Product Size and Product Style value. if 0 Then it replace 'NA' .  
        4. Product SKU column Uses Text Before Delimiter (-) And Create a new column .  
 
       **Product Subcategories:** 
   
        1.Change Column Data Type Like. Subcategories Key(int), Subcategories Name(text).

       **Customer Lookup:**
        
         1. Calculate Birth age.
         2. Extracted Email Text before delimiter(.com)
         3. Added Conditional Column: 
             = Table.AddColumn(#"Renamed Columns", "Parents", each if [Total Children] >= 1 then "Yes" else "No")

### **Data Modeling:**
All The Sheet and Table are Link by the dataset modelling.
<img width="705" alt="1 Adventureworks Data modeling" src="https://github.com/Tanim-code/power-Bi-project/assets/86589317/fdf02ae2-5c02-4c88-947b-02b3ad809659">

     
