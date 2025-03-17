Documentation:

    This project sources its data from a MySQL database, local json file, and a MongoDB collection. It aggregates the sourced information into a fact table that models the rental business process.

    The source data files are the following:
        1. "sakila" database: stored in the "data" directory; can be recreated using the sql scripts under "data/sakila_set_up" if desired.
        2. "film" collection: stored in MongoDB; included under "data/MongoDB" for your reference.
        3. "customer_data.json": a local file stored in this directory.
    
    All of the code for the project is in the "midterm_project.ipynb" file. The code is organized by cells with section headers where necessary that describe the general intent for that section.

    I used four dimension tables: dim_date, dim_customer, dim_film, and dim_staff. These dimension tables were created by combining/cleaning other dataframes and connecting them to the fact_rentals table through foreign keys. A step-by-step procedure can be found below.
    The date dimension in specific was created using the "dim_date_sql.sql" file. There is a note in the python notebook on when to run this sql script.

    This is my process:
        1. Necessary libraries were imported and MongoDB and MySQL connections were set up.
        2. The "dim_date_sql.sql" file was ran to create the date dimension.
        3. The data from all the source database tables was extracted for later use.
            a. df_payment from the sakila database.
            b. df_inventory from the sakila database.
            c. df_film from MongoDB "film" collection.
            d. df_staff from the sakila database.
            c. df_customer from the local "customer_data.json" file.
        4. These dataframes were cleaned by dropping certain columns and inserting keys where needed.
        5. The dimension tables were loaded into the new database "sakila_dw". The three tables that are loaded are "dim_customers", "dim_film", and "dim_staff".
        6. The rental table was extracted from the sakila database to use as the new fact_rentals table. This table was cleaned and necessary columns were attached by merging it with df_inventory and df_payment. 
        7. The date dimension ("dim_date") key and full_date were extracted in order to modify it and connect payment_date_key, rental_date_key, and return_date_key to the fact_rentals table.
        8. The rest of the dimension tables were connected to fact_rentals by merging them on their respective ids.
        9. The fact_rentals dataframe was cleaned and written back to the database (destination: "sakila_dw").
        
    The two SQL statements I chose to write returns the sum of rental payments for each film and the total number of rentals for each customer 