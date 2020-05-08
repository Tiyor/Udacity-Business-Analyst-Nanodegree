    /* QUESTION 1. Who are the top 10 customers (worldwide) that spent the most on Jazz
             between 2009 and 2013? */
      
    SELECT c.firstname, 
           c.lastname, 
           c.country, 
           g.name, 
           Sum(inv.Quantity  * inv.UnitPrice) total 
    FROM   invoice i 
           JOIN customer c 
             ON c.customerid = i.customerid 
           JOIN invoiceline inv 
             ON i.invoiceid = inv.invoiceid 
           JOIN track t 
             ON t.trackid = inv.trackid 
           JOIN genre g 
             ON g.genreid = t.genreid 
    WHERE  g.genreid = 2 
    GROUP  BY c.CustomerId  
    ORDER  BY 5 DESC 
    LIMIT  10;
    
     /* QUESTION 2. Who have written the most rock music between 2009 and 2013 ?  */
   
    SELECT a.name           artist_name, 
           g.name           genre_name, 
           Count(*) total 
    FROM   track t 
           JOIN genre g 
             ON g.genreid = t.genreid 
           JOIN album al 
             ON al.albumid = t.albumid 
           JOIN artist a 
             ON a.artistid = al.artistid 
    WHERE  t.genreid = 1 
    GROUP  BY 1 
    ORDER  BY 3 DESC 
    LIMIT  10;
    
    
       /* QUESTION 3. Which artist has earned the most from 2009 to 2013 ? */
    
    SELECT artist_name, 
           Round(earnings, 2) revenue, 
           CASE 
             WHEN earnings > 90 THEN 'Top' 
             WHEN earnings > 40 
                  AND earnings < 90 THEN 'Middle' 
             ELSE 'Low' 
           end                AS categories 
    FROM   (SELECT a.name                        artist_name, 
                   Sum(i.unitprice * i.quantity) earnings 
            FROM   invoiceline i 
                   JOIN track t 
                     ON t.trackid = i.trackid 
                   JOIN album alb 
                     ON alb.albumid = t.albumid 
                   JOIN artist a 
                     ON a.artistid = alb.artistid 
            GROUP  BY 1 
            ORDER  BY 2 DESC) t1 
    LIMIT  15;
    
    
      /* Question 4. What employees contributed the most to the sale throughout 2009 to
        2013 ? */
    
    SELECT e.firstname, 
           e.lastname, 
           e.title, 
           (SELECT Round(Sum(i.total))) total_sale 
    FROM   invoice i 
           JOIN customer c 
             ON c.customerid = i.customerid 
           JOIN employee e 
             ON e.employeeid = c.supportrepid 
    GROUP  BY 1;
