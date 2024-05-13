-- This CTE calculates the total payment amount for each customer and their location details.
WITH average_amount_cte (customer_id, first_name, last_name, country, city) AS (
    SELECT CU.customer_id, CU.first_name, CU.last_name, CI.city, CO.country, SUM(P.amount) AS total_payment_amount
    FROM customer CU
    INNER JOIN payment P ON CU.customer_id = P.customer_id
    INNER JOIN address AD ON CU.address_id = AD.address_id 
    INNER JOIN city CI ON AD.city_id = CI.city_id
    INNER JOIN country CO ON CI.country_id = CO.country_id
    -- Subquery to filter cities based on the top 10 countries with the most customers
    WHERE CI.city IN (
        SELECT CI.city
        FROM customer CU
        INNER JOIN address AD ON AD.address_id = CU.address_id 
        INNER JOIN city CI ON AD.city_id = CI.city_id
        INNER JOIN country CO ON CO.country_id = CI.country_id
        -- Subquery to filter countries based on the top 10 countries with the most customers
        WHERE CO.country IN (
            SELECT CO.country
            FROM customer CU
            INNER JOIN address AD ON AD.address_id = CU.address_id 
            INNER JOIN city CI ON AD.city_id = CI.city_id
            INNER JOIN country CO ON CO.country_id = CI.country_id
            GROUP BY country
            ORDER BY COUNT(customer_id) DESC
            LIMIT 10
        )
        GROUP BY CI.city
        ORDER BY COUNT(customer_id) DESC
        LIMIT 10
    )
    GROUP BY CO.country, CI.city
    ORDER BY COUNT(customer_id) DESC
    LIMIT 10
)
-- This query calculates the average total payment amount for the top 5 customers.
SELECT AVG(total_payment_amount) 
FROM average_amount_cte;
