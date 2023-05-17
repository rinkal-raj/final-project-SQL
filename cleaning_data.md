What issues will you address by cleaning the data?
    When i deleted duplicate values from tables i lost all data from tables many times.
    It was difficult update NULL values from all cloumns.
    In analytics table, i deleted duplicate value of visitnumber i get only 3 numbers of data and also when i deleted fullvisitorid duplicate data i lost many number of data. i didn't get accurate result.

Queries:
Below, provide the SQL queries you used to clean your data.
# Find duplicate values from table
    SELECT fullvisitorid,
    COUNT(fullvisitorid) as count
    FROM all_sessions
    GROUP BY fullvisitorid

    SELECT fullvisitorid,
    ROW_NUMBER() OVER(PARTITION BY fullvisitorid) AS visitor
    FROM all_sessions

# Remove duplicate value from table
For all_sessions table::

    DELETE FROM all_sessions 
    WHERE fullvisitorid IN
    (SELECT fullvisitorid
    FROM 
    (SELECT fullvisitorid,
    ROW_NUMBER() OVER( PARTITION BY fullvisitorid
    ORDER BY  fullvisitorid ) AS row_num
    FROM all_sessions ) t
    WHERE t.row_num > 1 );

For analytics table:

    ALTER TABLE analytics RENAME TO analytics_old;
    CREATE TABLE analytics AS SELECT DISTINCT * FROM analytics_old;

# Make price column data accurate

    SET unit_price = unit_price /100000
    ALTER TABLE analytics ALTER COLUMN unit_price TYPE numeric(10,2);

    UPDATE all_sessions 
    SET productprice = productprice /100000

# Drop irrelevant columns
    ALTER TABLE all_Sessions 
    DROP COLUMN transactions,
    DROP COLUMN sessionqualitydim,
    DROP COLUMN productrefundamount,
    DROP COLUMN productquantity,
    DROP COLUMN productrevenue,
    DROP COLUMN totaltransactionrevenue,
    DROP COLUMN itemquantity,
    DROP COLUMN itemrevenue,
    DROP COLUMN transactionid,
    DROP COLUMN searchkeyword,
    DROP COLUMN ecommerceaction_option,
    DROP COLUMN productvariant;


    ALTER TABLE analytics 
    DROP COLUMN userid

# Change datatypes 
    ALTER TABLE sales_report ALTER COLUMN ratio TYPE numeric(10,2);

    ALTER TABLE all_sessions
    ALTER COLUMN city TYPE VARCHAR(50),
    ALTER COLUMN country TYPE VARCHAR(50),
    ALTER COLUMN ecommerceaction_type TYPE INTEGER
    USING ecommerceaction_type::INTEGER,
    ALTER COLUMN ecommerceaction_step TYPE INTEGER
    USING ecommerceaction_step::INTEGER,
    ALTER COLUMN time TYPE INTEGER USING time :: integer,
    ALTER COLUMN productprice TYPE NUMERIC(10,2),
    ALTER COLUMN timeonsite TYPE INTEGER USING timeonsite :: integer;

# Check NULL values

    SELECT timeonsite, 
    pageviews,
    date,
    productprice,
    v2productname,
	v2productcategory,
    currencycode,
    transactionrevenue,
    pagetitle,
	pagepathlevel1,
    ecommerceaction_type,
    ecommerceaction_step
    FROM all_sessions
    WHERE pageviews IS NULL
        AND date IS NULL
        AND productprice IS NULL
        AND v2productname IS NULL
        AND v2productcategory IS NULL
        AND currencycode IS NULL
        AND transactionrevenue IS NULL
        AND pagetitle IS NULL
        AND pagepathlevel1 IS NULL
        AND ecommerceaction_type IS NULL
        AND ecommerceaction_step IS NULL

    SELECT revenue,bounces,timeonsite,units_sold
    FROM analytics   
    WHERE revenue IS NULL
    AND bounces IS NULL
    AND timeonsite IS NULL
    AND units_sold IS NULL

    SELECT orderedquantity,
    stocklevel,
    restockingleadtime,
    sentimentscore,
	sentimentmagnitude
    FROM products
    WHERE orderedquantity IS NULL
    AND stocklevel IS NULL
    AND restockingleadtime IS NULL
    AND sentimentscore IS NULL
    AND sentimentmagnitude IS NULL


    SELECT total_ordered,productsku
    FROM sales_by_sku   
    WHERE total_ordered IS NULL
    AND productsku IS NULL

    SELECT restockingleadtime,
        sentimentscore,
        sentimentmagnitude,
        ratio,stocklevel
    FROM sales_report
    WHERE restockingleadtime IS NULL
    AND sentimentscore IS NULL
    AND sentimentmagnitude IS NULL
    AND ratio IS NULL
    AND stocklevel IS NULL


# Fill NULL values
    SELECT revenue,bounces,timeonsite,units_sold,
    CASE
    WHEN revenue IS NULL THEN (SELECT AVG(revenue) FROM analytics)
    ELSE revenue
    END as revenue,
    CASE
    WHEN bounces IS NULL THEN (SELECT AVG(bounces) FROM analytics)
    ELSE bounces
    END as bounces,
    CASE
    WHEN timeonsite IS NULL THEN (SELECT AVG(timeonsite) FROM analytics)
    ELSE timeonsite
    END AS timeonsite,
    CASE
    WHEN units_sold IS NULL THEN (SELECT AVG(units_sold) FROM analytics)
    ELSE units_sold
    END 
    FROM analytics


    SELECT transactionrevenue,
    CASE
    WHEN transactionrevenue IS NULL THEN (SELECT AVG(transactionrevenue) FROM all_sessions)
    ELSE transactionrevenue
    END
    FROM all_sessions


# Update table remove null values

    UPDATE all_sessions
    SET transactionrevenue = 602750000.00000000
    WHERE transactionrevenue IS NULL;

    UPDATE all_sessions
    SET currencycode = 'USD'
    WHERE currencycode IS NULL;

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

