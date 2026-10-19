# capstone-project
Q: 1
Capstone Project — Mamaearth Returns & Growth Intelligence Pipeline
Program: Data Analytics with AI & Gen AI · E&ICT Academy IIT Roorkee Duration: 15 days · Total Marks: 100 · Submission: one public GitHub repository link

Scenario
You are a Data Analyst on Mamaearth's Growth Analytics team. The team suspects that returns are eating into margins on a subset of orders, but nobody has actually built the pipeline to prove it end-to-end: a relational store for order data, a clean analysis layer that finds and quantifies the real patterns, and an AI-generated narrative the regional ops and finance heads can act on without reading a notebook. You will build all three, wired into one connected repository — the SQL layer feeds the Python analysis layer, and the analysis layer's verified numbers feed the GenAI narrative layer. No layer may report a number it did not itself compute or receive from the layer before it.

Submission
One public GitHub repository. README.md is the front door: it must explain the repo structure below, the exact order to run the three layers in, and how output from each layer flows into the next. No PDF, slides, screenshots, video, or any file/link type other than the repo itself.

<repo>/
├── README.md
├── sql/
│   ├── schema.sql
│   ├── seed_data.sql
│   └── reports.sql
├── data/
│   ├── customers.csv
│   ├── products.csv
│   └── orders.csv
├── analysis/
│   ├── clean_and_eda.py
│   └── visualize.py
├── visualizations/
│   ├── return_rate_by_payment.png
│   └── monthly_revenue_trend.png
└── narrator/
    ├── findings.json
    └── generate_narrative.py
Every number quoted anywhere in this brief was produced by actually running SQLite and pandas against the exact seed data below — not estimated. Your own numbers must match these exactly (aggregate/statistical values); if they don't, your code has a bug, not the brief.

Provided data
Three CSV files, to be committed under data/ exactly as given and loaded into SQLite in Part 1. Do not edit these files — all cleaning happens in Part 2's Python code, never by hand-editing the source data.

data/customers.csv (45 rows)
customer_id,name,city,city_tier,signup_date,acquisition_source
C001,Aarav,Mumbai,1,2026-01-07,Organic
C002,Vivaan,Delhi,1,2026-02-27,Referral
C003,Aditi,Mumbai,1,2026-06-23,Organic
C004,Ananya,Lucknow,2,2026-01-23,Organic
C005,Ishaan,Mumbai,1,2026-01-08,Referral
C006,Diya,Delhi,1,2026-05-10,Organic
C007,Kabir,Lucknow,2,2026-02-20,Organic
C008,Meera,Lucknow,2,2026-04-18,Ad
C009,Rohan,Lucknow,2,2026-03-13,Social
C010,Sara,Mumbai,1,2026-02-10,Organic
C011,Arjun,Bangalore,1,2026-03-13,Referral
C012,Priya,Bangalore,1,2026-01-27,Referral
C013,Karan,Mumbai,1,2026-04-02,Social
C014,Neha,Lucknow,2,2026-03-09,Social
C015,Yash,Jaipur,2,2026-05-18,Referral
C016,Pooja,Jaipur,2,2026-01-21,Organic
C017,Rahul,Lucknow,2,2026-04-03,Organic
C018,Simran,Mumbai,1,2026-01-12,Organic
C019,Dev,Bangalore,1,2026-01-21,Social
C020,Tara,Mumbai,1,2026-04-08,Ad
C021,Aryan,Bangalore,1,2026-02-11,Ad
C022,Kavya,Delhi,1,2026-06-21,Ad
C023,Nikhil,Mumbai,1,2026-06-05,Organic
C024,Riya,Lucknow,2,2026-03-04,Referral
C025,Varun,Jaipur,2,2026-03-11,Social
C026,Isha,Lucknow,2,2026-02-26,Organic
C027,Manav,Mumbai,1,2026-02-28,Social
C028,Shreya,Bangalore,1,2026-04-13,Ad
C029,Tanish,Delhi,1,2026-05-26,Social
C030,Anika,Bangalore,1,2026-02-24,Organic
C031,Aditya,Jaipur,2,2026-06-14,Ad
C032,Naina,Bangalore,1,2026-02-05,Ad
C033,Siddharth,Lucknow,2,2026-05-18,Ad
C034,Zara,Lucknow,2,2026-04-20,Social
C035,Krishna,Jaipur,2,2026-04-03,Ad
C036,Aisha,Delhi,1,2026-05-11,Ad
C037,Rudra,Mumbai,1,2026-01-29,Referral
C038,Myra,Delhi,1,2026-06-24,Ad
C039,Om,Mumbai,1,2026-04-09,Ad
C040,Vanya,Jaipur,2,2026-05-16,Ad
C041,Ayaan,Lucknow,2,2026-01-03,Organic
C042,Sanya,Mumbai,1,2026-06-24,Social
C043,Reyansh,Bangalore,1,2026-06-14,Ad
C044,Aria,Bangalore,1,2026-04-22,Referral
C045,Vihaan,Mumbai,1,2026-03-09,Social
data/products.csv (16 rows)
product_id,product_name,category,price
P01,Onion Hair Oil,Haircare,349
P02,Onion Shampoo,Haircare,399
P03,Argan Hair Mask,Haircare,599
P04,Anti-Hairfall Serum,Haircare,649
P05,Rice Water Conditioner,Haircare,349
P06,Vitamin C Face Wash,Skincare,249
P07,Vitamin C Serum,Skincare,699
P08,Tea Tree Face Wash,Skincare,229
P09,Ubtan Face Mask,Skincare,299
P10,Rice Sunscreen SPF50,Skincare,449
P11,Milky Soft Baby Lotion,Babycare,299
P12,Baby Shampoo,Babycare,249
P13,Baby Massage Oil,Babycare,279
P14,Charcoal Deodorant,PersonalCare,199
P15,Tea Tree Body Wash,PersonalCare,349
P16,Aloe Vera Gel,PersonalCare,199
Prices are in Rupees (INR); no currency symbol is stored in the data, only in written reports.

data/orders.csv (180 rows)
Blank discount_pct / rating cells are genuinely missing values, not zeros — this is deliberate and is exactly what Part 2 asks you to clean. payment_method casing is also deliberately inconsistent (COD/cod, Card/card/CARD, UPI/upi) — also deliberate, also a Part 2 cleaning target. Five rows (O0176–O0180) are exact duplicates of five earlier rows (same customer, product, date, quantity, discount, payment method, rating and return status — only order_id differs), simulating a double-submit bug; that too is a Part 2 cleaning target, not a data error to "fix" by editing the CSV.

order_id,customer_id,product_id,order_date,quantity,discount_pct,payment_method,rating,returned
O0001,C012,P04,2026-03-18,2,10,upi,2,0
O0002,C024,P06,2026-05-16,1,30,UPI,1,0
O0003,C008,P12,2026-03-20,2,0,COD,1,1
O0004,C032,P03,2026-05-17,3,30,UPI,5,0
O0005,C011,P09,2026-06-05,1,10,Card,3,0
O0006,C026,P12,2026-05-13,1,0,Card,3,0
O0007,C002,P08,2026-01-02,1,10,COD,1,0
O0008,C003,P11,2026-03-02,1,10,COD,,0
O0009,C037,P16,2026-05-02,1,0,UPI,4,0
O0010,C023,P14,2026-01-14,1,0,Card,1,0
O0011,C026,P11,2026-01-28,25,0,COD,2,1
O0012,C028,P06,2026-03-05,1,0,UPI,1,1
O0013,C042,P01,2026-03-02,3,,COD,4,0
O0014,C004,P06,2026-04-10,1,30,cod,3,0
O0015,C028,P16,2026-03-17,1,,COD,3,1
O0016,C004,P02,2026-05-09,1,0,UPI,5,0
O0017,C006,P06,2026-01-18,1,,Card,5,0
O0018,C016,P02,2026-04-18,2,20,CARD,3,0
O0019,C014,P11,2026-04-12,1,30,COD,3,0
O0020,C005,P01,2026-05-25,1,10,UPI,5,1
O0021,C017,P05,2026-01-18,3,10,UPI,,0
O0022,C035,P10,2026-06-17,2,20,Card,1,0
O0023,C009,P09,2026-01-28,1,20,Card,5,0
O0024,C014,P11,2026-06-12,1,20,UPI,1,0
O0025,C006,P14,2026-01-12,2,20,COD,2,0
O0026,C029,P14,2026-01-29,1,10,COD,5,0
O0027,C003,P12,2026-02-07,1,20,card,1,1
O0028,C023,P07,2026-06-20,2,30,COD,5,0
O0029,C010,P08,2026-02-15,2,20,UPI,4,1
O0030,C043,P08,2026-06-29,1,,COD,2,0
O0031,C013,P15,2026-02-28,1,30,COD,3,0
O0032,C018,P03,2026-03-13,3,,Card,1,0
O0033,C008,P09,2026-03-09,1,20,COD,3,0
O0034,C028,P04,2026-05-28,1,30,COD,1,1
O0035,C034,P07,2026-01-18,1,20,UPI,1,0
O0036,C020,P10,2026-03-25,2,,Card,2,0
O0037,C027,P13,2026-02-14,2,0,Card,3,0
O0038,C019,P07,2026-05-29,1,30,Card,4,0
O0039,C044,P07,2026-02-13,1,20,Card,1,0
O0040,C016,P10,2026-02-20,1,30,cod,5,1
O0041,C005,P15,2026-06-11,1,30,Card,4,0
O0042,C026,P08,2026-06-26,1,0,COD,4,0
O0043,C015,P06,2026-06-28,2,10,Card,1,1
O0044,C030,P05,2026-06-20,2,20,Card,4,0
O0045,C040,P14,2026-05-21,2,30,Card,4,0
O0046,C017,P08,2026-03-12,2,10,UPI,3,0
O0047,C029,P03,2026-03-02,2,0,COD,2,1
O0048,C010,P08,2026-02-09,1,30,Card,3,1
O0049,C035,P15,2026-02-22,1,0,UPI,5,0
O0050,C025,P16,2026-04-01,1,30,COD,5,1
O0051,C035,P08,2026-03-11,1,20,Card,4,1
O0052,C011,P15,2026-06-09,3,0,CARD,1,0
O0053,C042,P14,2026-04-29,1,20,cod,,1
O0054,C030,P11,2026-04-08,1,30,COD,,0
O0055,C006,P16,2026-05-19,1,10,COD,1,0
O0056,C042,P02,2026-03-05,2,10,COD,2,1
O0057,C009,P16,2026-05-25,2,20,UPI,3,0
O0058,C011,P04,2026-02-11,2,0,UPI,3,0
O0059,C037,P13,2026-02-20,1,10,COD,1,0
O0060,C020,P04,2026-05-25,2,30,UPI,3,0
O0061,C005,P11,2026-04-18,1,20,UPI,4,0
O0062,C010,P14,2026-05-14,1,30,UPI,4,0
O0063,C028,P09,2026-03-04,1,30,UPI,2,1
O0064,C030,P13,2026-05-07,1,10,UPI,3,0
O0065,C017,P11,2026-06-02,1,0,Card,5,0
O0066,C013,P03,2026-04-15,1,30,card,4,0
O0067,C029,P01,2026-02-26,1,20,Card,4,0
O0068,C036,P12,2026-05-21,1,20,Card,3,0
O0069,C017,P08,2026-02-19,1,10,CARD,2,0
O0070,C014,P16,2026-05-31,1,20,UPI,1,0
O0071,C013,P10,2026-02-15,1,10,Card,3,0
O0072,C003,P02,2026-06-28,1,30,UPI,1,0
O0073,C001,P10,2026-04-23,1,20,Card,,0
O0074,C008,P03,2026-01-19,1,10,Card,2,0
O0075,C037,P10,2026-03-05,1,10,COD,5,0
O0076,C025,P15,2026-03-18,3,20,UPI,5,0
O0077,C040,P02,2026-01-26,2,10,UPI,3,0
O0078,C043,P03,2026-02-14,1,30,Card,4,0
O0079,C039,P16,2026-03-01,1,30,cod,,1
O0080,C044,P08,2026-06-10,3,,Card,4,0
O0081,C008,P08,2026-03-10,2,10,UPI,3,1
O0082,C039,P10,2026-04-30,1,,Card,5,0
O0083,C035,P16,2026-06-03,1,20,COD,5,0
O0084,C017,P01,2026-06-22,1,0,UPI,3,0
O0085,C037,P02,2026-02-14,2,,Card,2,0
O0086,C038,P14,2026-05-06,2,30,UPI,3,0
O0087,C021,P04,2026-03-26,2,30,Card,5,0
O0088,C003,P15,2026-03-06,1,30,Card,,0
O0089,C001,P15,2026-02-18,1,30,Card,4,0
O0090,C004,P07,2026-02-03,1,30,UPI,1,0
O0091,C002,P08,2026-03-21,2,0,Card,2,0
O0092,C008,P15,2026-06-15,3,20,upi,5,0
O0093,C018,P14,2026-05-01,2,30,COD,2,0
O0094,C039,P05,2026-03-12,2,20,UPI,5,0
O0095,C018,P01,2026-03-18,1,30,UPI,,0
O0096,C029,P16,2026-05-22,1,20,UPI,2,0
O0097,C016,P13,2026-04-16,1,30,COD,4,0
O0098,C043,P05,2026-01-10,30,0,COD,1,0
O0099,C029,P04,2026-04-27,1,10,COD,1,1
O0100,C031,P09,2026-06-27,1,20,Card,5,1
O0101,C025,P11,2026-05-05,2,0,UPI,2,1
O0102,C041,P10,2026-01-24,3,0,Card,4,1
O0103,C011,P10,2026-01-12,3,20,CARD,3,1
O0104,C028,P05,2026-04-16,1,10,Card,2,0
O0105,C006,P13,2026-03-03,2,10,card,4,0
O0106,C041,P09,2026-06-20,1,20,COD,5,0
O0107,C011,P03,2026-03-30,1,30,UPI,3,0
O0108,C030,P10,2026-04-09,1,30,UPI,5,0
O0109,C023,P10,2026-03-17,3,30,COD,3,0
O0110,C001,P02,2026-05-08,3,10,UPI,5,0
O0111,C023,P08,2026-06-08,2,10,COD,1,0
O0112,C041,P02,2026-04-23,1,10,COD,1,1
O0113,C019,P11,2026-02-14,2,20,COD,5,0
O0114,C033,P09,2026-03-07,2,20,UPI,3,0
O0115,C008,P15,2026-02-06,3,30,UPI,5,0
O0116,C024,P03,2026-01-04,2,20,COD,3,1
O0117,C038,P13,2026-04-06,2,0,COD,5,1
O0118,C036,P11,2026-02-26,3,30,card,,0
O0119,C042,P14,2026-01-12,1,30,UPI,1,0
O0120,C007,P08,2026-02-04,2,30,CARD,5,0
O0121,C010,P14,2026-05-06,2,20,Card,1,0
O0122,C024,P07,2026-03-02,1,20,UPI,5,1
O0123,C042,P12,2026-03-12,1,30,COD,1,1
O0124,C043,P07,2026-06-02,2,20,UPI,,1
O0125,C009,P07,2026-05-22,1,10,COD,3,1
O0126,C010,P01,2026-02-07,1,10,UPI,1,0
O0127,C043,P01,2026-04-02,1,20,UPI,1,0
O0128,C012,P09,2026-04-18,1,30,Card,4,0
O0129,C024,P04,2026-02-26,1,20,UPI,4,1
O0130,C042,P01,2026-05-03,1,0,UPI,4,0
O0131,C029,P03,2026-03-24,2,20,card,5,1
O0132,C041,P11,2026-06-02,1,30,Card,1,0
O0133,C008,P07,2026-02-28,1,30,Card,4,0
O0134,C007,P11,2026-06-20,1,30,COD,,0
O0135,C006,P03,2026-01-25,1,10,Card,5,0
O0136,C004,P11,2026-04-16,2,30,Card,1,0
O0137,C019,P10,2026-05-28,1,30,CARD,2,0
O0138,C007,P12,2026-04-05,2,10,COD,4,1
O0139,C036,P01,2026-06-18,2,10,UPI,3,0
O0140,C020,P11,2026-02-16,1,30,UPI,1,0
O0141,C010,P01,2026-05-16,1,20,COD,2,1
O0142,C024,P10,2026-05-26,2,10,Card,2,0
O0143,C040,P02,2026-03-11,2,30,Card,4,0
O0144,C018,P07,2026-01-30,2,30,card,5,0
O0145,C043,P10,2026-04-12,1,10,UPI,3,1
O0146,C014,P05,2026-03-16,2,30,Card,2,1
O0147,C009,P13,2026-02-28,1,20,Card,1,0
O0148,C026,P02,2026-04-28,1,30,UPI,5,0
O0149,C026,P14,2026-04-14,1,30,COD,,1
O0150,C006,P14,2026-03-04,2,0,Card,4,0
O0151,C020,P11,2026-02-13,1,10,COD,3,0
O0152,C023,P05,2026-02-07,1,,COD,1,1
O0153,C012,P16,2026-05-25,1,20,UPI,3,0
O0154,C010,P15,2026-04-24,1,0,CARD,3,0
O0155,C033,P03,2026-04-26,1,20,COD,1,1
O0156,C042,P03,2026-05-10,2,,Card,4,0
O0157,C037,P07,2026-05-02,1,30,card,1,0
O0158,C022,P03,2026-02-14,1,30,COD,5,0
O0159,C034,P06,2026-03-14,1,0,Card,3,0
O0160,C005,P11,2026-06-23,1,10,Card,3,0
O0161,C006,P05,2026-03-21,3,30,UPI,2,0
O0162,C039,P03,2026-04-07,1,10,Card,5,0
O0163,C006,P14,2026-01-05,1,10,Card,,0
O0164,C032,P07,2026-02-05,1,0,COD,,1
O0165,C035,P02,2026-06-08,2,10,COD,2,1
O0166,C040,P06,2026-01-12,2,10,Card,4,0
O0167,C040,P10,2026-04-25,2,30,COD,2,1
O0168,C024,P15,2026-03-14,1,30,UPI,,0
O0169,C013,P05,2026-01-14,2,,Card,5,0
O0170,C008,P10,2026-02-11,1,10,cod,4,0
O0171,C006,P08,2026-03-31,2,30,UPI,5,0
O0172,C024,P08,2026-01-21,1,0,Card,3,1
O0173,C010,P05,2026-05-01,1,30,Card,4,0
O0174,C040,P01,2026-01-05,3,30,COD,1,0
O0175,C019,P08,2026-01-13,1,30,COD,1,0
O0176,C032,P03,2026-05-17,3,30,UPI,5,0
O0177,C005,P15,2026-06-11,1,30,Card,4,0
O0178,C008,P08,2026-03-10,2,10,UPI,3,1
O0179,C010,P14,2026-05-06,2,20,Card,1,0
O0180,C020,P11,2026-02-13,1,10,COD,3,0
Part 1 — SQL Relational Layer & Reporting (30 marks)
Build the relational store in SQLite (or any SQL engine that supports the exact syntax below) and write the report queries against it.

Task 1 — Schema (sql/schema.sql) — 6 marks
Three tables, matching the taught CREATE TABLE syntax (column name, data type, NOT NULL, DEFAULT, PRIMARY KEY, FOREIGN KEY ... REFERENCES):

customers(customer_id VARCHAR(10) PRIMARY KEY, name VARCHAR(50) NOT NULL, city VARCHAR(50) NOT NULL, city_tier INT NOT NULL, signup_date DATE NOT NULL, acquisition_source VARCHAR(20) NOT NULL)
products(product_id VARCHAR(10) PRIMARY KEY, product_name VARCHAR(100) NOT NULL, category VARCHAR(30) NOT NULL, price DECIMAL(10,2) NOT NULL)
orders(order_id VARCHAR(10) PRIMARY KEY, customer_id VARCHAR(10) NOT NULL, product_id VARCHAR(10) NOT NULL, order_date DATE NOT NULL, quantity INT NOT NULL, discount_pct INT, payment_method VARCHAR(10) NOT NULL, rating INT, returned INT NOT NULL DEFAULT 0, FOREIGN KEY (customer_id) REFERENCES customers(customer_id), FOREIGN KEY (product_id) REFERENCES products(product_id))
Note discount_pct and rating deliberately have no NOT NULL — real orders can be un-rated or promo-code-free, so NULL is the correct representation, not an error to constrain away.

Task 2 — Load seed data (sql/seed_data.sql) — 4 marks
Load data/customers.csv, data/products.csv, data/orders.csv into the three tables (via INSERT statements generated from the CSVs, or your engine's CSV import — either is acceptable as long as seed_data.sql or an equivalent loader script is committed and re-runnable). After loading:

SELECT COUNT(*) FROM customers; → 45
SELECT COUNT(*) FROM products; → 16
SELECT COUNT(*) FROM orders; → 180
If you load via SQLite's .import CLI instead of hand-generated INSERT statements: .import has no built-in way to turn a blank CSV cell into SQL NULL — it loads every blank discount_pct/rating cell as an empty string, not NULL (confirm this yourself with SELECT typeof(rating) FROM orders; before moving on). Left uncorrected, this silently breaks Report (b) in Task 3, since COUNT(rating) then counts those empty strings as present. If you use .import, add this cleanup immediately after it, before running any report:

UPDATE orders SET discount_pct = NULL WHERE discount_pct = '';
UPDATE orders SET rating = NULL WHERE rating = '';
Task 3 — Reports (sql/reports.sql) — 20 marks
Write each query below, run it against the loaded (raw, uncleaned) data, and paste the exact output as a comment above each query in reports.sql.

a) Order totals — COUNT(*), total revenue, and average order value, where revenue per row is quantity * price * (1 - discount_pct/100) and NULL discount is treated as 0% (COALESCE). Joins orders to products. — 2 marks Expected: total_orders = 180, total_revenue = 99860.20, avg_order_value = 554.78.

b) COUNT(*) vs COUNT(column) — one query showing COUNT(*), COUNT(rating), and the difference, on orders. — 2 marks Expected: (180, 165, 15) — 15 orders have no rating yet.

c) LEFT JOIN with a genuine zero-match row — LEFT JOIN customers to orders, GROUP BY customer, HAVING COUNT(order_id) = 0, to find any customer with zero orders. Then write a second, independent query using NOT IN (SELECT DISTINCT customer_id FROM orders) that must return the same customer, confirming the LEFT JOIN result rather than trusting it blindly. — 3 marks Expected: both queries return exactly one row — C045, Vihaan.

d) GROUP BY + HAVING — join orders to customers, group by city, compute total_orders, returned_orders, and return_rate_pct (rounded to 1 decimal), then filter with HAVING return_rate_pct > 20, ordered by return_rate_pct DESC. — 3 marks Expected exactly 3 rows: Jaipur (19, 8, 42.1), Lucknow (49, 15, 30.6), Bangalore (33, 8, 24.2). (Mumbai at 17.9% and Delhi at 17.4% are correctly excluded.)

e) Ranking with ORDER BY + LIMIT/OFFSET — join orders, products, customers; group by customer; compute total_spend; order by total_spend DESC, customer_id ASC (the tie-break matters — state why in a one-line comment). Run it once with LIMIT 5 and once with LIMIT 3 OFFSET 2 to get ranks 3–5 without re-deriving the top 5. — 3 marks Expected top 5: C043 Reyansh 12920.00, C026 Isha 8371.60, C008 Meera 4564.60, C011 Arjun 4111.00, C042 Sanya 3785.00. The LIMIT 3 OFFSET 2 query must return exactly the last three of those five, in the same order.

f) Three-table JOIN with GROUP BY — join orders, products, customers (only the first two are strictly needed for this one, but confirm the three-way join compiles cleanly since Part 3 will need it); group by category; compute order_count and category_revenue; order by revenue descending. — 3 marks Expected: Haircare (54, 44956.10), Skincare (60, 27346.00), Babycare (30, 16805.00), PersonalCare (36, 10753.10).

g) LIKE pattern match — customers whose name starts with 'A'. — 2 marks Expected: exactly 10 rows.

h) DISTINCT — distinct acquisition_source values used across all customers. — 1 mark Expected: exactly 4 values — Ad, Organic, Referral, Social.

i) ALTER TABLE + UPDATE with CASE — add a loyalty_tier VARCHAR(10) column to customers, then a single UPDATE ... SET loyalty_tier = CASE WHEN city_tier = 1 THEN 'Gold' ELSE 'Silver' END (no WHERE clause — every row gets a value). — 1 mark Expected: SELECT loyalty_tier, COUNT(*) FROM customers GROUP BY loyalty_tier; → Gold, 28 and Silver, 17.

Part 2 — Python/Pandas Data Wrangling & EDA (40 marks)
All work in analysis/clean_and_eda.py, run against data/orders.csv, data/customers.csv, data/products.csv directly (not against the SQL database — Part 2 is an independent pandas pipeline over the same raw source files, so a grader can run Part 1 and Part 2 in either order). Print every intermediate result your acceptance criteria reference; do not just compute silently.

Task 1 — Load and inspect — 2 marks
Load the three CSVs with pd.read_csv. orders.shape must print (180, 9) before any cleaning.

Task 2 — Standardize payment_method casing — 4 marks
Apply .str.strip().str.upper() to payment_method. Before the fix, orders['payment_method'].unique() must show 7 distinct raw values (COD, cod, Card, card, CARD, UPI, upi). After the fix, exactly 3 remain, with counts CARD: 70, UPI: 55, COD: 55.

Task 3 — Remove duplicate orders — 4 marks
Using .duplicated(subset=[...], keep='first') on the natural key customer_id, product_id, order_date, quantity, discount_pct, payment_method, rating, returned (everything except order_id, which differs on the duplicated rows by design), detect and drop duplicates. Exactly 5 rows must be flagged and dropped; orders_clean.shape must be (175, 9) afterward. Print the 5 dropped order_id values — they must be O0176, O0177, O0178, O0179, O0180.

Task 4 — Impute missing values — 4 marks
discount_pct: fill NaN with 0 (business rule: no promo code applied). 12 rows affected (count this on the deduplicated frame, i.e. after Task 3).
rating: fill NaN with the median of the non-null ratings on the deduplicated frame. Print the median before imputing — it must be 3.0. 15 rows affected.
After imputing, orders_clean[['discount_pct','rating']].isnull().sum() must be {'discount_pct': 0, 'rating': 0}.
Task 5 — Merge and reconcile against Part 1 — 5 marks
Merge the cleaned orders with products (on product_id) and customers (on customer_id). Compute order_value = quantity * price * (1 - discount_pct/100) per row. Print the total: it must be 97358.30 across the 175 cleaned rows.

This is ₹2,501.90 less than Part 1 Report (a)'s raw total of ₹99,860.20. Write a one-paragraph reconciliation note (in the script's print output, not just a comment) that explicitly attributes this exact delta to the 5 duplicate rows removed in Task 3 (their combined order_value is ₹2,501.90 — sum the order_value of the 5 dropped rows independently as a check) — not to the discount/rating imputation, which does not change any order_value total. A note that just says "cleaning changed the numbers" without this specific attribution does not satisfy this task.

Task 6 — IQR outlier detection on quantity — 4 marks
Compute Q1, Q3, IQR = Q3 - Q1, lower = Q1 - 1.5*IQR, upper = Q3 + 1.5*IQR on the merged frame's quantity column. Expected: Q1 = 1.0, Q3 = 2.0, IQR = 1.0, lower = -0.5, upper = 3.5. Filter rows outside [lower, upper]: exactly 2 rows — O0011 (quantity 25) and O0098 (quantity 30). Do not drop these rows from the dataset; flag them (e.g. an is_outlier boolean column) for Task 9 to use.

Task 7 — Hypothesis: does COD have a higher return rate? — 4 marks
State the hypothesis explicitly in your script's output, then verify with groupby('payment_method')['returned'].agg(['count','mean']). Expected return rates (rounded to 1 decimal): CARD: 14.7%, COD: 44.4%, UPI: 18.9%. Label the hypothesis Confirmed.

Task 8 — Multi-level segmentation — 4 marks
Group by ['payment_method', 'city_tier'], compute return rate per segment. The single highest-risk segment must be identified explicitly in your output: COD + Tier-2 cities at 54.5% (32 Tier-1 COD orders at 37.5%, versus 22 Tier-2 COD orders at 54.5% — the segmentation must show COD risk is not uniform across tiers, mirroring how a single blended rate can hide where the real problem is concentrated).

Task 9 — Correlation analysis — 2 marks
Compute the correlation matrix across rating, returned, discount_pct, quantity. State explicitly which taught correlation-strength band (negligible / weak / moderate / strong, using the 0–0.19 / 0.2–0.39 / 0.4–0.69 / 0.7–1.0 bands) every pairwise value falls into. All six pairs must be labeled negligible (|r| < 0.2) — in particular, label the hypothesis "higher discounts reduce returns" Busted (discount_pct vs returned correlation ≈ -0.09).

Task 10 — Outlier-corrected time series — 4 marks
Convert order_date to datetime, extract year-month, compute monthly total order_value two ways: (1) including the two Task 6 outlier orders, and (2) excluding them. Print both series. Expected (1): 2026-01: 29582.10 (highest month), ..., 2026-06: 11615.40. Expected (2), outlier-corrected: 2026-01: 11637.10, 2026-02: 13195.50, 2026-03: 20318.90 (now the true highest month), 2026-04: 9495.30, 2026-05: 13151.10, 2026-06: 11615.40. State explicitly, in your printed output, that January's apparent lead is an artifact of the two bulk orders landing in January (O0011 on 2026-01-28, O0098 on 2026-01-10), and that March is the genuine peak month once they're excluded — this is the whole point of doing Task 6 before Task 10, not after.

Task 11 — Two visualizations (analysis/visualize.py) — 3 marks
Using Matplotlib, save two PNGs to visualizations/:

return_rate_by_payment.png — a bar chart of return rate by (cleaned) payment_method, descending, with the exact percentage labeled on each bar, and a title that states the finding (e.g. "COD Returns at 44.4% — 3x Card").
monthly_revenue_trend.png — a line chart of the outlier-corrected monthly revenue from Task 10 (not the outlier-inflated version), with axis labels and a title naming the actual peak month.
Both scripts must be re-runnable end-to-end (python analysis/clean_and_eda.py then python analysis/visualize.py) and regenerate identical numbers/files from the raw CSVs every time — no manual editing of intermediate output.

Part 3 — GenAI-Powered Insight Narrator (30 marks)
Everything in narrator/. This Part turns the Part 1 + Part 2 verified numbers into a business narrative using the Gemini API and the SCR (Situation–Complication–Resolution) structure — with a fully keyless fallback so the pipeline never depends on network access or a paid quota to be gradable.

Free-path statement (required by the brief, not optional): Google AI Studio issues a free Gemini API key with a free usage tier — use that, never a paid-only key. In addition, Task 4 below requires a fully offline, keyless fallback path, so the whole pipeline is gradable with zero API spend or network access if needed.

Task 1 — narrator/findings.json — 4 marks
A structured JSON file (not free text) capturing exactly these verified figures from Parts 1–2, keyed clearly (your own key names are fine as long as they're self-explanatory):

cleaned_total_revenue_inr: 97358.30
raw_total_revenue_inr: 99860.20
duplicate_reconciliation_delta_inr: 2501.90
return_rate_by_payment: {"COD": 44.4, "CARD": 14.7, "UPI": 18.9}
highest_risk_segment: {"payment_method": "COD", "city_tier": 2, "return_rate_pct": 54.5}
true_peak_month: {"month": "2026-03", "revenue_inr": 20318.90}
outlier_inflated_month: {"month": "2026-01", "apparent_revenue_inr": 29582.10, "corrected_revenue_inr": 11637.10}
This file must be written by a step at the end of analysis/clean_and_eda.py (or a small separate export step) — never hand-typed — so it can never silently drift from the Part 2 numbers it claims to report.

Task 2 — generate_scr_narrative(findings: dict) -> dict — 6 marks
A Python function in narrator/generate_narrative.py using the google-genai client library (from google import genai). It must:

Build a system instruction (not folded into the user prompt) that fixes the agent's role ("You are a senior data analyst writing for Mamaearth's regional ops and finance heads"), the required structure (three labeled sections: Situation, Complication, Resolution), and an explicit constraint that every number in the output must come from the supplied findings and appear with the same value — no invented statistics.
Build the user prompt (contents) from the findings dict — do not hardcode the numbers into the prompt string; interpolate them from the argument, so a different findings.json produces a different narrative without touching this function.
Call client.models.generate_content(...).
Task 3 — Parameter locking and error handling — 6 marks
temperature=0.0 (deterministic — this is a factual business report, not creative writing; state this reasoning in a comment).
max_output_tokens set to an explicit value (not left as the API default) — at least 300, enough for a 3-section, ~250-word narrative.
A timeout set on the request (any value ≥ 10 seconds, per the taught Gemini minimum).
The whole call wrapped in try/except, returning a structured dict in both branches: {"status": "success", "narrative": ..., "tokens": ...} on success, {"status": "error", "narrative": None, "message": str(err)} on failure — the caller must never receive a raw exception.
Task 4 — Offline fallback path — 5 marks
When no API key is configured (or the API call's status is "error"), the function must fall back to a second, fully deterministic function, generate_scr_narrative_offline(findings: dict) -> dict, that formats the same three SCR sections using an f-string/template built directly from findings — no network call, no API key, same return-dict shape, same key numbers required (Task 5). This path must run and produce output with zero environment configuration.

Task 5 — Numeric accuracy checklist — 5 marks
Whichever path produced the narrative (online or offline), the returned narrative text must contain, verbatim or with equivalent rounding, all five of these figures:

97,358.30 (or 97358.3) — the cleaned total revenue
44.4 — the COD return rate
54.5 — the COD + Tier-2 highest-risk segment return rate
2,501.90 (or 2501.9) — the duplicate-driven reconciliation delta
March together with 20,318.90 (or 20318.9) — the true peak month
Write a small checker function that asserts all five are present as substrings (after normalizing commas) and prints a pass/fail line per figure. For the offline path this is a hard pass/fail (the template guarantees it). For the online Gemini path, since LLM output is not byte-deterministic, run it at least once, paste the actual output into narrator/sample_output.txt, and run the checker against that saved output — the grader verifies the saved sample, not a live API call at grading time.

Task 6 — README pipeline write-up — 4 marks
README.md must state, in order: (1) how to load sql/schema.sql + sql/seed_data.sql and run sql/reports.sql; (2) how to run analysis/clean_and_eda.py then analysis/visualize.py, and that Task 5 of Part 2 writes narrator/findings.json; (3) how to run narrator/generate_narrative.py, including how to set a Gemini API key as an environment variable versus running with no key at all (the offline path). A reader who has never seen this repo must be able to reproduce every number in this brief by following the README top to bottom.

Grand total: 30 (Part 1) + 40 (Part 2) + 30 (Part 3) = 100 marks.
