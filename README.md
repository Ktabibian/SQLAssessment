# SQLAssessment
```
mysql> create table movie(
    -> id int unsigned NOT NULL AUTO_INCREMENT PRIMARY KEY,
    -> Title varchar(30) NOT NULL DEFAULT '',
    -> Runtime int(10) unsigned NOT NULL DEFAULT 0,
    -> Genre varchar(20) NOT NULL DEFAULT '',
    -> IMDB_Score decimal(3,1) NOT NULL DEFAULT 99.9,
    -> Rating char(10) NOT NULL DEFAULT ''
    -> );
```
```
mysql> alter table movie alter column id set invisible;
```
```
mysql> describe movie;
+------------+--------------+------+-----+---------+--------------------------+
| Field      | Type         | Null | Key | Default | Extra                    |
+------------+--------------+------+-----+---------+--------------------------+
| id         | int unsigned | NO   | PRI | NULL    | auto_increment INVISIBLE |
| Title      | varchar(30)  | NO   |     |         |                          |
| Runtime    | int unsigned | NO   |     | 0       |                          |
| Genre      | varchar(20)  | NO   |     |         |                          |
| IMDB_Score | decimal(3,1) | NO   |     | 99.9    |                          |
| Rating     | char(10)     | NO   |     |         |                          |
+------------+--------------+------+-----+---------+--------------------------+
```
```
mysql> insert into movie value('Howard the Duck', 110, 'Sci-Fi', 4.6, 'PG'),
    -> ('Lavalantula', 83, 'Horror', 4.7, 'TV-14'),
    -> ('Starship Troopers', 129, 'Sci-Fi', 7.2, 'PG-13'),
    -> ('Waltz With Bashir', 90, 'Documentary', 8.0, 'R'),
    -> ('Spaceballs', 96, 'Comedy', 7.1, 'PG'),
    -> ('Monsters Inc.', 92, 'Animation', 8.1, 'G'),
    -> ('Dredd', 95, 'Action', 7.1, 'R'),
    -> ('Blade Runner 2049', 163, 'Sci-Fi', 8.0, 'R');
```
```
mysql> select * from movie;
+-------------------+---------+-------------+------------+--------+
| Title             | Runtime | Genre       | IMDB_Score | Rating |
+-------------------+---------+-------------+------------+--------+
| Howard the Duck   |     110 | Sci-Fi      |        4.6 | PG     |
| Lavalantula       |      83 | Horror      |        4.7 | TV-14  |
| Starship Troopers |     129 | Sci-Fi      |        7.2 | PG-13  |
| Waltz With Bashir |      90 | Documentary |        8.0 | R      |
| Spaceballs        |      96 | Comedy      |        7.1 | PG     |
| Monsters Inc.     |      92 | Animation   |        8.1 | G      |
| Dredd             |      95 | Action      |        7.1 | R      |
| Blade Runner 2049 |     163 | Sci-Fi      |        8.0 | R      |
+-------------------+---------+-------------+------------+--------+
```
```
mysql> select * from movie where genre = 'Sci-Fi';
+-------------------+---------+--------+------------+--------+
| Title             | Runtime | Genre  | IMDB_Score | Rating |
+-------------------+---------+--------+------------+--------+
| Howard the Duck   |     110 | Sci-Fi |        4.6 | PG     |
| Starship Troopers |     129 | Sci-Fi |        7.2 | PG-13  |
| Blade Runner 2049 |     163 | Sci-Fi |        8.0 | R      |
+-------------------+---------+--------+------------+--------+
```
```
mysql> select * from movie where IMDB_Score >= 6.5;
+-------------------+---------+-------------+------------+--------+
| Title             | Runtime | Genre       | IMDB_Score | Rating |
+-------------------+---------+-------------+------------+--------+
| Starship Troopers |     129 | Sci-Fi      |        7.2 | PG-13  |
| Waltz With Bashir |      90 | Documentary |        8.0 | R      |
| Spaceballs        |      96 | Comedy      |        7.1 | PG     |
| Monsters Inc.     |      92 | Animation   |        8.1 | G      |
| Dredd             |      95 | Action      |        7.1 | R      |
| Blade Runner 2049 |     163 | Sci-Fi      |        8.0 | R      |
+-------------------+---------+-------------+------------+--------+
```
```
mysql> select * from movie where runtime < 100 and rating like '_G%' or rating like 'G%';
+---------------+---------+-----------+------------+--------+
| Title         | Runtime | Genre     | IMDB_Score | Rating |
+---------------+---------+-----------+------------+--------+
| Spaceballs    |      96 | Comedy    |        7.1 | PG     |
| Monsters Inc. |      92 | Animation |        8.1 | G      |
+---------------+---------+-----------+------------+--------+
```
```
mysql> select *, AVG(runtime) from movie where imdb_score <7.5 group by genre;
+-----------------+---------+--------+------------+--------+--------------+
| Title           | Runtime | Genre  | IMDB_Score | Rating | AVG(runtime) |
+-----------------+---------+--------+------------+--------+--------------+
| Howard the Duck |     110 | Sci-Fi |        4.6 | PG     |     119.5000 |
| Lavalantula     |      83 | Horror |        4.7 | TV-14  |      83.0000 |
| Spaceballs      |      96 | Comedy |        7.1 | PG     |      96.0000 |
| Dredd           |      95 | Action |        7.1 | R      |      95.0000 |
+-----------------+---------+--------+------------+--------+--------------+
```
```
mysql> update movie set rating = 'R' where title = 'Starship Troopers';
```
```
mysql> select * from movie;
+-------------------+---------+-------------+------------+--------+
| Title             | Runtime | Genre       | IMDB_Score | Rating |
+-------------------+---------+-------------+------------+--------+
| Howard the Duck   |     110 | Sci-Fi      |        4.6 | PG     |
| Lavalantula       |      83 | Horror      |        4.7 | TV-14  |
| Starship Troopers |     129 | Sci-Fi      |        7.2 | R      |
| Waltz With Bashir |      90 | Documentary |        8.0 | R      |
| Spaceballs        |      96 | Comedy      |        7.1 | PG     |
| Monsters Inc.     |      92 | Animation   |        8.1 | G      |
| Dredd             |      95 | Action      |        7.1 | R      |
| Blade Runner 2049 |     163 | Sci-Fi      |        8.0 | R      |
+-------------------+---------+-------------+------------+--------+
```
```
mysql> select id, rating from movie where genre = 'Horror' or genre = 'Documentary';
+----+--------+
| id | rating |
+----+--------+
|  2 | TV-14  |
|  4 | R      |
+----+--------+
```
```
mysql> select rating, AVG(imdb_score), MAX(imdb_score), MIN(imdb_score) from movie group by rating;
+--------+-----------------+-----------------+-----------------+
| rating | AVG(imdb_score) | MAX(imdb_score) | MIN(imdb_score) |
+--------+-----------------+-----------------+-----------------+
| PG     |         5.85000 |             7.1 |             4.6 |
| TV-14  |         4.70000 |             4.7 |             4.7 |
| R      |         7.57500 |             8.0 |             7.1 |
| G      |         8.10000 |             8.1 |             8.1 |
+--------+-----------------+-----------------+-----------------+
```
```
mysql> select rating, COUNT(rating) from movie group by rating having COUNT(rating) > 1;
+--------+---------------+
| rating | COUNT(rating) |
+--------+---------------+
| PG     |             2 |
| R      |             4 |
+--------+---------------+
```
```
mysql> delete from movie where rating = 'R';
```
```
mysql> select * from movie;
+-----------------+---------+-----------+------------+--------+
| Title           | Runtime | Genre     | IMDB_Score | Rating |
+-----------------+---------+-----------+------------+--------+
| Howard the Duck |     110 | Sci-Fi    |        4.6 | PG     |
| Lavalantula     |      83 | Horror    |        4.7 | TV-14  |
| Spaceballs      |      96 | Comedy    |        7.1 | PG     |
| Monsters Inc.   |      92 | Animation |        8.1 | G      |
+-----------------+---------+-----------+------------+--------+
```
