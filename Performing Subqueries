-- This query retrieves the count of top 5 customers identified in step 1 within each country.
SELECT 
    CO.country, 
    COUNT(DISTINCT CU.customer_id) AS all_customer_count, -- Count of all customers within each country
    COUNT(DISTINCT top_5_customers.customer_id) AS top_customer_count -- Count of top 5 customers within each country
FROM 
    customer CU
INNER JOIN 
    address AD ON CU.address_id = AD.address_id
INNER JOIN 
    city CI ON AD.city_id = CI.city_id
INNER JOIN 
    country CO ON CI.country_id = CO.country_id 
LEFT JOIN
    (
    -- Subquery to calculate the top 5 customers and their details
    SELECT 
        CU.customer_id, 
        CU.first_name, 
        CU.last_name, 
        CI.city,
        CO.country, 
        SUM(P.amount) AS total_payment_amount
    FROM 
        customer CU
    INNER JOIN 
        payment P ON CU.customer_id = P.customer_id 
    INNER JOIN 
        address AD ON CU.address_id = AD.address_id 
    INNER JOIN 
        city CI ON AD.city_id = CI.city_id
    INNER JOIN 
        country CO ON CI.country_id = CO.country_id 
    WHERE 
        CI.city IN (
            -- Subquery to filter cities based on the top 10 countries with the most customers
            SELECT 
                CI.city
            FROM 
                customer CU
            INNER JOIN 
                address AD ON AD.address_id = CU.address_id 
            INNER JOIN 
                city CI ON AD.city_id = CI.city_id
            INNER JOIN 
                country CO ON CO.country_id = CI.country_id
            -- Subquery to filter countries based on the top 10 countries with the most customers
            WHERE 
                CO.country IN (
                    SELECT 
                        CO.country
                    FROM 
                        customer CU
                    INNER JOIN 
                        address AD ON AD.address_id = CU.address_id 
                    INNER JOIN 
                        city CI ON AD.city_id = CI.city_id
                    INNER JOIN 
                        country CO ON CO.country_id = CI.country_id
                    GROUP BY 
                        country
                    ORDER BY 
                        COUNT(customer_id) DESC
                    LIMIT 10
                )
            GROUP BY 
                CI.city
            ORDER BY 
                COUNT(customer_id) DESC
            LIMIT 10
        )
    GROUP BY 
        CO.country, CI.city
    ORDER BY 
        COUNT(customer_id) DESC
    LIMIT 10
    ) AS top_5_customers
ON 
    CU.customer_id = top_5_customers.customer_id 
GROUP BY 
    CO.country
ORDER BY 
    all_customer_count DESC
LIMIT 10;
