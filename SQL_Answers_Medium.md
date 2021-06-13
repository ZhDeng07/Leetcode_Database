# Difficulty: Medium
## Language: MySQL
### Continuously updating...
---

> 1. Nth Highest Salary
> ``` MySQL
> create function getNthHighestSalary(N int) returns int
> begin
> set N=N-1;
> return(
>    select ifnull((select distinct salary from employee order by salary desc limit N,1),null)
>  );
> End
