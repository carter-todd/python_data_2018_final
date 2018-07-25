This is a final project for the summer 2018 session of the Code Louisville Python for Data course.  This project examines the prices of avocados in Louisville and how they compare to different regions.

Questions that will be answered:
    -What is the average price of an avocado in Louisville?  
    -How does this price compare with the most expensive and least expensive regions in the United States?

I will use a database downloaded from Kaggle.com that contains avocado prices for different regions during the years 2015, 2016, 2017 through the first half of 2018.

How I answered:
    -I executed python code and SQL lite queries in a jupyter notebook using the database I downloaded from Kaggle.com. 
    -I executed SQL queries to determine the average prices, minimum prices and maximum prices of avocados in Louisville as well the most expensive and least expensive regions in the United States.
    -I used matplotlib bar plots to illustrate my findings.

Dependencies:
    -Python 3
    -Jupyter notebook
    -matplotlib
    -SQL lite

Included in this download:
    -This readme.txt file that contains instructions for executing this code in a jupyter notebook.
    -avocado.db file
    -avocado.csv file
    -a jupyter notebook with all the executed code and my comments.

Let's Begin!
######### It will work better by copying and pasting from the clean draft!#############

Cell by cell instructions:
    1.  Open a new jupyter notebook and enter the initial imports:
        import pandas as pd
        import numpy as np 
        import sqlite3 
    
    2.  Connect to the database:
        conn = sqlite3.connect('avocado.db')

    3.  Create cursor:
        cur = conn.cursor()
        avo = cur.execute("""
           select * from avocado;"""
           ).fetchall()

    4.  Create first dataframe:
        df = pd.DataFrame(data=avo)

    5.  Preview the data:
        df.head()

    6.  The column names are missing.  To see the names of columns:
        colnames = cur.description
        for row in colnames:
            print (row[0])

    7.  Now we will add the column names to our table:
        Frame=pd.DataFrame(avo, columns = ["field1", "Date", "AveragePrice", "TotalPrice","4046","4225","4770","TotalBags","SmallBags",
                                  "LargeBags","XLargeBags","type","year","region"])

    8.  Column names are now added:
        Frame.head()

    9.  How does Louisville's average price for an avocado compare with the region with the highest average price for an avocado?.  First, we will establish a new connection:
        conn2 = sqlite3.connect('avocado.db')

    10.  Execute a SQL query that finds the information we need to answer this question:
        cur2 = conn2.cursor()
        avo2 = cur2.execute("""
        Select * from 
        (
        SELECT 
        REGION, 
        AVG(AVERAGEPRICE) AS AVG_REGION_PRICE,
        MIN(AVERAGEPRICE) AS MIN_REGION_PRICE,
        MAX(AVERAGEPRICE) AS MAX_REGION_PRICE
        FROM AVOCADO
        GROUP BY REGION
        ORDER BY AVG(AVERAGEPRICE) DESC
        LIMIT 1
        )

        UNION

        SELECT 
        REGION, 
        AVG(AVERAGEPRICE) AS AVG_REGION_PRICE,
        MIN(AVERAGEPRICE) AS MIN_REGION_PRICE,
        MAX(AVERAGEPRICE) AS MAX_REGION_PRICE
        FROM AVOCADO
        WHERE REGION = 'Louisville'
        GROUP BY REGION
        ORDER BY AVG(AVERAGEPRICE) DESC
        ;"""
        ).fetchall()

    11.  Make this query into a dataframe in order to illustrate it:
        df2 = pd.DataFrame(data=avo2)

    12.  View the dataframe:
        df2.head()

    13.  No column names like before.  Let's add them:
        colnames2 = cur2.description
        for row in colnames2:
            print (row[0])

    14.  Then:
        Frame2=pd.DataFrame(avo2, columns = ["REGION", "AVG_REGION_PRICE", "MIN_REGION_PRICE","MAX_REGION_PRICE"])

    15.  Column names are now added:
            Frame2.head()

    16.  Louisville's average price is $.60 lower than HartfordSpringfield, which had the highest average price per avocado. Louisville's maximum price and minimum price was lower as well.Now we will illustrate this data using matplotlib:
        import matplotlib.pyplot as plt

    17.  Illustrate using a bar plot:
        ax = Frame2[['AVG_REGION_PRICE','MIN_REGION_PRICE','MAX_REGION_PRICE']].plot(kind='bar', title ="Louisville_Hartford_Comparison", figsize=(15, 10), legend=True, fontsize=12)
        ax.set_xlabel("Region", fontsize=12)
        ax.set_ylabel("Price in dollars", fontsize=12)
        plt.show()

    18.  How does the average price of an avocado in Louisville compare region with the lowest average price?  Enter:
        conn3 = sqlite3.connect('avocado.db')   

    19.  Execute this SQL query to answer this question:
        cur3 = conn3.cursor()
        avo3 = cur3.execute("""
        Select * from 
        (
        SELECT 
        REGION, 
        AVG(AVERAGEPRICE) AS AVG_REGION_PRICE,
        MIN(AVERAGEPRICE) AS MIN_REGION_PRICE,
        MAX(AVERAGEPRICE) AS MAX_REGION_PRICE
        FROM AVOCADO
        GROUP BY REGION
        ORDER BY AVG(AVERAGEPRICE)
        LIMIT 1
        )

        UNION

        SELECT 
        REGION, 
        AVG(AVERAGEPRICE) AS AVG_REGION_PRICE,
        MIN(AVERAGEPRICE) AS MIN_REGION_PRICE,
        MAX(AVERAGEPRICE) AS MAX_REGION_PRICE
        FROM AVOCADO
        WHERE REGION = 'Louisville'
        GROUP BY REGION
        ORDER BY AVG(AVERAGEPRICE) DESC
        ;"""
        ).fetchall()

    20.  Create dataframe in order to illustrate this data:
        df3 = pd.DataFrame(data=avo3)

    21.  View data:
        df3.head()

    22.  Once again we'll need to add the column names:
        colnames3 = cur3.description
        for row in colnames3:
            print (row[0])

    23.  Add the column names:
        Frame3=pd.DataFrame(avo3, columns = ["REGION", "AVG_REGION_PRICE", "MIN_REGION_PRICE","MAX_REGION_PRICE"])

    24.  Column names are now added:
            Frame3.head()

    25. #Louisville's average price is $.24 more expensive than Houston, which has the cheapest avocados on average.  Louisville' minimum price is slightly higher while it's maximum price is noticeably higher.

        Use another bar plot to illustrate this data:
            ax = Frame3[['AVG_REGION_PRICE','MIN_REGION_PRICE','MAX_REGION_PRICE']].plot(kind='bar', title ="Louisville_Houston_Comparison", figsize=(15, 10), legend=True, fontsize=12)
            ax.set_xlabel("Region", fontsize=12)
            ax.set_ylabel("Price in dollars", fontsize=12)
            plt.show()


