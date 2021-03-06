# Groupdate.sql

The simplest way to group by:

- day
- week
- month
- day of the week
- hour of the day
- and more (complete list at bottom)

:tada: Time zones supported!!

:earth_americas: Use it with any programming language

Supports PostgreSQL and MySQL

[![Build Status](https://travis-ci.org/ankane/groupdate.sql.svg?branch=master)](https://travis-ci.org/ankane/groupdate.sql)

## Usage

Groupdate.sql creates SQL functions, which are stored by the database. You can use these functions with any ORM or without one. Times stored in your database are assumed to be UTC (this is the default for frameworks like Rails).

### SQL

Group by day

```sql
SELECT gd_day(created_at, 'America/Los_Angeles') AS day, COUNT(*) FROM users GROUP BY day;
-- result
    day     | count
------------+-------
 2013-04-11 |   145
 2013-04-12 |    65
 2013-04-13 |   293
```

Group by week

```sql
SELECT gd_week(created_at, 'America/Chicago') AS week, COUNT(*) FROM users GROUP BY week;
-- result
    week    | count
------------+-------
 2013-05-05 |  1327
 2013-05-12 |  3432
 2013-05-19 |   673
```

Group by day of week

```sql
SELECT gd_day_of_week(created_at, 'America/New_York') AS day_of_week, COUNT(*) FROM orders GROUP BY day_of_week;
-- result
 day_of_week | count
-------------+-------
           0 |   167
           1 |   273
           2 |   439
           3 |   285
           4 |   318
           5 |   123
           6 |   189
```

> Weeks start on Sunday by default

Group by dynamic period

```sql
SELECT gd_period('day', created_at, 'America/Los_Angeles') AS period, COUNT(*) FROM users GROUP BY period;
```

> Works with `day`, `week`, `month`, and `year`

### Time Zone

With PostgreSQL, the time zone is optional.

```sql
SELECT gd_day(created_at) AS day, COUNT(*) FROM users GROUP BY day;
```

The default is UTC. Change this to your preferred time zone by running:

```sql
CREATE OR REPLACE FUNCTION gd_time_zone()
  RETURNS text AS
$$
  SELECT 'America/Los_Angeles'::text;
$$
  LANGUAGE SQL IMMUTABLE;
```

### Week Start

Set the day weeks start. The default is Sunday.

For PostgreSQL, run:

```sql
CREATE OR REPLACE FUNCTION gd_week_start()
  RETURNS int AS
$$
  SELECT 6; -- mon=0, tue=1, wed=2, thu=3, fri=4, sat=5, sun=6
$$
  LANGUAGE SQL IMMUTABLE;
```

For MySQL, run:

```sql
START TRANSACTION;

DROP FUNCTION IF EXISTS gd_week_start;
CREATE FUNCTION gd_week_start()
  RETURNS INT
  RETURN 6; -- mon=0, tue=1, wed=2, thu=3, fri=4, sat=5, sun=6

COMMIT;
```

### Language / Frameworks

Ruby on Rails (ActiveRecord)

```ruby
User.group("gd_day(created_at, 'America/Los_Angeles')").count
```

Django, Node.js, Go, etc

```python
# pull requests please :)
```

## Installation

Run this SQL

- [PostgreSQL](https://raw.githubusercontent.com/ankane/groupdate.sql/master/postgresql/install.sql)
- [MySQL](https://raw.githubusercontent.com/ankane/groupdate.sql/master/mysql/install.sql)

For MySQL, [time zone support](https://dev.mysql.com/doc/refman/5.7/en/time-zone-support.html) must be installed on the server.

```sh
mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root mysql
```

## Complete list

All functions are prefixed with `gd_` to prevent conflicts with MySQL functions.

- second
- minute
- hour
- day
- week
- month
- year
- hour_of_day
- day_of_week

A list of time zones can be found [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

## Uninstall

Run this SQL

- [PostgreSQL](https://raw.githubusercontent.com/ankane/groupdate.sql/master/postgresql/uninstall.sql)
- [MySQL](https://raw.githubusercontent.com/ankane/groupdate.sql/master/mysql/uninstall.sql)

## Contributing

Everyone is encouraged to help improve this project. Here are a few ways you can help:

- [Report bugs](https://github.com/ankane/groupdate.sql/issues)
- Fix bugs and [submit pull requests](https://github.com/ankane/groupdate.sql/pulls)
- Write, clarify, or fix documentation
- Suggest or add new features
