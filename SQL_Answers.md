# Difficulty: Easy
## Language: MySQL
### Continuously updating...
---

> 1. Combine Two Tables
> ``` MySQL
> select FirstName, LastName, City, State from Person
> left join Address on Person.PersonID = Address.PersonID

---
> 2. Second Highest Salary
> ``` MySQL
> select max(Salary) SecondHighestSalary from Employee
> where Salary < (select max(distinct Salary) from Employee)

---
> 3. Employees Earning More Than Their Managers
> ``` MySQL
> SELECT 
>    ea.Name AS Employee
> FROM Employee ea 
> JOIN Employee eb
> ON ea.ManagerId = eb.Id
> AND ea.salary > eb.salary

---
> 4. Duplicate Emails
> ``` MySQL
> SELECT Email 
> FROM(
>    SELECT 
>        Email,
>        count(Email) c
>        FROM Person 
>        GROUP BY Email
>        HAVING c > 1) AS result;

---
> 5. Customers Who Never Order
> ``` MySQL
> select Name Customers from Customers c
> left join Orders o on c.ID = o.CustomerID
> where o.CustomerID is null

---
> 6. Delete Duplicate Emails
> ``` MySQL
> DELETE FROM person 
> WHERE id NOT IN(
>    SELECT t.min_id from (SELECT MIN(id) AS min_id FROM person GROUP BY Email
> ) AS t
> )

---
> 7. Rising Temperature
> ``` MySQL
> SELECT weather.id AS id
> FROM weather
> JOIN weather w1 ON DATEDIFF(weather.recorddate, w1.recorddate) = 1
> AND weather.temperature > w1.temperature

---
> 8. Game Play Analysis I
> ``` MySQL
> select player_id, event_date as first_login
> from(select player_id, event_date, row_number() over(partition by player_id order by event_date) rn
> from activity) a
> where rn = 1

---
> 9. Game Play Analysis II
> ``` MySQL
> select player_id, device_id
> from(
>    select 
>        player_id,
>        device_id,
>        row_number() over(partition by player_id order by event_date) as ranked
>    from activity
> ) as ranked
> where ranked = 1

---
> 10. Employee Bonus
> ``` MySQL
> select name, bonus
> from employee e
> left join bonus b on e.empId=b.empId
> where bonus < 1000 or bonus is null

---
> 11. Find Customer Referee
> ``` MySQL
> select name 
> from customer
> where not referee_id = 2 or referee_id is null

---
> 12. Customer Placing the Largest Number of Orders
> ``` MySQL
> select customer_number
> from orders
> group by customer_number
> order by count(*) desc
> limit 1

---
> 13. Big Countries
> ``` MySQL
> select name, population, area
> from world
> where area > 3000000 or population > 25000000

---
> 14. Classes More Than 5 Students
> ``` MySQL
> select class
> from courses
> group by class
> having count(*) >= 5

---
> 15. Friend Requests I: Overall Acceptance Rate
> ``` MySQL
> select ifnull(round(count(distinct requester_id, accepter_id)/(select count(distinct sender_id, send_to_id)
> from friendrequest), 2),0) AS accept_rate
> from requestaccepted

---
> 16. Consecutive Available Seats
> ``` MySQL
> select seat_id
> from cinema
> where free = 1 
> and (seat_id + 1 in (select seat_id from cinema where free = 1) or seat_id - 1 in(select seat_id from cinema where free = 1))
> order by seat_id 

---
> 17. Sales Person
> ``` MySQL
> select s.name
> from salesperson s
> where s.sales_id not in(
>     select o.sales_id
>     from orders o
>     join company c on o.com_id = c.com_id
>     where c.name = 'RED'
> )

---
> 18. Triangle Judgement
> ``` MySQL
> select x, y, z,
>     case
>         when x + y > z and x + z > y and y + z >x then 'Yes'
>         else 'No'
>     end as triangle
> from triangle

---
> 19. Shortest Distance in a Line
> ``` MySQL
> select 
>     min(abs) as shortest
> from(
>     select 
>         p.x as x1,
>         pp.x as x2,
>         abs(pp.x - p.x) as abs 
>     from point p
>     join point pp
>     on p.x = p.x
> ) as t
> where abs > 0 

---
> 20. Biggest Single Number
> ``` MySQL
> select 
>    max(num) as num
> from(
>    select *
>    from my_numbers
>    group by num 
>    having count(num) = 1
> ) as t

---
> 21. Not Boring Movies
> ``` MySQL
> select 
>     id,
>     movie,
>     description,
>     rating
> from cinema
> where ( id % 2 ) > 0 and description  != 'boring'
> order by rating desc
 
---
> 22. Swap Salary
> ``` MySQL
> update salary
> set sex = case
>     when sex = 'm' then 'f'
>     else 'm'
>     end

---
> 23. Actors and Directors Who Cooperated At Least Three Times
> ``` MySQL
> select 
>     actor_id,
>     director_id
> from actordirector
> group by actor_id, director_id
> having count(timestamp) >= 3

---
> 24. Product Sales Analysis I
> ``` MySQL
> select 
>     product_name,
>     year,
>     price
> from product p
> join sales s using(product_id)
> order by year


---
> 25. Product Sales Analysis II
> ``` MySQL
> select 
>  product_id,
>  sum(quantity) as total_quantity
> from sales
> group by product_id

---
> 26. Project Employees I
> ``` MySQL
> select
>     project_id,
>     round(avg(experience_years), 2) as average_years
> from(
>     select * 
> from project
> join employee using(employee_id) 
> ) as t
> group by project_id
> order by project_id

---
> 27. Project Employees II
> ``` MySQL
> select
>     project_id
> from project
> group by project_id
> having count(employee_id) = (select count(employee_id) as c from project group by project_id order by c desc limit 1)


---
> 28. Sales Analysis I
> ``` MySQL
> select seller_id
> from (select seller_id, rank() over(order by sum(price) desc) as rn
> from sales
> group by seller_id) as t
> where rn = 1

---
> 29. Sales Analysis II
> ``` MySQL
> select buyer_id
> from sales s
> join product p  on s.product_id = p.product_id
> group by buyer_id
> having max(product_name = 'S8') = 1 and max(product_name = 'iPhone') = 0

---
> 30. Sales Analysis III
> ``` MySQL
> select p.product_id, p.product_name
> from product p 
> join sales using(product_id)
> where sale_date between '2019-01-01' and '2019-03-31'  and product_id not in (select
>             distinct product_id from sales where sale_date not between '2019-01-01' and '2019-03-31')

---
> 31. Reported Posts
> ``` MySQL
> select extra as report_reason, count(distinct post_id) as report_count
> from(
> select *
> from actions
> where action_date = '2019-07-05' - interval 1 day and extra is not null and action = 'report'
> ) as t
> group by extra

---
> 32. User Activity for the Past 30 Days I
> ``` MySQL
> Select
> activity_date as day,
> count(distinct user_id) as active_users 
> from Activity
> where activity_date between date_sub('2019-07-27', INTERVAL 29 DAY) AND '2019-07-27'
> group by activity_date

---
> 33. User Activity for the Past 30 Days II
> ``` MySQL
> select ifnull((select round(count(distinct session_id) / count(distinct user_id), 2 )
> from activity
> where activity_date between date_sub('2019-07-27', interval 29 day) and '2019-07-27'), 0) as average_sessions_per_user

---
> 34. Article Views I
> ``` MySQL
> select distinct author_id as id
> from views
> where author_id = viewer_id
> order by author_id

---
> 35. Immediate Food Delivery I
> ``` MySQL
> select round( (select count(delivery_id) 
> from delivery 
> where order_date = customer_pref_delivery_date) * 100 / count(delivery_id), 2) as immediate_percentage
> from delivery


---
> 36. Reformat Department Table
> ``` MySQL
> select id,
> sum(case when month = 'jan' then revenue end) as Jan_revenue,
> sum(case when month = 'feb' then revenue end) as Feb_revenue,
> sum(case when month = 'mar' then revenue end) as Mar_revenue,
> sum(case when month = 'apr' then revenue end) as Apr_revenue,
> sum(case when month = 'may' then revenue end) as May_revenue,
> sum(case when month = 'jun' then revenue end) as Jun_revenue,
> sum(case when month = 'jul' then revenue end) as Jul_revenue,
> sum(case when month = 'aug' then revenue end) as Aug_revenue,
> sum(case when month = 'sep' then revenue end) as Sep_revenue,
> sum(case when month = 'oct' then revenue end) as Oct_revenue,
> sum(case when month = 'nov' then revenue end) as Nov_revenue,
> sum(case when month = 'dec' then revenue end) as Dec_revenue
> from department
> group by id

---
> 37. Queries Quality and Percentage
> ``` MySQL
>  select query_name, round((sum(rating/position)/count(query_name)), 2) as quality,
>  round(sum(case when rating < 3 then 1 else 0 end)*100 / count(rating), 2) as poor_query_percentage
>  from queries
>  group by query_name

---
> 38. Number of Comments per Post
> ``` MySQL
> select post_id, ifnull(number_of_comments, 0) as number_of_comments
> from
> (select distinct s.sub_id as post_id from submissions s where s.parent_id is null) as t
> left join 
> (select ss.parent_id, count(distinct ss.sub_id) as number_of_comments
> from submissions ss
> group by ss.parent_id
> )  as s
> on t.post_id = s.parent_id
> order by post_id

---
> 39. Average Selling Price
> ``` MySQL
> select product_id,
> round((sum(price * units) / sum(units) ), 2) as average_price
> from
> (select * from prices left join unitssold using (product_id)
> where purchase_date between start_date and end_date) as t
> group by product_id


---
> 40. Students and Examinations
> ``` MySQL
> with r as(select s.student_id, student_name, sb.subject_name, e.subject_name as exams_subject
> from students s
> cross join subjects sb
> left join examinations e on s.student_id = e.student_id and sb.subject_name = e.subject_name)
> select student_id, student_name, subject_name, count(exams_subject) as attended_exams
> from r
> group by student_id, student_name, subject_name
> order by student_id, subject_name

---
> 41. Weather Type in Each Country
> ``` MySQL
> select c.country_name, 
> case 
>     when avg(w.weather_state) <= 15 then 'Cold'
>     when avg(w.weather_state) >= 25 then 'Hot'
>     else 'Warm'
> End as weather_type
> from countries c
> left join weather w
> on c.country_id = w.country_id
> where w.day like '2019-11%'
> group by c.country_id

---
> 42. Find the Team Size
> ``` MySQL
> with t as(select team_id, count(employee_id) as team_size
> from employee
> group by team_id)
> select employee_id, team_size
> from employee
> join t using (team_id)

---
> 43. Ads Performance
> ``` MySQL
> with t1 as(select 
>     ad_id,
>     round(sum(if(action = 'Clicked', 1, 0)) / count(ad_id) *100, 2) as ctr
> from Ads
> where action in ('Clicked', 'Viewed')
> group by ad_id),
> t2 as(
> select distinct ad_id
> from ads
> )
> select t2.ad_id, coalesce(ctr, 0) as ctr
> from t2
> left join t1
> on t1.ad_id = t2.ad_id
> order by t1.ctr desc,  t2.ad_id asc

---
> 44. List the Products Ordered in a Period
> ``` MySQL
> with t as(select product_id, sum(unit) as unit
> from orders
> where order_date like '2020-02%'
> group by product_id
> having sum(unit) >= 100)
> select product_name as PRODUCT_NAME, t.unit as 'UNIT'
> from products
> join t using(product_id)


---
> 45. Students With Invalid Departments
> ``` MySQL
> with t as(
> select department_id
> from students s
> join departments d
> on s.department_id = d.id
> )
> select id, name
> from students
> where department_id not in (select * from t)

---
> 46. Replace Employee ID With The Unique Identifier
> ``` MySQL
> select unique_id, name
> from employees es
> left outer join employeeuni ei
> on es.id = ei.id

---
> 47. Top Travellers
> ``` MySQL
> select name, ifnull(sum(distance), 0 )as travelled_distance
> from users u
> left join rides r
> on u.id = r.user_id
> group by name
> order by travelled_distance desc, name asc

---
> 48. Create a Session Bar Chart
> ``` MySQL
> select "[0-5>" as BIN, count( case when duration >= 0 and duration < 300 then session_id end) as TOTAL from sessions
> union
> select "[5-10>" as BIN, count( case when duration >= 300 and duration < 600 then session_id end) as TOTAL from sessions
> union
> select "[10-15>" as BIN, count( case when duration >= 600 and duration < 900 then session_id end) as TOTAL from sessions
> union
> select "15 or more" as BIN, count( case when duration > 900 then session_id end) as TOTAL from sessions

---
> 49. Group Sold Products By The Date
> ``` MySQL
> select sell_date, count(distinct product) as num_sold,
> group_concat(distinct product order by product asc separator ',') as products
> from activities
> group by sell_date

---
> 50. Friendly Movies Streamed Last Month
> ``` MySQL
> select distinct title as TITLE
> from content
> join tvprogram using(content_id)
> where program_date regexp '^2020-06' and kids_content = 'Y' and content_type = 'Movies'

---
> 51. Customer Order Frequency
> ``` MySQL
> with t as (
> select customer_id, sum(case when date_format(order_date,'%m')='06' then p.price*quantity end) monthlycost6,
> sum(case when date_format(order_date,'%m')='07' THEN p.price*quantity end) monthlycost7
> from orders o
> join product p using(product_id)
> group by customer_id
> having monthlycost6 >=100 and monthlycost7 >=100
> )
> select customer_id, name
> from customers
> where customer_id in (select customer_id from t)

---
> 52. Find Users With Valid E-Mails
> ``` MySQL
> select *
> from users
> where mail regexp '^[a-zA-Z][a-zA-Z0-9_./-]*@leetcode.com$'

---
> 53. Patients With a Condition
> ``` MySQL
> select * 
> from patients 
> where conditions like 'DIAB1%' or conditions like '% DIAB1%'

---
> 54. Fix Product Name Format
> ``` MySQL
> with t as (
> select sale_id, lower(trim(product_name)) as product_name, date_format(sale_date, "%Y-%m") as sale_date
> from sales)
> select product_name, sale_date, count(sale_id) as total
> from t
> group by 1,2
> order by product_name, sale_date

---
> 55. Unique Orders and Customers Per Month
> ``` MySQL
> select DATE_FORMAT(order_date,'%Y-%m') as month, count(distinct order_id) as order_count,count(distinct customer_id) as customer_count
> from orders
> where invoice > 20
> group by 1

---
> 56. Warehouse Manager
> ``` MySQL
> select name as warehouse_name, sum(width*length*height*units) as volume
> from warehouse
> join products using(product_id)
> group by name

---
> 57. Customer Who Visited but Did Not Make Any Transactions
> ``` MySQL
> select customer_id, count(customer_id) as count_no_trans
> from visits v
> left join transactions t
> using(visit_id)
> where transaction_id is null 
> group by customer_id

---
> 58. Bank Account Summary II
> ``` MySQL
> with t as(
> select account, sum(amount) as balance
> from transactions
> group by account
> having balance > 10000
> )
> select name, balance
> from users
> join t using(account)

---
> 59. Sellers With No Sales
> ``` MySQL
> select seller_name
> from seller
> where seller_id not in (select distinct seller_id
> from orders
> where sale_date regexp '^2020')
> order by seller_name 

---
> 60. All Valid Triplets That Can Represent a Country
> ``` MySQL
> select *
> from schoola 
> cross join schoolb 
> cross join schoolc

---
> 61. Percentage of Users Attended a Contest
> ``` MySQL
> select contest_id, round(count(distinct user_id) * 100 / (select count(distinct user_id) from users), 2)as percentage
> from register 
> group by contest_id
> order by percentage desc, contest_id asc

---
> 62. Average Time of Process per Machine
> ``` MySQL
> with st as(select * from activity where activity_type = 'start'),
> et as( select * from activity where activity_type = 'end')
> select machine_id, round(sum(et.timestamp - st.timestamp) / count(st.machine_id), 3) as processing_time
> from st
> join et using(machine_id, process_id)
> group by machine_id

---
> 63. Fix Names in a Table
> ``` MySQL
> select user_id, concat(upper(substring(name, 1, 1)), '', lower(substring(name, 2, length(name)-1))) as name
> from users
> order by user_id


---
> 64. Product's Worth Over Invoices
> ``` MySQL
> select name, sum(rest) as rest, sum(paid) as paid, sum(canceled) as canceled, sum(refunded) as refunded 
> from product
> join invoice using(product_id)
> group by product_id
> order by name

---
> 65. Invalid Tweets 
> ``` MySQL
> select tweet_id
> from tweets
> where length(content) > 15

--- 
> 66. Daily Leads and Partners
> ``` MySQL
> select date_id, make_name, count(distinct lead_id) as unique_leads, count(distinct partner_id) as unique_partners
> from dailysales
> group by 1,2

--- 
> 67. Find Followers Count
> ``` MySQL
> select distinct user_id, count(follower_id) as followers_count
> from followers
> group by 1
> order by user_id

--- 
> 68. The Number of Employees Which Report to Each Employee
> ``` MySQL
> select e1.reports_to as employee_id, e2.name, count(e1.name) as reports_count, round(avg(e1.age),0) as average_age
> from employees e1, employees e2 
> where e1.reports_to = e2.employee_id
> group by 1,2
> order by 1,2
