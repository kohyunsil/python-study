# DATABASE_3

```sql
use world;

# group by
# agg  : count, max, min, sum, avg

# countrylanguage 테이블에서 전체 언어의 갯수가 몇개인지

select count(distinct(language))
from countrylanguage;

select language, count(language) as count
from countrylanguage
group by language
order by count desc;

#country 테이블에서 대륙별 총 인구수를 출력하고 
#인구가 많은 대륙 순으로 1~3위까지 출력

select continent, sum(population) as population
from country
group by continent
order by population desc
limit 3;

# 스탭별 총 매출
use sakila;
 
select staff_id, sum(amount) as amount
from payment
group by staff_id;

# 월별 총 매출을 출력하세요 
# date_format

select date_format(payment_date, "%m") as payment_month, sum(amount)
from payment
group by payment_month;

# having (쿼리순서때문에 where 절 사용이 불가함. having은 가장 마지막 조건 실행이므로 마지막에 사용 가능)
# 대륙별로 전체 인구를 출력하고 대륙의 전체인구가 5억 이상인 대륙만 출력
use world;
select continent, sum(population) as population
from country
group by continent
having population >= 500000000;

# with rollup
# sakila에서 스탭별, 고객별 매출을 출력
use sakila;
select ifnull(customer_id,"total") as customer_id
		, ifnull(staff_id, "total") as staff_id
		, sum(amount) as amount
from payment
group by customer_id, staff_id
with rollup;    # 총합 데이터 출력

use world;
select ifnull(continent,"total") as continent
, ifnull(region,"total") as region
, sum(population) as population
from country
group by continent, region
with rollup;

# rank 설정
SET @number = 0;
select @number;

select @number := @number + 1 as ranking, name, population
from city
order by population desc
limit 5;
```

```sql
create database join_test;
use join_test;
create table user(
	user_id int primary key auto_increment,
    name varchar(30)
);
create table addr(
	addr_id int primary key auto_increment,
    addr varchar(30),
    user_id int
);
insert into user(name) values ("Jin"), ("Po"), ("Alice");
select * from user;

insert into addr(addr, user_id)
values ("Seoul", 1), ("Pusan", 2), ("Daegu",4), ("Seoul", 5);
select * from addr;

# INNER JOIN
select * from user; # 3
select * from addr; # 4

select user.user_id, user.name, addr.addr
from user
join addr # 3*4 (inner join)
where user.user_id = addr.user_id;

select user.user_id, user.name, addr.addr
from user
join addr 
on user.user_id = addr.user_id;

select user.user_id, user.name, addr.addr
from user, addr 
where user.user_id = addr.user_id;

## Left join
select user.user_id, user.name, addr.addr
from user
left join addr 
on user.user_id = addr.user_id;

## right join
select addr.user_id, user.name, addr.addr
from user
right join addr 
on user.user_id = addr.user_id;

use world;
# 도시이름(city), 국가이름(country), 국가인구, 도시인구 출력
select city.name as city_name
, country.name as country_name
, country.population as country_population
, city.population as city_population
, city.population / country.population * 100 as rate
from city
join country
on city.countrycode = country.code
order by city_population desc
limit 5;

# 도시인구가 900만 이상인 도시의 도시이름, 국가이름, 국가인구, 도시인구
# 내림차순으로 정렬하여 출력
select city.name as city_name
, country.name as country_name
, country.population as country_population
, city.population as city_population
from city
join country
on city.countrycode = country.code
having city_population >= 900*10000
order by city_population desc;

# 국가이름, 도시이름, 사용언어, 언어사용률(%), 도시에서 해당언어 사용 인구수 출력
# 국가에서 사용하는 언어의 비율은 도시와 같다
select city.name, country.name
, countrylanguage.language, countrylanguage.percentage
, round(city.population * countrylanguage.percentage / 100)
from country, city, countrylanguage
where country.code = city.countrycode
and city.countrycode = countrylanguage.countrycode;

## outer join
# union : 두개의 중복데이터를 제거해주는 기능도 추가적으로 있다.
use join_test;

select name from user
union all # 중복데이터를 제거하지 않으려먼 all
select addr from addr;

select name from user
union
select addr from addr;

select user.user_id, user.name, addr.addr
from user
left join addr
on user.user_id = addr.user_id
union
select addr.user_id, user.name, addr.addr
from user
right join addr
on user.user_id = addr.user_id;
```

```sql
# sub query : 쿼리 안에 쿼리를 실행
# select, from, where 절 등에 사용 가능

# select절
# 전체 나라수, 전체 도시수, 전체 언어수를 1row + 3column으로 출력
use world;
select (select count(*) from city) as city_count
	,(select count(*) from country) as country_count
	,(select count(distinct(language)) from countrylanguage) as language_count
from dual;

## from 절 sub query
# 900만 이상이 되는 도시의 국가코드, 국가이름, 도시이름, 도시인구수 출력
select country.code, country.name, city.name, city.population
from city
join country
on city.countrycode = country.code
having city.population >= 900*10000;

select country.code, country.name, city.name, city.population
from (select countrycode, name, population
	 from city
     where population >= 900*10000 ) as city
join country
on city.countrycode = country.code;

### where 절 sub query
# 한국의 인구보다 더 많은 인구를 가진 국가의 국가코드, 국가이름, 인구수를 출력
select population from country where code = "KOR";

select code, name, population
from country
where population > (select population
					from country
                    where code = "KOR");
                    
# any(or) / all(and)
select code, name, population
from country
where population > any (select population
					from country
                    where code in ("KOR", "BRA"));
                    
select code, name, population
from country
where population > all (select population
					from country
                    where code in ("KOR", "BRA"));
                    
# 지역과 대륙별 사용하는 언어출력
select distinct country.continent, country.region, countrylanguage.language
from country 
join countrylanguage
where country.code = countrylanguage.countrycode;

# 대륙과 지역별 사용하는 언어의 수를 출력
select data.continent,data.region, count(data.language)
from (
	select distinct country.continent, country.region, countrylanguage.language
	from country 
	join countrylanguage
	where country.code = countrylanguage.countrycode
    
) as data
group by data.continent, data.region;
```