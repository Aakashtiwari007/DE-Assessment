Staging Table

CREATE TABLE Staging_Customers (
    Customer_Name VARCHAR(255) NOT NULL,
    Customer_Id VARCHAR(18) NOT NULL,
    Open_Date DATE NOT NULL,
    Last_Consulted_Date DATE,
    Vaccination_Id CHAR(5),
    Dr_Name CHAR(255),
    State CHAR(5),
    Country CHAR(5),
    DOB DATE,
    Is_Active CHAR(1),
    PRIMARY KEY (Customer_Id)
);
Country-Specific Tables

CREATE TABLE Table_India (
    Customer_Name VARCHAR(255) NOT NULL,
    Customer_Id VARCHAR(18) NOT NULL,
    Open_Date DATE NOT NULL,
    Last_Consulted_Date DATE,
    Vaccination_Id CHAR(5),
    Dr_Name CHAR(255),
    State CHAR(5),
    Country CHAR(5),
    DOB DATE,
    Is_Active CHAR(1),
    Age INT,
    Days_Since_Last_Consulted INT,
    PRIMARY KEY (Customer_Id)
);

Derived Age and Days Since Last Consulted

INSERT INTO Table_India (Customer_Name, Customer_Id, Open_Date, Last_Consulted_Date, Vaccination_Id, Dr_Name, State, Country, DOB, Is_Active, Age, Days_Since_Last_Consulted)
SELECT 
    Customer_Name,
    Customer_Id,
    Open_Date,
    Last_Consulted_Date,
    Vaccination_Id,
    Dr_Name,
    State,
    Country,
    DOB,
    Is_Active,
    FLOOR(DATEDIFF(CURRENT_DATE, DOB) / 365) AS Age,
    DATEDIFF(CURRENT_DATE, Last_Consulted_Date) AS Days_Since_Last_Consulted
FROM Staging_Customers
WHERE Country = 'IND';

SELECT * FROM Staging_Customers 
WHERE 
    Customer_Name IS NULL OR
    Customer_Id IS NULL OR
    Open_Date IS NULL OR
    (Last_Consulted_Date IS NOT NULL AND Last_Consulted_Date < Open_Date) OR
    (DATEDIFF(CURRENT_DATE, DOB) < 0);
