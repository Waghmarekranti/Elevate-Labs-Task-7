# Elevate-Labs-Task-7

# Title : TASK 7: Get Basic Sales Summary from a Tiny SQLite Database using Python


Step 1: Import Required Libraries
import sqlite3
import pandas as pd
import matplotlib.pyplot as plt


sqlite3 → Python built-in module to work with SQLite databases

pandas → For loading SQL query results into DataFrame and data manipulation

matplotlib.pyplot → For plotting charts

Step 2: Connect to SQLite Database
conn = sqlite3.connect("sales_data.db")
cur = conn.cursor()


sales_data.db नावाचा database file तयार होतो (जर आधी नसेल तर)

conn → database connection object

cur → cursor object used to execute SQL commands

Important: Database connection must remain open while running SQL queries.

Step 3: Create Sales Table
cur.execute("""
CREATE TABLE IF NOT EXISTS sales (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    order_date TEXT,
    product TEXT,
    quantity INTEGER,
    price REAL
)
""")


CREATE TABLE IF NOT EXISTS → Table create only if it does not exist

Columns:

id → Auto-increment primary key

order_date → Date of order

product → Product name

quantity → Quantity sold

price → Price per unit

Step 4: Insert Sample Data
sample_data = [
    ("2025-11-01", "Notebook", 10, 2.5),
    ("2025-11-02", "Pen", 50, 0.5),
    ("2025-11-02", "Notebook", 5, 2.5),
    ("2025-11-03", "Pencil", 30, 0.2),
    ("2025-11-04", "Pen", 20, 0.5),
    ("2025-11-05", "Eraser", 15, 0.3),
    ("2025-11-06", "Notebook", 8, 2.5),
    ("2025-11-07", "Marker", 12, 1.0),
    ("2025-11-08", "Highlighter", 10, 1.5),
    ("2025-11-09", "Notebook", 7, 2.5)
]

cur.execute("DELETE FROM sales;")  # Optional: Clear table before inserting
cur.executemany("INSERT INTO sales (order_date, product, quantity, price) VALUES (?, ?, ?, ?)", sample_data)
conn.commit()


DELETE FROM sales → Clear old data (optional)

executemany → Insert multiple rows at once

conn.commit() → Save changes to database

Step 5: Run SQL Query to Get Summary
query = """
SELECT product, SUM(quantity) AS total_qty, SUM(quantity * price) AS revenue
FROM sales
GROUP BY product
"""
df = pd.read_sql_query(query, conn)


SUM(quantity) → Total quantity sold per product
SUM(quantity * price) → Total revenue per product
GROUP BY product → Aggregate by product

pd.read_sql_query → Load result directly into Pandas DataFrame

Step 6: Print Results
print("Sales Summary by Product:")
print(df)


Example Output:

product	total_qty	revenue
Notebook	30	75.0
Pen	70	35.0
Pencil	30	6.0
Eraser	15	4.5
Marker	12	12.0
Highlighter	10	15.0

Easy to check total quantity and revenue per product

Step 7: Plot Bar Chart
df.plot(kind='bar', x='product', y='revenue', legend=False, color='skyblue')
plt.title("Product-wise Revenue")
plt.xlabel("Product")
plt.ylabel("Revenue")
plt.tight_layout()
plt.savefig("sales_chart.png")  # Save chart as PNG
plt.show()                      # Show chart inline


kind='bar' → Vertical bar chart
x='product' → X-axis labels
y='revenue' → Bar height
plt.tight_layout() → Prevent overlapping labels
plt.savefig() → Save chart as PNG file
plt.show() → Display chart

Step 8: Close Database Connection
conn.close()
print("Database connection closed.")


Always close connection after finishing work
Prevents Cannot operate on closed database error

Summary of Deliverables

Database: sales_data.db → Contains table + sample data
Python Script / Notebook: Full workflow above
Chart Image: sales_chart.png → Product-wise revenue bar chart
Printed Table: Shows total_qty and revenue per product

✅ Key Points / Notes

SQLite database does not need separate software; Python’s sqlite3 module handles it
Use Pandas to read SQL query results → easy for printing and plotting
Always compute revenue if dataset has only price and quantity
Charts can be saved and customized with Matplotlib
