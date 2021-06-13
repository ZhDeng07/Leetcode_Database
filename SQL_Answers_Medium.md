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
