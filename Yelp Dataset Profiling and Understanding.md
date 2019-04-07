### Part 1: Yelp Dataset Profiling and Understanding ###

**1. Profile the data by finding the total number of records for each of the tables below:**
```
i. Attribute table =  10000
ii. Business table = 10000
iii. Category table = 10000
iv. Checkin table = 10000
v. elite_years table = 10000
vi. friend table = 10000
vii. hours table = 10000
viii. photo table = 10000
ix. review table = 10000
x. tip table = 10000
xi. user table = 10000`
```

**2. Find the total distinct records by either the foreign key or primary key for each table. If two foreign keys are listed in the table, please specify which foreign key.**
```
**i. Business**
- With DISTINCT on id (PK)= 10000
ii. Hours
- With DISTINCT on business_id (FK)= 1562 
iii. Category
- With DISTINCT on business_id (FK)= 2643 
iv. Attribute
- With DISTINCT on business_id (FK)= 1115 
v. Review
- With DISTINCT on id (PK)= 10000
- With DISTINCT on business_id(FK)= 8090 
- With DISTINCT on User_id(FK)= 9581  
vi. Checkin
- With DISTINCT on business_id(FK)= 493 
vii. Photo
- With DISTINCT on id (PK) = 10000
- With DISTINCT on business_id(FK)= 6493 
viii. Tip 
- With DISTINCT on business_id(FK)= 3979 
- With DISTINCT on User_id(FK)= 537 
ix. User 
- With DISTINCT on id (PK)= 10000
x. Friend
- With DISTINCT on User_id(FK)= 11
xi. Elite_years
- With DISTINCT on User_id(FK)= 2780 

*Note: Primary Keys are denoted in the ER-Diagram with a yellow key icon.*
```
**3. Are there any columns with null values in the Users table? Indicate "yes," or "no."**
```
	SELECT COUNT(*)
	FROM user
	WHERE id IS NULL OR 
		  name IS NULL OR 
		  review_count IS NULL OR 
		  yelping_since IS NULL OR
		  useful IS NULL OR 
		  funny IS NULL OR 
		  cool IS NULL OR 
		  fans IS NULL OR 
		  average_stars IS NULL OR 
		  compliment_hot IS NULL OR 
		  compliment_more IS NULL OR 
		  compliment_profile IS NULL OR 
		  compliment_cute IS NULL OR 
		  compliment_list IS NULL OR 
		  compliment_note IS NULL OR 
		  compliment_plain IS NULL OR 
		  compliment_cool IS NULL OR 
		  compliment_funny IS NULL OR 
		  compliment_writer IS NULL OR 
		  compliment_photos IS NULL 
```
Answer: No  

**4. For each table and column listed below, display the smallest (minimum), largest (maximum), and average (mean) value for the following fields:**
```
	i. Table: Review, Column: Stars
		min: 1		max: 5		avg: 3.7082
		
	ii. Table: Business, Column: Stars
		min: 1		max: 5		avg: 3.6549 
		
	iii. Table: Tip, Column: Like
		min:0		max: 2		avg: 0.0144
		
	iv. Table: Checkin, Column: Count
		min: 1		max: 53		avg: 1.9414 
		
	v. Table: User, Column: Review_count
		min: 0		max: 2000		avg: 24.2995 
```

**5. List the cities with the most reviews in descending order:**
SQL code used to arrive at answer: 
Adding up the total review_count for each City.
```	
SELECT SUM(REVIEW_COUNT) AS SUMA_RECUENTO, CITY
	FROM BUSINESS
	GROUP BY CITY
	ORDER BY SUMA_RECUENTO DESC
```
***Copy and Paste the Result Below:***
		 
  SUMA_RECUENTO | city         
--|--
       82854 | Las Vegas    
       34503 | Phoenix   
       24113 | Toronto         
       20614 | Scottsdale      
     12523 | Charlotte       
        10871 | Henderson       
         10504 | Tempe           
         9798 | Pittsburgh      
         9448 | Montréal        
         8112 | Chandler        
         6875 | Mesa            
          6380 | Gilbert         
          5593 | Cleveland       
          5265 | Madison         
          4406 | Glendale       
          3814 | Mississauga     
          2792 | Edinburgh       
          2624 | Peoria          
          2438 | North Las Vegas 
          2352 | Markham         
         2029 | Champaign       
          1849 | Stuttgart       
            1520 | Surprise        
         1465 | Lakewood        
          1155 | Goodyear  
	
**6. Find the distribution of star ratings to the business in the following cities:**
*i. Avon*

```	
SELECT stars, sum(review_count) AS Total_stars
FROM business
GROUP BY stars, city
HAVING city ='Avon'
```
***Copy and Paste the Resulting Table Below (2 columns – star rating and count):***

| stars | Total_stars |
--|--
|   1.5 |                10 |
|   2.5 |                 6 |
|   3.5 |                88 |
|   4.0 |                21 |
|   4.5 |                31 |
|   5.0 |                 3 |


*ii. Beachwood*

```
	SELECT stars, sum(review_count) AS Total_stars
	FROM business
	GROUP BY stars, city
	HAVING city ='Beachwood'
```

***Copy and Paste the Resulting Table Below (2 columns – star rating and count):***


| stars | Total_Stars |
--|--
|   2.0 |                 8 |
|   2.5 |                 3 |
|   3.0 |                11 |
|   3.5 |                 6 |
|   4.0 |                69 |
|   4.5 |                17 |
|   5.0 |                23 |


**7. Find the top 3 users based on their total number of reviews:**

``` 
SELECT NAME, REVIEW_COUNT
FROM USER
ORDER BY REVIEW_COUNT DESC
LIMIT 3
```

	
name   | review_count |
--|--
| Gerald |         2000 |
| Sara   |         1629 |
| Yuri   |         1339 |

**8. Does posing more reviews correlate with more fans?**

*Please explain your findings and interpretation of the results:*

- As table below illustrates, there is no strong correlation between posing more reviews with having more fans.
- Gerarld is who has more reviews but not who has more fans in comparison with Harald who has more fans than Gerarld and less reviews. 
- Sorting the users in descending order based on their total number of reviews does not sort the fans in the same order.
```
SELECT NAME, REVIEW_COUNT, FANS
FROM USER
WHERE FANS > (SELECT AVG(FANS) FROM USER)
ORDER BY REVIEW_COUNT DESC
```
name | review_count | fans
:--:|:--:|:--:
 Gerald    |2000 |  253 
| Sara      |         1629 |   50 |
| Yuri      |         1339 |   76 |
| .Hon      |         1246 |  101 |
| William   |         1215 |  126 |
| Harald    |         1153 |  311 |
| eric      |         1116 |   16 |
| Roanna    |         1039 |  104 |
| Mimi      |          968 |  497 |
| Christine |          930 |  173 |
| Ed        |          904 |   38 |
| Nicole    |          864 |   43 |
| Fran      |          862 |  124 |
| Mark      |          861 |  115 |
| Christina |          842 |   85 |
| Dominic   |          836 |   37 |
| Lissa     |          834 |  120 |
| Lisa      |          813 |  159 |
| Alison    |          775 |   61 |
| Sui       |          754 |   78 |
| Tim       |          702 |   35 |
| L         |          696 |   10 |
| Angela    |          694 |  101 |
| Crissy    |          676 |   25 |
| Lyn       |          675 |   45 |  
*(Output limit exceeded, 25 of 1209 total rows shown)*

**9. Are there more reviews with the word "love" or with the word "hate" in them?**

**Answer**: As you can see below, there is more reviews with the word "Love" than with the word "Hate"

```
SELECT COUNT(TEXT)
FROM REVIEW
WHERE TEXT LIKE '%LOVE%' 
```
COUNT(TEXT) |
-|
 1780|


	SELECT COUNT(TEXT)
	FROM REVIEW
	WHERE TEXT LIKE '%HATE%'

COUNT(TEXT) |
-|
|         232 |


**10. Find the top 10 users with the most fans:**

	SELECT ID, FANS, NAME
	FROM USER
	ORDER BY FANS DESC
	LIMIT 10
	

id                     | fans | name      |
--|--|--|
| -9I98YbNQnLdAmcYfb324Q |  503 | Amy       |
| -8EnCioUmDygAbsYZmTeRQ |  497 | Mimi      |
| --2vR0DIsmQ6WfcSzKWigw |  311 | Harald    |
| -G7Zkl1wIWBBmD0KRy_sCw |  253 | Gerald    |
| -0IiMAZI2SsQ7VmyzJjokQ |  173 | Christine |
| -g3XIcCb2b-BD0QBCcq2Sw |  159 | Lisa      |
| -9bbDysuiWeo2VShFJJtcw |  133 | Cat       |
| -FZBTkAZEXoP7CYvRV2ZwQ |  126 | William   |
| -9da1xk7zgnnfO1uTVYGkA |  124 | Fran      |
| -lh59ko3dxChBSZ9U7LfUw |  120 | Lissa     |


**11. Is there a strong relationship (or correlation) between having a high number of fans and being listed as "useful" or "funny?" 
Out of the top 10 users with the highest number of fans, what percent are also listed as “useful” or “funny”?**

*Key:
0% - 25% - Low relationship
26% - 75% - Medium relationship
76% - 100% - Strong relationship*
	
	SELECT NAME,FANS, USEFUL, FUNNY
	FROM USER
	ORDER BY FANS DESC
	LIMIT 10
 name      | fans | useful |  funny 
:--:|:--:|:--:|:--:
| Amy       |  503 |   3226 |   2554 |
| Mimi      |  497 |    257 |    138 |
| Harald    |  311 | 122921 | 122419 |
| Gerald    |  253 |  17524 |   2324 |
| Christine |  173 |   4834 |   6646 |
| Lisa      |  159 |     48 |     13 |
| Cat       |  133 |   1062 |    672 |
| William   |  126 |   9363 |   9361 |
| Fran      |  124 |   9851 |   7606 |
| Lissa     |  120 |    455 |    150 |

There is no correlation having more fans and listed as 'useful' or 'funny'. 
As you can see above, Amy is who has more fans but on the contrary has less comments useful than Gerald 
who has the middle fans.


### Part 2: Inferences and Analysis ###

**1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating. 
Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. Include your code.**

*- Selecting a random city as 'Toronto' and random Category as 'Food'*
	

**i. Do the two groups you chose to analyze have a different distribution of hours?**
   
The group of 2-3 stars seems to belong to the category 'Food' who starts on the Mornings and the group of 4-5 stars seems to belong to the category 'Food' who starts on Afternoon and Night.
Also we can see diferences between the group of 2-3 starts on business works for 14 hours and group of 4-5 starts on business works between 6 - 10 hours.

**ii. Do the two groups you chose to analyze have a different number of reviews?**
    
Yes, for the group of 2-3 stars has less reviews (10) than the other group 4-5 stars that the business 'Food' has 15 and 26 reviews
         
**iii. Are you able to infer anything from the location data provided between these two groups? Explain.**

For all groups there are different locations

    SELECT name, 
		 CASE
			WHEN hours LIKE "%|8%" THEN 'Morning'
			WHEN hours LIKE "%|15%" THEN 'Afternoon'
			WHEN hours LIKE "%|18%" THEN 'Night'
			WHEN hours LIKE "%|16%" THEN 'Afternoon'
		    WHEN hours LIKE "%|11%" THEN 'Morning Late'
			END hourcategory,
			   
		 CASE
			 WHEN hours LIKE "%|8:00-22:00%" THEN '14 HOURS'
			 WHEN hours LIKE "%|16:00-2:00%" THEN '10 HOURS'
			 WHEN hours LIKE "%|15:00-21:00%" THEN '6 HOURS'
			 WHEN hours LIKE "%|11:00-21:00%" THEN '10 HOURS'
			 WHEN hours LIKE "%|18:00-2:00%" THEN '8 HOURS'
			 END Total_Hours,
				  
		 CASE
			 WHEN Business.stars BETWEEN 2 AND 3.5 THEN '2-3 stars'
			 WHEN Business.stars BETWEEN 4 AND 5 THEN '4-5 stars'
			 END star_rating,
				  
		postal_code,
		hours,
		review_count
		
		FROM business INNER JOIN hours 
		ON Business.id = Hours.business_id
		INNER JOIN category 
		ON Category.business_id = Business.id
		WHERE (Business.city = 'Toronto'AND
		Category.category = 'Food') AND
		(Business.stars BETWEEN 2 AND 3 OR
		Business.stars BETWEEN 4 AND 5)
		ORDER BY star_rating,hourcategory
		
 name         | hourcategory | Total_Hours | star_rating | postal_code | hours                 | review_count 
--|--|--|--|--|--|--
| Loblaws      | Morning      | 14 HOURS    | 2-3 stars   | M6R 1X3     | Monday|8:00-22:00     |           10 |
| Loblaws      | Morning      | 14 HOURS    | 2-3 stars   | M6R 1X3     | Tuesday|8:00-22:00    |           10 |
| Loblaws      | Morning      | 14 HOURS    | 2-3 stars   | M6R 1X3     | Friday|8:00-22:00     |           10 |
| Loblaws      | Morning      | 14 HOURS    | 2-3 stars   | M6R 1X3     | Wednesday|8:00-22:00  |           10 |
| Loblaws      | Morning      | 14 HOURS    | 2-3 stars   | M6R 1X3     | Thursday|8:00-22:00   |           10 |
| Loblaws      | Morning      | 14 HOURS    | 2-3 stars   | M6R 1X3     | Sunday|8:00-22:00     |           10 |
| Loblaws      | Morning      | 14 HOURS    | 2-3 stars   | M6R 1X3     | Saturday|8:00-22:00   |           10 |
| Cabin Fever  | Afternoon    | 10 HOURS    | 4-5 stars   | M6P 1A6     | Monday|16:00-2:00     |           26 |
| Cabin Fever  | Afternoon    | 10 HOURS    | 4-5 stars   | M6P 1A6     | Sunday|16:00-2:00     |           26 |
| Cabin Fever  | Afternoon    | 10 HOURS    | 4-5 stars   | M6P 1A6     | Saturday|16:00-2:00   |           26 |
| Halo Brewery | Afternoon    | 6 HOURS     | 4-5 stars   | M6H 1V5     | Tuesday|15:00-21:00   |           15 |
| Halo Brewery | Afternoon    | 6 HOURS     | 4-5 stars   | M6H 1V5     | Friday|15:00-21:00    |           15 |
| Halo Brewery | Afternoon    | 6 HOURS     | 4-5 stars   | M6H 1V5     | Wednesday|15:00-21:00 |           15 |
| Halo Brewery | Afternoon    | 6 HOURS     | 4-5 stars   | M6H 1V5     | Thursday|15:00-21:00  |           15 |
| Halo Brewery | Morning Late | 10 HOURS    | 4-5 stars   | M6H 1V5     | Sunday|11:00-21:00    |           15 |
| Halo Brewery | Morning Late | 10 HOURS    | 4-5 stars   | M6H 1V5     | Saturday|11:00-21:00  |           15 |
| Cabin Fever  | Night        | 8 HOURS     | 4-5 stars   | M6P 1A6     | Tuesday|18:00-2:00    |           26 |
| Cabin Fever  | Night        | 8 HOURS     | 4-5 stars   | M6P 1A6     | Friday|18:00-2:00     |           26 |
| Cabin Fever  | Night        | 8 HOURS     | 4-5 stars   | M6P 1A6     | Wednesday|18:00-2:00  |           26 |
| Cabin Fever  | Night        | 8 HOURS     | 4-5 stars   | M6P 1A6     | Thursday|18:00-2:00   |           26 |

**2. Group business based on the ones that are open and the ones that are closed. 
What differences can you find between the ones that are still open and the ones that are closed? 
List at least two differences and the SQL code you used to arrive at your answer.**
		
*i. Difference 1:*
The business with more reviews are open.   
         
*ii. Difference 2:*
There is no relationship between how many stars has received a business and whether it tend to close or not  
    
    SELECT SUM(REVIEW_COUNT), IS_OPEN
    FROM BUSINESS
    GROUP BY IS_OPEN


 SUM(REVIEW_COUNT) | is_open |
--|--
|             35261 |       0 |
|            269300 |       1 |
```
SELECT stars, count(is_open)AS BUSINESS_CLOSED
FROM business
WHERE is_open = 0 
GROUP BY stars
```

 stars | BUSINESS_CLOSED |
--|--
|   1.0 |              14 |
|   1.5 |              24 |
|   2.0 |              94 |
|   2.5 |             168 |
|   3.0 |             272 |
|   3.5 |             295 |
|   4.0 |             326 |
|   4.5 |             189 |
|   5.0 |             138 |


**3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis.**

*i. Indicate the type of analysis you chose to do:*
  
  How many attributes has to have a business for good rating from users    
         
*ii. Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data:*
    
To understand which kind of users tend to go to one business or other I related the attributes for each business with 
how many comments and stars has. I select the total comments for each business with the average rate of stars and
and related with the groups of attributes.
So as you can see below, the business who accepts credit cards or has a pricerange 2 or business parking are who has more
reviews which means also that people use to go there and stay to comment.
The Avg Stars is to confirm that the reviews are not in a bad way.
                  
iii. Output of your finished dataset:

| TOTAL_BUSINESS | ATTRIBUTES                 | TOTAL_REVIEWS |     AVG_STARS |
--|--|--|--
|             74 | BusinessAcceptsCreditCards |          2370 | 3.69594594595 |
|             56 | RestaurantsPriceRange2     |          2164 | 3.40178571429 |
|             51 | BusinessParking            |          2130 | 3.50980392157 |
|             40 | BikeParking                |          1983 |        3.5375 |
|             35 | OutdoorSeating             |          1946 | 3.14285714286 |
|             26 | WiFi                       |          1781 | 3.38461538462 |
|             35 | RestaurantsTakeOut         |          1774 | 3.17142857143 |
|             37 | RestaurantsGoodForGroups   |          1745 | 3.06756756757 |
|             30 | HasTV                      |          1709 |          3.25 |
|             30 | Alcohol                    |          1707 | 3.21666666667 |
|             30 | NoiseLevel                 |          1706 | 3.23333333333 |
|             28 | Ambience                   |          1696 | 3.28571428571 |
|             34 | GoodForKids                |          1674 | 3.38235294118 |
|             31 | RestaurantsDelivery        |          1661 | 3.12903225806 |
|             31 | RestaurantsReservations    |          1639 |  3.1935483871 |
|             22 | Caters                     |          1579 | 3.38636363636 |
|             29 | RestaurantsAttire          |          1498 | 3.18965517241 |
|             28 | GoodForMeal                |          1493 | 3.26785714286 |
|             25 | RestaurantsTableService    |          1483 |          3.26 |
|             27 | WheelchairAccessible       |          1352 | 3.44444444444 |
|              8 | Music                      |           730 |        3.0625 |
|              7 | Smoking                    |           727 | 3.21428571429 |
|              6 | BestNights                 |           716 | 3.41666666667 |
|              6 | CoatCheck                  |           716 | 3.41666666667 |
|              6 | GoodForDancing             |           716 | 3.41666666667 |

*iv. Provide the SQL code you used to create your final dataset:*
	
	SELECT COUNT(attribute.business_id) as TOTAL_BUSINESS,          attribute.name AS ATTRIBUTES, sum(review_count) AS         TOTAL_REVIEWS, 
		avg(business.stars) AS AVG_STARS
	FROM attribute inner join business on business.id=attribute.business_id 
	GROUP BY attribute.name
	ORDER BY sum(review_count) DESC


















 

