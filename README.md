# MY-SQL-PROJECT

/* The following lines of code are been used to analyze the Car_dataset, housed under 'sqlprotfolio' schema*/

/* Creating a database called 'sqlportfolio' and making it available for use*/
CREATE SCHEMA sqlportfolio;
USE sqlportfolio;

/* Reading the first 6 rows of the 'Car_Data' dataset*/

SELECT 
    *
FROM
    sqlportfolio.car_data
LIMIT 6;

/*Counting the number of Rows in the Dataset*/
SELECT 
    COUNT(*)
FROM
    sqlportfolio.car_data;

/*Counting the number of Used and New Cars in the Dataset*/
SELECT 
    Car_Condition, COUNT(Car_Condition)
FROM
    sqlportfolio.car_data
GROUP BY Car_Condition;

/* Searching for the Car Brand with the Highest Quantity in Stock according to its condition while excluding null values*/
SELECT Brand, COUNT(Brand) as Quantity_In_Stock, Car_Condition, RANK() OVER(ORDER BY COUNT(Brand) DESC) as Rank_Order
FROM sqlportfolio.car_data
WHERE Brand IS NOT NULL && Car_Condition IS NOT NULL
GROUP BY Brand, Car_Condition
LIMIT 10;


/* Searching for the Car Brand with the Least Quantity in Stock based on itz condition While excluding Null Values*/
SELECT Brand, COUNT(Brand) as Quantity, Car_Condition, RANK() OVER(ORDER BY COUNT(Brand) ASC) as Rank_Order
FROM sqlportfolio.car_data
WHERE Brand IS NOT NULL && Car_Condition IS NOT NULL
GROUP BY Brand, Car_Condition
LIMIT 10;

/* Let's look out for the Car Brand with the Highest Quantity in Stock based on color*/
SELECT 
    Brand, Color, COUNT(Color) AS Quantity
FROM
    sqlportfolio.car_data
WHERE
    Brand IS NOT NULL && Color IS NOT NULL
GROUP BY Brand , Color
ORDER BY Brand ASC , Color ASC , Quantity DESC;

/* The next line of code checks for the Top 10 most expensive Car Brands and Models in Stock based on their Condition*/
SELECT Brand, Model, Car_Condition,  max(Price) as Price,
RANK() OVER (ORDER BY max(Price) DESC) AS Price_rank
FROM sqlportfolio.car_data
GROUP BY Brand, Model, Car_Condition
LIMIT 10;

/* The next line of code checks for the 10 least expensive Car Brands and Models in Stock*/
SELECT Brand, Car_Condition, Model, min(Price) as Price,
RANK() OVER (ORDER BY min(Price) ASC) AS salary_rank
FROM sqlportfolio.car_data
GROUP BY Brand, Model, Car_Condition
LIMIT 10;

/* Checking for the percentage of New and Used Cars in the Total Dataset*/
SELECT 
    (COUNT(CASE
        WHEN Car_Condition = 'New' THEN 1
    END) / COUNT(*)) * 100 AS Percentage_of_New_Cars,
    (COUNT(CASE
        WHEN Car_Condition = 'Used' THEN 1
    END) / COUNT(*)) * 100 AS Percentage_of_Used_Cars
FROM
    sqlportfolio.car_data;

/* Percentage of All Car brands in the Dataset*/
SELECT 
    Brand,
    COUNT(Brand) AS Quantity,
    (COUNT(Brand) / (SELECT 
            COUNT(*)
        FROM
            sqlportfolio.car_data)) * 100 AS Percentage,
    (SELECT 
            COUNT(*)
        FROM
            sqlportfolio.car_data) AS Total_cars
FROM
    sqlportfolio.car_data
GROUP BY Brand
ORDER BY Brand;


/* The following Lines of Code Calculates the Average Mileage of Used and New Cars and their average Prices, based on the Car condition*/
SELECT 
    Brand, Car_Condition, AVG(Mileage), AVG(Price)
FROM
    sqlportfolio.car_data
GROUP BY Brand , Car_Condition
ORDER BY Brand ASC , Car_Condition ASC , AVG(Mileage) , AVG(Price);

/* The following lines of code looks for the most common Manufacture Year of Cars in the Dataset*/
SELECT Brand, Model, Year as Manufacture_Year, COUNT(Year) as Number_In_Stock, Car_Condition
FROM sqlportfolio.car_data
GROUP BY Brand, Year, Car_Condition, Model
ORDER BY Number_In_Stock DESC;
