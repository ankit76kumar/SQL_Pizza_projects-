
# 🍕 Pizza Sales SQL Analysis Project

This project demonstrates the use of **SQL queries** to analyze pizza sales data using datasets like `orders.csv` and `pizza_types.csv`. The goal is to extract insights such as total orders, top-selling pizzas, revenue generation, and customer order patterns.

---

## 👨‍💻 About Me

Hello!  
I'm **Ankit Kumar**, and this project helped me apply and reinforce my SQL skills through a practical business dataset involving **pizza sales**. It boosted my confidence in handling real-world data problems.

---

## 🎯 Objective

- Analyze pizza sales using SQL.
- Extract key business metrics like revenue, order count, top products, etc.
- Practice joins, aggregations, sorting, and window functions.

---

## 📊 Datasets Used

- `orders.csv`: Contains order ID, pizza type, quantity, order timestamp, etc.
- `pizza_types.csv`: Contains pizza category, ingredients, and prices.

---

## 🔍 SQL Query List & Explanation

### 1. 🔢 Total Number of Orders Placed
```sql
SELECT COUNT(DISTINCT order_id) AS total_orders
FROM orders;
````

---

### 2. 💰 Calculate Total Revenue

```sql
SELECT ROUND(SUM(o.quantity * pt.price), 2) AS total_revenue
FROM orders o
JOIN pizza_types pt ON o.pizza_id = pt.pizza_id;
```

---

### 3. 🔼 Identify Highest Priced Pizza

```sql
SELECT name, category, price
FROM pizza_types
ORDER BY price DESC
LIMIT 1;
```

---

### 4. 🏆 Top 5 Most Ordered Pizza Types (by Quantity)

```sql
SELECT o.pizza_id, pt.name, SUM(o.quantity) AS total_quantity
FROM orders o
JOIN pizza_types pt ON o.pizza_id = pt.pizza_id
GROUP BY o.pizza_id, pt.name
ORDER BY total_quantity DESC
LIMIT 5;
```

---

### 5. 🧾 Top 3 Most Ordered Pizza Types by Revenue (per Category)

```sql
SELECT category, name, revenue
FROM (
  SELECT pt.category, pt.name,
         SUM(o.quantity * pt.price) AS revenue,
         RANK() OVER(PARTITION BY pt.category ORDER BY SUM(o.quantity * pt.price) DESC) AS rank
  FROM orders o
  JOIN pizza_types pt ON o.pizza_id = pt.pizza_id
  GROUP BY pt.category, pt.name
) ranked
WHERE rank <= 3;
```

---

### 6. 📊 Percentage Contribution of Each Pizza to Total Revenue

```sql
SELECT pt.name,
       ROUND(SUM(o.quantity * pt.price), 2) AS revenue,
       ROUND(SUM(o.quantity * pt.price) * 100.0 /
             (SELECT SUM(o.quantity * pt.price)
              FROM orders o JOIN pizza_types pt ON o.pizza_id = pt.pizza_id), 2) AS percentage
FROM orders o
JOIN pizza_types pt ON o.pizza_id = pt.pizza_id
GROUP BY pt.name
ORDER BY percentage DESC;
```

---

### 7. ⏰ Order Distribution by Hour

```sql
SELECT EXTRACT(HOUR FROM order_date) AS hour, COUNT(*) AS total_orders
FROM orders
GROUP BY hour
ORDER BY hour;
```

---

### 8. 🍕 Total Quantity Ordered by Pizza Category

```sql
SELECT pt.category, SUM(o.quantity) AS total_quantity
FROM orders o
JOIN pizza_types pt ON o.pizza_id = pt.pizza_id
GROUP BY pt.category
ORDER BY total_quantity DESC;
```

---

## 📁 Project Structure

```
pizza-sales-sql-project/
│
├── orders.csv                     # Orders dataset
├── pizza_types.csv                # Pizza types, price & ingredients
├── pizza_sales_queries.sql        # All SQL queries in a single file
├── README.md                      # Project documentation
├── requirements.txt               # Optional: Python SQL libraries
└── sample_dashboard.png           # Optional: dashboard or screenshots
```

---

## 🔮 Future Enhancements

* Visualize results using Power BI / Tableau.
* Create a Flask/Streamlit dashboard for dynamic exploration.
* Export query results to CSV/Excel for reporting.
* Apply advanced SQL (CTEs, window functions, etc.) for deeper insights.

---

## 🧾 requirements.txt

```txt
pandas
sqlalchemy
sqlite3   # If you're using SQLite, this comes with Python
```

---

## 👨‍💻 Author

**Ankit Kumar**
🎓 B.Tech – Artificial Intelligence & Data Science
🏫 Truba Institute of Engineering and Information Technology
📍 Bhopal, Madhya Pradesh

---

## 📜 License

This project is open-source under the **MIT License**.

---

⭐ Star this repository if you found it helpful or use it in your own learning!


