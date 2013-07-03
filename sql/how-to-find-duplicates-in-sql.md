Finding duplicate rows with your code can be resource intensive if you use bad technique. It would be much better to have SQL server to do their job.

I use this way all the time.
Why? Because you can define “Duplicate” after how many duplicate records being found.

This will find duplicate record counts more than one.

````sql
SELECT user_name, COUNT(user_name) AS total_dups
FROM users
GROUP BY user_name
HAVING ( COUNT(user_name) > 1 )
````

This will find duplicate record counts more than two (three or more duplicated records).

````sql
SELECT user_name, COUNT(user_name) AS total_dups
FROM users
GROUP BY user_name
HAVING ( COUNT(user_name) > 2 )
````

This will find NO Duplicated records only. Just having one record will be counted and returned

````sql
SELECT user_name
FROM users
GROUP BY user_name
HAVING ( COUNT(user_name) = 1 )
````

So, what if you want to duplicate on more than one field?
I don’t know :-\  ... just kidding :D

Use AND

like
````sql
SELECT user_first_name, user_last_name,
COUNT(user_first_name) AS total_first_name_dups,
COUNT(user_last_name) AS total_last_name_dups,
FROM users
GROUP BY user_first_name, user_last_name
HAVING ( COUNT(user_first_name) > 1 ) AND ( COUNT(user_last_name) > 1 )
````
