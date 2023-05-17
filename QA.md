What are your risk areas? Identify and describe them.


Data of ecommerce database has so much messy so it is difficult to understand it.
In database too many columns has null values and duplicate values thus,it is hard to understand how to manage this.
when i remov null values i lost all data from tables many times.
 It is difficult to understand the excute query gives proper output or not.


QA Process:
Describe your QA process and include the SQL queries used to execute it.
(1) DATA PROFILING :: 
    This query return number of tables of ecommerce database

    SELECT *
    FROM information_schema.tables
    WHERE table_schema = 'public';

    To determine datatypes,nullvalues and unique values from each table

    SELECT column_name, data_type, is_nullable
    FROM information_schema.columns
    WHERE table_name = 'all_sessions';

     SELECT column_name, data_type, is_nullable
    FROM information_schema.columns
    WHERE table_name = 'analytics';

     SELECT column_name, data_type, is_nullable
    FROM information_schema.columns
    WHERE table_name = 'products';

     SELECT column_name, data_type, is_nullable
    FROM information_schema.columns
    WHERE table_name = 'sales_by_sku';

     SELECT column_name, data_type, is_nullable
    FROM information_schema.columns
    WHERE table_name = 'sales_report';

 For count unique values in tables
    
     SELECT DISTINCT COUNT(fullvisitorid)
                    FROM all_sessions

(2) DATA VALIDATION ::
    Check missing data from all tables of ecommerce database (Here i put query of analytics table)

    SELECT pageviews,units_sold,timeonsite,bounces,revenue
     FROM analytics
        WHERE pageviews IS NULL
        AND units_sold IS NULL 
		AND timeonsite IS NULL
		AND bounces IS NULL
		AND revenue IS NULL

    Check duplicate data from all tables (i put query of count duplicate values of fullvisitoreid)
     SELECT fullvisitorid,
         ROW_NUMBER() OVER(PARTITION BY fullvisitorid) AS visitor
        FROM all_sessions

    SELECT fullvisitorid,
    COUNT(fullvisitorid) as count
    FROM all_sessions
    GROUP BY fullvisitorid

(3) DATA CLEANING ::

    DELETE irrelevant columns from all_sessions and analytics tables

            ALTER TABLE all_Sessions 
        DROP COLUMN transactions,
        DROP COLUMN sessionqualitydim,
        DROP COLUMN productrefundamount,
        DROP COLUMN productquantity,
        DROP COLUMN productrevenue,
        DROP COLUMN itemquantity,
        DROP COLUMN itemrevenue,
        DROP COLUMN transactionrevenue,
        DROP COLUMN transactionid,
        DROP COLUMN searchkeyword,
        DROP COLUMN ecommerceaction_option,
        DROP COLUMN productvariant;

        ALTER TABLE analytics 
        DROP COLUMN userid

    Remove NULL values from tables and fill with AVG of data in each columns

    UPDATE analytics
    SET revenue = 81691284.246441612639
    WHERE revenue IS NULL;

     UPDATE analytics
     SET bounces = 1.00000000000000000000
     WHERE bounces IS NULL;

    UPDATE analytics
    SET timeonsite = 524.8416259996826575
    WHERE timeonsite IS NULL;

    UPDATE analytics
    SET units_sold = 4.8236270417721103
    WHERE units_sold IS NULL;


(4) DATA TESTING

    The  validation check verifies that the productsku column in the sales_by_sku table contains only valid integer values greater than 0.
    -- Validate the join between the tables

        SELECT COUNT(*)
        FROM analytics a
        JOIN all_sessions s ON a.fullvisitorid = s.fullvisitorid
        JOIN products p ON s.productsku = p.sku
        JOIN sales_report r ON p.sku = r.productsku
        JOIN sales_by_sku b ON p.sku = b.productsku;


    The  validation check ensures that the tables are properly joined on their respective keys and that all necessary columns exist.
    --Validate the COUNT function with DISTINCT

    SELECT COUNT(DISTINCT s.fullvisitorid)
    FROM all_sessions s
    JOIN analytics a ON s.fullvisitorid = a.fullvisitorid
    JOIN products p ON s.productsku = p.sku
    JOIN sales_report r ON p.sku = r.productsku
    JOIN sales_by_sku b ON p.sku = b.productsku;

-- Validate the COUNT function with DISTINCT

    SELECT COUNT(DISTINCT p.sku)
    FROM products p
    JOIN sales_report r ON p.sku = r.productsku
    JOIN sales_by_sku b ON p.sku = b.productsku
    JOIN all_sessions s ON p.sku = s.productsku;



-- Validate that the query only returns visitors who have ordered product


        SELECT COUNT(*)
        FROM (
        SELECT a.visitnumber, COUNT(r.total_ordered)
                 AS  NumofOrder
        FROM analytics a
        JOIN all_sessions s ON a.fullvisitorid = s.fullvisitorid
                JOIN products p ON s.productsku = p.sku
                JOIN sales_report r ON p.sku = r.productsku
                --JOIN sales_by_sku b ON p.sku = b.productsku
        GROUP BY a.visitnumber
        HAVING COUNT(DISTINCT r.total_ordered) > 1
        ) AS subquery;






   

