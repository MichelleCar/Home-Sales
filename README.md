# The Big Data Revolution
## Case Study: Home Sales Analysis

Nearly everything we do today leaves an indelible digital trail: Where we live. What we search. What we read. Where we go. What we buy. What we say. All of this data is being recorded and stored.  The data revolution is unlike anything humanity has ever experienced. Now, every two days, the human race is generating as much data as was generated from the dawn of humanity through the year 2003. Today, individuals in the world’s major cities are exposed to as much information in a single day as their 15th-century ancestors were exposed to in a lifetime. With phones and devices serving as the “on-ramp” for billions of people to access the information network, technology has fundamentally changed the way people interact with everything. Our phones, computers, homes, cars, appliances and a multitude of objects are generating data that can be extracted and used on a mass scale, enabling us to collect, store and process vast quantities of data and then respond in real time.

![22-dns-10](https://user-images.githubusercontent.com/115101031/229378348-830df34d-0e1c-41b4-a4ed-7c78bcc18697.jpg)

Big data refers to data that is so large, fast or complex that it’s difficult or impossible to process using traditional methods. Characteristics of big data include:
* High Volume: Organizations collect data from a variety of sources, including transactions, smart (IoT) devices, industrial equipment, videos, images, audio, social media and more. In the past, storing all that data would have been too costly – but cheaper storage using data lakes, Hadoop and the cloud have eased the burden.
* High Velocity: With the growth in the Internet of Things, data streams into businesses at an unprecedented speed and must be handled in a timely manner. RFID tags, sensors and smart meters are driving the need to deal with these torrents of data in near-real time.
* High Variety: Data comes in all types of formats – from structured, numeric data in traditional databases to unstructured text documents, emails, videos, audios, stock ticker data and financial transactions.
* Veracity: When talking about big data that comes from a variety of sources, it’s important to understand the chain of custody, metadata and the context when the data was collected to be able to glean accurate insights. The higher the veracity of the data equates to the data’s importance to analyze and contribute to meaningful results for an organization.

## Approaches to Big Data

With big data analytics, you can make better and faster decisions using a range of different tools, such as Apache Spark.  Apache Spark is a data processing framework that can quickly perform processing tasks on very large data sets.  Apache Spark has become one of the key big data distributed processing frameworks in the world. Spark can be deployed in a variety of ways, including supporting SQL.  Spark SQL is focused on the processing of structured data, using a dataframe approach borrowed from Python (in Pandas) and an interface for querying data.

The spark.sql is a module in Spark that is used to perform SQL-like operations on the data stored in memory. 

<img width="1162" alt="Screenshot 2023-04-02 at 5 28 11 PM" src="https://user-images.githubusercontent.com/115101031/229379891-8578dfba-3b1a-4e88-ac5f-61706fe50b6f.png">

Spark SQL will make it possible for developers to:
* Import relational data from parquet files and hive tables
* Execute SQL queries across the imported data
* Create hive tables or parquet files 

In addition, Spark SQL comes with columnar storage, cost-based optimizer, and code generation, all of which help to speed up query execution. At the same time, it is scalable to thousands of nodes and searches that take many hours by using the Spark engine, which offers complete fault tolerance during the middle of a query and eliminates the need to worry about using a different engine for historical data.

<img width="608" alt="Screenshot 2023-04-02 at 5 48 45 PM" src="https://user-images.githubusercontent.com/115101031/229380792-aaaad0d2-61f3-4055-8dcc-3aabb815846a.png">


## Case Study

### Methodology
Using my knowledge of SparkSQL, I determined key metrics about home sales data. I used Spark to create temporary views, partition the data, cache and uncache a temporary table, and verify that the table had been uncached.

After importing the necessary PySpark SQL functions:
* I read the home_sales_revised.csv data in the starter code into a Spark DataFrame.
* Created a temporary table called home_sales.
* Answered the following questions using SparkSQL:
    * What is the average price for a four-bedroom house sold for each year? Round off your answer to two decimal places.
                  spark.sql("""
                    SELECT 
                      date_built,
                      ROUND(AVG(price), 2) as avg_price
                    FROM home_sales
                  WHERE bedrooms = 4
                  GROUP BY date_built
                  ORDER BY 1 DESC
                  """).show()
    
    * What is the average price of a home for each year it was built that has three bedrooms and three bathrooms? Round off your answer to two decimal places.
                  spark.sql("""
                    SELECT 
                        date_built,
                        ROUND(AVG(price), 2) as avg_price
                    FROM home_sales
                    WHERE bedrooms = 3 and bathrooms = 3
                    GROUP BY date_built
                    ORDER BY 1 DESC
                    """).show()

    * What is the average price of a home for each year that has three bedrooms, three bathrooms, two floors, and is greater than or equal to 2,000 square feet? Round off your answer to two decimal places.
                    spark.sql("""
                      SELECT 
                          date_built,
                          ROUND(AVG(price), 2) as avg_price
                      FROM home_sales
                      WHERE bedrooms = 3 and bathrooms = 3 and floors =2 and sqft_living >= 2000
                      GROUP BY date_built
                      ORDER BY 1 DESC
                      """).show()
    * What is the "view" rating for homes costing more than or equal to $350,000? Determine the run time for this query, and round off your answer to two decimal places.
                      spark.sql("""
                        SELECT 
                            view,
                            ROUND(AVG(price), 2) as avg_price
                        FROM home_sales
                        WHERE price >= 350000
                        GROUP BY view
                        ORDER BY 1 DESC
                        """).show()


* I then cached the temporary table home_sales and checked if your temporary table is cached.
* Using the cached data, I ran the query that filters out the view ratings with an average price of greater than or equal to $350,000. I determined the runtime to compare it to uncached runtime.
* I partitioned by the "date_built" field on the formatted parquet home sales data and created a temporary table for the parquet data.
* I ran the query that filters out the view ratings with an average price of greater than or equal to $350,000, and determined the runtime to compare it to uncached runtime.
* Finally, I uncached the home_sales temporary table, and verified that the home_sales temporary table is uncached using PySpark.

### Results
Before caching my table data, the runtime to generate the "view" rating for the average price of a home, rounded to two decimal places, where the homes are greater than or equal to $350,000 was **0.2004401683807373 seconds**.  After caching the table, it was **0.11345887184143066 seconds**, showing a significant improvement in processing time. After partitioned by the "date_built" field on the formatted parquet home sales data and created a temporary table for the parquet data, and rerunning the query that filters out the view ratings with an average price of greater than or equal to $350,000, the runtime was **0.29360389709472656 seconds**.


