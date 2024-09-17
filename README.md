# Sales Analytics Dashboard in Power BI
The following project is a sales analytics dashboard created in Power BI to display key insights on customer behavior. The [E-Commerce Data Sd](https://www.kaggle.com/datasets/danttis/e-commerce-data-sd?select=payments.csv) dataset was used for this project, and includes extensive data on sale orders, products, and customers.

The dashboard can be viewed [here](https://github.com/n-anna49/sales_analytics_powerbi/blob/60cf3fa7167914b5269457d77ff00efc00c2d210/Power%20BI%20Sales%20Analytics%20Dashboard.pdf).

## Data Processing
In the Query Editor, rows with blank cells were removed and an inner join merge was performed on certain tables to make sure that any foreign key values that did not exist as primary keys in their respective tables were removed. 

For example, the customer_id column in the **orders** table is a foreign key of the customer_id primary key in the **customers** table. To make sure there were no customer_id values in **orders** that did not exist in **customers**, an inner join merge was performed in the **orders** table to remove those values.

## Data Visualization
### Model View
Relationships were created between the five tables, as seen in the image below. Note that bi-directional cross-filtering was implemented between **order** and **order_items** to enable certain filters for visualizations.
![Sales Analytics Model View](https://github.com/user-attachments/assets/36d18141-b1ed-470a-b306-2f62cd71da03)

### Table View
A number of columns were added to aid in the visualization process. In **customers**, two new columns were added: orders_per_customer and customer_type.

orders_per_customer fetches all orders purchased by a single customer.
```
orders_per_customer = COUNTROWS(RELATEDTABLE(orders))
```

customer_type checks the value of orders_per_customer, and if it's greater than 1, displays "Returning Customer", otherwise it displays "New Customer." This distinguishes customers that are first-time buyers from customers that have purchased more than a single order.
```
customer_type = IF(customers[orders_per_customer] > 1, "Returning Customer", "New Customer")
```

In **orders** two additional columns were also added: day_of_the_week_value and day_of_the_week.

day_of_the_week_value extracts the day of the week in the order_date column and represents it as an integer (1 represents Sunday, 2 represents Monday, etc.).
```
day_of_the_week_value = WEEKDAY(orders[order_date].[Date], 1)
```

day_of_the_week formats the day of the week to just a 3-character string (Sun, Mon, etc.). This column was sorted by the day_of_the_week_value column to tie the integer to its string. This will aid in plotting the line chart in the dashboard to make sure the days of the week are in the correct order.
```
day_of_the_week = FORMAT(orders[order_date], "ddd")
```

### Report View
Six visualizations were created for this dashboard.
- Card 1: the average payment a customer makes on a single order.
- Card 2: the average number of orders each customer purchases.
- Vertical Bar Chart: total sales by product category.
- Horizontal Bar Chart: total sales by customer type.
- Line Chart: total sales by day of the week.
- Table: display of the most popular products, along with their item price and total revenue.
