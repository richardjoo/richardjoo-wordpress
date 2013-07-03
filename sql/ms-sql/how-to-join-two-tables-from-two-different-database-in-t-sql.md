Sooo,
You have two tables from two different databases and need to join. Fear not, it is rather ridiculously easier than you think. You will laugh hard after checking this code out.

Here is the example on getting clients from one database and records from the other database that belongs to the clients list from the other database. @_@ confused? me too.

Say again. Clients are in database A. Records are in database B. You want to make a report to find out the total monthly record count for each client. I throw one more twist. Getting the cancelled order count along with non-cancelled orders.

LOOOONG LOOOONG TIME AGO, I tried linked databases, but that was not too stable. So, I like this way better…
But down side is it only works if your two databases are in the same server… shame… #_#

Clients table in DatabaseA = DatabaseA.dbo.Clients
Appointments table in DatabaseB = DatabaseB.dbo.Appointments

Here is an example:

````sql
SELECT A.client_name, MONTH(B.appointment_date) AS [month],
     COUNT(B.appointment_id) AS [total_non_cancelled],
    (   SELECT COUNT(appointment_id) 
        FROM GIP.dbo.Appointments 
        WHERE client_id=A.id AND
            (appointment_date BETWEEN '2012-09-01' AND '2012-09-30') AND
            (cancelled=1)
    ) AS [total_cancelled]
FROM DatabaseA.dbo.Clients A 
    INNER JOIN DatabaseB.dbo.Appointments B ON A.id = B.client_id
WHERE (B.appointment_date BETWEEN '2012-09-01' AND '2012-09-30') AND
    (B.cancelled=0)
GROUP BY A.id,A.client_name, MONTH(B.appointment_date), A.rate_plan_amount
ORDER BY A.client_name
````
Then the result would look like this:

````sql
client_name     rate_plan_amount       month    total_non_cancelled    total_cancelled
client_1    199.00                 9    174                 9
client_2    499.00                 9    782                 82
client_3    99.00                  9    32                  2
client_4    99.00                  9    22                  0
client_5    99.00                  9    1                   0
````