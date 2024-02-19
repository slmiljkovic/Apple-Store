# Apple-Store
Data set that converts informayion about apps avaible on apple store from Kaggle
Using SQLite 3 analyzing apps and trends
1. Whitch apps have better training
2. Apps  with how many languages are the best
3. Witch apps have low ratings
4. New apps must have average rating it witch scope
5. Games and entartaiment apps have higer competition


SQL code is 

SELECT * FROM appleStore ; 

/* finding average rating for free apps and paid apps*/

SELECT CASE
       WHEN price = '0' THEN 'Free'
       ELSE 'Paid'
       END AS Ap_type
       ,AVG(user_rating) AS ap_rating
       FROM  appleStore
       GROUP BY Ap_type;
       
 SELECT * FROM appleStore ; 
 
 /*check number of language ratings compare with ap ratings*/     

SELECT CASE
       WHEN lang_num < '10' THEN 'less than 10'
       WHEN lang_num BETWEEN '10' and '30' THEN 'Beetwen 10 and 20'
       ELSE 'More than 30'
       END AS Ap_language_basket,
AVG(user_rating) AS ap_rating
FROM  appleStore
GROUP BY Ap_language_basket
ORDER BY   ap_rating DESC;

/*  check genres with low rating*/
  
SELECT prime_genre,
       AVG(user_rating) AS ap_rating
  FROM  appleStore     
  GROUP BY  prime_genre
ORDER BY   ap_rating ASC
LIMIT 10;

/* check is there corelation between the length of app description */

 SELECT  CASE
          WHEN length(b.app_desc)<500 THEN 'LOW'
          WHEN length(b.app_desc) BETWEEN 500 AND 1000 THEN 'MEDIUM'
          ELSE 'HIGH'
         END AS apple_storedesc,
  AVG(a.user_rating) AS ap_rating
  FROM  appleStore a
  JOIN     appleStore_description_combined b
  ON a.id=b.id 
  GROUP BY apple_storedesc
  ORDER BY  ap_rating DESC;
  
/* check the top rated aps for each genre*/

SELECT prime_genre,
       track_name,
       user_rating
 FROM( 
       SELECT
       prime_genre,
       track_name,
       user_rating,
       RANK() OVER(PARTITION BY prime_genre ORDER BY user_rating DESC,rating_count_tot DESC) AS rank FROM appleStore )    AS a   
 WHERE a.rank=1 
