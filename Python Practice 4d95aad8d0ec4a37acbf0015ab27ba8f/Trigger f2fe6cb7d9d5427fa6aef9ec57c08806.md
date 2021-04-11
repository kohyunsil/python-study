# Trigger

```sql
# trigger
# 특정 테이블을 감시하고 있다가 설정한 조건이 감지되면 미리 작성해 놓은 쿼리를 실행

create database tr;
use tr;
create table chat(
	chat_id int primary key auto_increment,
    msg varchar(200) not null
);
create table backup(
	backup_id int primary key auto_increment,
    msg varchar(200) not null,
    bdate timestamp default current_timestamp
);

delimiter |
create trigger backup_tr
before delete on chat
for each row begin
 insert into backup(msg) value(old.msg);
end |
show triggers;

insert into chat(msg) values ("hello"), ("hi"), ("hello!!!");
select * from chat;
select * from backup;

delete from chat
where msg like "%hello%"
limit 10;
# 복원
insert into chat(msg)
select msg from backup;
```