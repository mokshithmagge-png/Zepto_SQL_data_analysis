# Zepto_SQL_data_analysis
This real-world portfolio project provides a end-to-end data analysis workflow built on scraped e-commerce inventory data from Zepto, one of India’s leading quick-commerce platforms. Designed to mimic the responsibilities of an industry data analyst, the project transitions raw product data into high-value business strategy.
🛒 Zepto Quick-Commerce Inventory Analysis using SQL📌 Project OverviewThis project simulates a real-world e-commerce data analyst workflow using SQL. Sourced from Kaggle via web-scraped Zepto product listings, the dataset reflects the complexities of actual quick-commerce inventory catalog management.Each record represents a unique Stock Keeping Unit (SKU). To mirror real catalog structures, identical product names appear multiple times across different package sizes, weights, and categories to maximize app visibility.The analysis progresses end-to-end through:Database Setup & Data IngestionExploratory Data Analysis (EDA)Data Cleaning & Currency NormalizationBusiness-Driven Analytical Querying🗂️ Database SchemaThe table structure was designed in PostgreSQL to handle high-precision financial data, stock flags, and unit dimensions.ColumnData TypeDescriptionsku_idSERIAL PRIMARY KEYSynthetic primary key for unique SKU trackingnameVARCHAR(150)Product name displayed in the mobile applicationcategoryVARCHAR(120)Broad product category (e.g., Fruits, Snacks, Beverages)mrpNUMERIC(8,2)Maximum Retail PricediscountPercentNUMERIC(5,2)Applied discount rate (%)discountedSellingPriceNUMERIC(8,2)Final selling price after discountavailableQuantityINTEGERCurrent inventory countweightInGmsINTEGERIndividual unit weight in gramsoutOfStockBOOLEANReal-time stock status flag (TRUE / FALSE)quantityINTEGERUnits per package / item count🔧 Execution Workflow┌──────────────────────┐    ┌──────────────────────┐    ┌──────────────────────┐    ┌──────────────────────┐
│ 1. Schema Creation   │───>│ 2. Data Ingestion    │───>│ 3. Exploratory Data  │───>│ 4. Data Cleaning &   │
│    & Data Types      │    │    & Encoding Fixes  │    │    Analysis (EDA)    │    │    Normalization     │
└──────────────────────┘    └──────────────────────┘    └──────────────────────┘    └──────────────────────┘
                                                                                               │
                                                                                               ▼
                                                                                    ┌──────────────────────┐
                                                                                    │ 5. Business Insights │
                                                                                    │    & Strategic SQL   │
                                                                                    └──────────────────────┘
1. Database & Table InitializationSQLCREATE TABLE zepto (
  sku_id SERIAL PRIMARY KEY,
  category VARCHAR(120),
  name VARCHAR(150) NOT NULL,
  mrp NUMERIC(8,2),
  discountPercent NUMERIC(5,2),
  availableQuantity INTEGER,
  discountedSellingPrice NUMERIC(8,2),
  weightInGms INTEGER,
  outOfStock BOOLEAN,
  quantity INTEGER
);
2. Data Ingestion & EncodingData was loaded directly into PostgreSQL. To prevent character encoding issues (UTF-8 parsing errors), source files were normalized prior to ingestion via the PSQL bulk copy command:SQL\copy zepto(category, name, mrp, discountPercent, availableQuantity, discountedSellingPrice, weightInGms, outOfStock, quantity)
FROM 'data/zepto_v2.csv' 
WITH (FORMAT csv, HEADER true, DELIMITER ',', QUOTE '"', ENCODING 'UTF8');
🔍 Data Exploration (EDA) & Data CleaningExploratory ChecklistRecord Volume: Evaluated total row count and unique product catalog depth.Missing Value Audit: Identified NULL values across inventory counts, pricing fields, and product names.Category Breakdown: Mapped distinct categories and SKU distribution across departments.Stock Health Check: Calculated ratios of active vs. out-of-stock items.Duplicate SKU Detection: Discovered re-listed products varying by weight variant or package configuration.Data Cleaning ActionsInvalid Record Removal: Identified and dropped corrupted records where mrp or discountedSellingPrice equaled zero.Currency Normalization: Converted raw mrp and discountedSellingPrice values from paise to Indian Rupees (₹) to ensure standardization across business queries.📊 Strategic Business Questions AddressedTop Value Deals: Ranked top 10 products offering the highest percentage discounts.Out-of-Stock Revenue Loss: Identified high-MRP items currently out of stock to highlight supply chain gaps.Category Revenue Potential: Calculated total potential inventory value per product category based on selling price and stock availability.Low-Discount Premium Items: Filtered premium SKUs (MRP > ₹500) carrying low discount rates (< 5%).Category Discount Leadership: Identified top 5 categories offering the highest average overall discounts.Unit Value Analysis: Derived price-per-gram metrics to pinpoint cost-effective products for consumer-side positioning.Weight Bucket Segmentation: Categorized products into Low, Medium, and Bulk weight classes using conditional CASE statements.Logistics Weight Distribution: Measured total combined inventory weight per category to inform warehouse space allocation.🚀 Getting StartedPrerequisitesPostgreSQL 12+pgAdmin 4 or PSQL CLIGitInstallation & SetupClone the repository:Bashgit clone https://github.com/amlanmohanty/zepto-SQL-data-analysis-project.git
cd zepto-SQL-data-analysis-project
Execute SQL scripts:Open zepto_SQL_data_analysis.sql inside your PostgreSQL client.Run schema creation, ingestion, cleaning, and analysis queries sequentially.
