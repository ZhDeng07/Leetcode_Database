# Difficulty: Medium
## Language: MySQL
### Continuously updating...
---

> 1. Nth Highest Salary
> ``` MySQL
> -- simple solution:
> create function getNthHighestSalary(N int) returns int
> begin
> set N=N-1;
> return(
>    select ifnull((select distinct salary from employee order by salary desc limit N,1),null)
>  );
> end
>
> -- window function solution:
> create function getNthHighestSalary(N int)
> returns int
> begin
> return(
>     with t as 
>     (select distinct salary, 
>      dense_rank () over(order by salary desc) as rn
>     from employee)
>     select salary 
>     from t 
>     where rn = N
> );
> end

> 2. Rank Scores
> ``` MySQL
> select score as 'Score', dense_rank() over(order by Score desc) as 'Rank' #
> from Scores
> order by 'Rank' asc

> 3. Consecutive Numbers
> ``` MySQL
> with t as(
> select 
> lag(num, 1) over(order by id) as num1,
> num, 
> lead(num, 1) over(order by id) as num2
> from logs
> )
> select distinct num as ConsecutiveNums
> from t
> where num1=num2 and num=num1



