# 🍽️ MySQL Data Analyst Portfolio Project – Restaurant Menu Analysis

![SQL Banner](https://user-images.githubusercontent.com/your-banner-image.png)

## 📊 Project Overview

This project showcases my **MySQL data analysis skills** using a simulated restaurant database. The goal was to analyze the **newly launched menu**, understand **customer ordering behavior**, and provide **actionable insights** to optimize operations and offerings.

---

## 🧠 Objectives

### 🔄 Restart Operations Analysis

The restaurant launched a new menu and started collecting order details. This analysis helps understand:

- The diversity and pricing of the new menu
- Order trends and customer preferences
- Revenue patterns and spending behavior

---

## 📁 Dataset Used

- `menu_items`: Contains the list of food items with their categories and prices.
- `order_details`: Records each order placed with corresponding items and timestamps.

---

## 🧪 Objective 1: Explore the `menu_items` Table

> Understanding what's on the menu and how items are priced.

**Tasks Performed:**
1. Viewed the full menu
2. Counted total menu items
3. Identified the least and most expensive items
4. Counted number of Italian dishes
5. Found the cheapest and most expensive Italian dishes
6. Counted items per category
7. Calculated average price per category

**Sample SQL Query:**
```sql
SELECT category, COUNT(*) AS item_count, AVG(price) AS avg_price
FROM menu_items
GROUP BY category;


# 📊 Objective 2: Analyze the `order_details` Table

This objective focuses on exploring the `order_details` table to understand customer order activity and transaction patterns. It helps answer questions about order frequency, volume, and purchasing behavior.

---

## 🧾 Tasks Performed

### 1. View the `order_details` Table
- Displayed all rows and structure of the table to understand what data has been collected.
```sql
SELECT * FROM order_details LIMIT 10;
```

---

### 2. What is the Date Range of the Table?
- Identified the earliest and latest dates in the dataset.
```sql
SELECT MIN(order_date) AS start_date, MAX(order_date) AS end_date FROM order_details;
```

---

### 3. How Many Orders Were Made Within This Date Range?
- Counted unique order IDs to get total number of individual orders.
```sql
SELECT COUNT(DISTINCT order_id) AS total_orders FROM order_details;
```

---

### 4. How Many Items Were Ordered Within This Date Range?
- Counted total number of rows to represent total items ordered.
```sql
SELECT COUNT(*) AS total_items_ordered FROM order_details;
```

---

### 5. Which Orders Had the Most Number of Items?
- Grouped items by order ID and sorted in descending order to find largest orders.
```sql
SELECT order_id, COUNT(*) AS item_count
FROM order_details
GROUP BY order_id
ORDER BY item_count DESC
LIMIT 5;
```

---

### 6. How Many Orders Had More Than 12 Items?
- Filtered to find orders with more than 12 items.
```sql
SELECT COUNT(*) AS large_orders
FROM (
    SELECT order_id
    FROM order_details
    GROUP BY order_id
    HAVING COUNT(*) > 12
) AS subquery;
```

---

## 📈 Insights

- The dataset covers a clear period, allowing time-based analysis.
- Several large orders with more than 12 items were identified, likely group orders.
- Helps identify peak order behavior and customer purchase volume.

---

## 🧰 Tools Used

- **MySQL** – For querying and data analysis

# 🔗 Objective 3: Analyze Combined Data from `menu_items` and `order_details`

This objective aims to analyze how customers are interacting with the new menu by combining `menu_items` and `order_details` tables. By merging both datasets, we gain a clearer understanding of item popularity, revenue-generating orders, and overall customer preferences.

---

## 🧾 Tasks Performed

### 1. Combine `menu_items` and `order_details` Tables
- Performed an inner join to combine item details with order records.
```sql
SELECT od.order_id, mi.item_name, mi.category, mi.price
FROM order_details od
JOIN menu_items mi ON od.item_id = mi.item_id;
```

---

### 2. What Were the Least and Most Ordered Items? What Categories Were They In?
- Counted each item ordered and sorted to identify top and bottom performers.
```sql
SELECT mi.item_name, mi.category, COUNT(*) AS order_count
FROM order_details od
JOIN menu_items mi ON od.item_id = mi.item_id
GROUP BY mi.item_name, mi.category
ORDER BY order_count DESC;  -- or ASC for least ordered
```

---

### 3. What Were the Top 5 Orders That Spent the Most Money?
- Summed item prices per order to find high-value transactions.
```sql
SELECT od.order_id, SUM(mi.price) AS total_spent
FROM order_details od
JOIN menu_items mi ON od.item_id = mi.item_id
GROUP BY od.order_id
ORDER BY total_spent DESC
LIMIT 5;
```

---

### 4. View the Details of the Highest Spend Order
- Displayed detailed breakdown of the top order.
```sql
SELECT od.order_id, mi.item_name, mi.category, mi.price
FROM order_details od
JOIN menu_items mi ON od.item_id = mi.item_id
WHERE od.order_id = (
    SELECT order_id
    FROM order_details od
    JOIN menu_items mi ON od.item_id = mi.item_id
    GROUP BY order_id
    ORDER BY SUM(mi.price) DESC
    LIMIT 1
);
```

---

### 5. View the Details of the Top 5 Highest Spend Orders
- Gathered item-level details of top 5 high-value orders.
```sql
SELECT od.order_id, mi.item_name, mi.category, mi.price
FROM order_details od
JOIN menu_items mi ON od.item_id = mi.item_id
WHERE od.order_id IN (
    SELECT order_id
    FROM order_details od
    JOIN menu_items mi ON od.item_id = mi.item_id
    GROUP BY order_id
    ORDER BY SUM(mi.price) DESC
    LIMIT 5
);
```

---

## 📈 Insights

- The most ordered items typically came from the Italian and Appetizer categories.
- The least ordered items were often expensive seafood dishes or niche items.
- High-spending orders usually included a mix of main courses and beverages.
- This analysis helps identify popular combinations and bundling opportunities.

---


