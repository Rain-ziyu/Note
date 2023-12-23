# 简介

RediSearch提供了一种简单快速的方法来使用任何字段（二级索引）对数据进行索引和查询，并对已索引的数据集进行搜索和聚合。

使用[redis-stack](https://redis.io/docs/stack/get-started/install/docker/)映像来使用[redisearch 模块](https://redis.io/docs/stack/search/)。其实redis-stack就是一个redis的模块。

# 安装

您有多种方式来运行 RediSearch：

* 从[源代码](https://github.com/RediSearch/RediSearch)构建并将其安装到现有的 Redis 实例中
* 使用[Redis Cloud ](https://redislabs.com/redis-enterprise-cloud/)*（当 RediSearch 模块 2.0 可用时）*
* 使用[Redis Enterprise ](https://redislabs.com/redis-enterprise-software/)*（当 RediSearch 模块 2.0 可用时）*
* 使用[Docker](https://hub.docker.com/r/redis/redis-stack)

# Docker部署

```shell
   docker run -it --rm --name redis-stack \
   -p 6379:6379 \
   redis/redis-stack:latest
```

# 创建索引

## 示例

使用一个描述电影的简单数据集，目前所有数据都是英文的，关于中文分词倒排索引待补充

数据集属性如下：

* **`movie_id`** : 数据id
* **`title`** : 电影标题
* **`plot`** : 概要
* **`genre`** : 电影 类型
* **`release_year`** : 发行年份
* **`rating`** : 评分
* **`votes`** : 票房
* **`poster`** : 电影海报链接
* **`imdb_id`** : [IMDB](https://imdb.com/)数据库中电影的 id 。

## 常规使用

我们一般会设计一个合适的key如：movie:1001来表示电影1001的缓存key。

即`business_object:key`模式

以往利用Redis 哈希允许应用程序在各个字段中构建所有电影属性；

RediSearch还将根据索引定义对字段进行索引。

### 插入测试数据：

```shell
HSET movie:11002 title "Star Wars: Episode V - The Empire Strikes Back" plot "After the Rebels are brutally overpowered by the Empire on the ice planet Hoth, Luke Skywalker begins Jedi training with Yoda, while his friends are pursued by Darth Vader and a bounty hunter named Boba Fett all over the galaxy." release_year 1980 genre "Action" rating 8.7 votes 1127635 imdb_id tt0080684

HSET movie:11003 title "The Godfather" plot "The aging patriarch of an organized crime dynasty transfers control of his clandestine empire to his reluctant son." release_year 1972 genre "Drama" rating 9.2 votes 1563839 imdb_id tt0068646

HSET movie:11004 title "Heat" plot "A group of professional bank robbers start to feel the heat from police when they unknowingly leave a clue at their latest heist." release_year 1995 genre "Thriller" rating 8.2 votes 559490 imdb_id tt0113277

HSET "movie:11005" title "Star Wars: Episode VI - Return of the Jedi" genre "Action" votes 906260 rating 8.3 release_year 1983  plot "The Rebels dispatch to Endor to destroy the second Empire's Death Star." ibmdb_id "tt0086190" 

```

#### 查询

根据id获取内容

```shell
HMGET movie:11002 title rating

1) "Star Wars: Episode V - The Empire Strikes Back"
2) "8.7"
```

增加这部电影的评级

```shell
 HINCRBYFLOAT movie:11002 rating 0.1

"8.8"
```

但是，如何按发行年份、评级或片名获取一部电影或电影列表呢？(条件查询)

一种选择是读取所有电影，检查所有字段，然后仅返回匹配的电影；不用说，这是一个非常糟糕的主意。

因此，Redis 开发人员经常使用指向电影哈希的 SET/SORTED SET 结构创建自定义二级索引。这需要一些繁重的设计和实现。

这就是 RediSearch 模块可以提供帮助的地方，也是创建它的原因。

# RediSearch索引原理

RediSearch 通过提供一种简单且自动的方式在 Redis 哈希上创建二级索引，极大地简化了这一过程。（最终会出现更多数据结构）

![二级索引](https://raw.githubusercontent.com/Rain-ziyu/img/master/20231222163215.png)

如果要使用 RediSearch 查询某个字段，则必须首先对该字段建立索引。

让我们首先为我们的电影索引以下字段：

* 标题
* 发布年份
* 评分
* 类型

创建索引时您定义：

* 您要索引哪些数据：以*`movies`开头的所有*哈希值*
* 您想要对哈希中的哪些字段进行索引。

> ***警告：不要索引所有字段***
>
> 索引占用内存空间，并且必须在主数据更新时更新。因此，请仔细创建索引并保持定义符合您的需求。

### 创建索引

使用以下命令创建索引：

```shell
 FT.CREATE idx:movie ON hash PREFIX 1 "movie:" SCHEMA title TEXT SORTABLE release_year NUMERIC SORTABLE rating NUMERIC SORTABLE genre TAG SORTABLE
```

**命令解释：**

* [`FT.CREATE`](https://oss.redislabs.com/redisearch/master/Commands/#ftcreate)：创建索引。索引名称将在所有键名称中使用，因此请保持简短。
* `idx:movie`：索引的名称
* `ON hash`：要索引的结构类型。*请注意，在 RediSearch 2.0 中仅支持哈希结构，此参数将来将接受其他 Redis 数据类型，因为 RediSearch 会更新以对它们进行索引*
* `PREFIX 1 "movie:"`：应索引的键的前缀。这是一个列表，因此由于我们只想索引 movie: ，因此数字为 1。假设您想索引具有相同字段的 movie 和 tv_show，您可以使用：`PREFIX 2 "movie:" "tv_show:"`
* `SCHEMA ...`：定义模式、字段及其类型以进行索引，正如您在命令中看到的那样，我们使用[TEXT](https://oss.redislabs.com/redisearch/Query_Syntax/#a_few_query_examples)、[NUMERIC](https://oss.redislabs.com/redisearch/Query_Syntax/#numeric_filters_in_query)和[TAG](https://oss.redislabs.com/redisearch/Query_Syntax/#tag_filters)以及[SORTABLE](https://oss.redislabs.com/redisearch/Sorting/)参数。

可以通过以下命令查看索引信息：

```
 FT.INFO idx:movie
```

# 使用索引查询数据

#### 查询所有包含字符串war的数据

```shell
FT.SEARCH idx:movie "war"

1) (integer) 2
2) "movie:11002"
3)  1) "release_year"
    2) "1980"
    3) "genre"
    4) "Action"
    5) "imdb_id"
    6) "tt0080684"
    7) "plot"
    8) "After the Rebels are brutally overpowered by the Empire on the ice planet Hoth, Luke Skywalker begins Jedi training with Yoda, while his friends are pursued by Darth Vader and a bounty hunter named Boba Fett all over the galaxy."
    9) "rating"
   10) "8.7"
   11) "title"
   12) "Star Wars: Episode V - The Empire Strikes Back"
   13) "votes"
   14) "1127635"
4) "movie:11005"
5)  1) "release_year"
    2) "1983"
    3) "genre"
    4) "Action"
    5) "plot"
    6) "The Rebels dispatch to Endor to destroy the second Empire's Death Star."
    7) "rating"
    8) "8.3"
    9) "title"
   10) "Star Wars: Episode VI - Return of the Jedi"
   11) "votes"
   12) "906260"
   13) "ibmdb_id"
   14) "tt0086190"
```

查询会首先返回结果数，之后就是对应的数据的key，在之后就是完整的hash内容

#### 使用参数限制查询返回的字段列表`RETURN`

让我们运行相同的查询，并仅返回标题和release_year：

```shell
FT.SEARCH idx:movie "war" RETURN 2 title release_year

1) (integer) 2
2) "movie:11002"
3) 1) "title"
   2) "Star Wars: Episode V - The Empire Strikes Back"
   3) "release_year"
   4) "1980"
4) "movie:11005"
5) 1) "title"
   2) "Star Wars: Episode VI - Return of the Jedi"
   3) "release_year"
   4) "1983"
```

以上查询都没有指定查询字段，但是仍然查出数据，这是因为 RediSearch 默认情况下会搜索所有 TEXT 字段。在当前索引中，只有标题作为 TEXT 字段出现。

### 针对固定字段进行条件查询

```shell
FT.SEARCH idx:movie "@title:war" RETURN 2 title release_year

1) (integer) 2
2) "movie:11002"
3) 1) "title"
   2) "Star Wars: Episode V - The Empire Strikes Back"
   3) "release_year"
   4) "1980"
4) "movie:11005"
5) 1) "title"
   2) "Star Wars: Episode VI - Return of the Jedi"
   3) "release_year"
   4) "1983"
```

### 查询字符串中包含war但不包含`jedi`

```shell
FT.SEARCH idx:movie "war -jedi" RETURN 2 title release_year

1) (integer) 1
2) "movie:11002"
3) 1) "title"
   2) "Star Wars: Episode V - The Empire Strikes Back"
   3) "release_year"
   4) "1980"
```

### 模糊（近似）搜索

gdfather这个词包含拼写错误，即完整是Godfather，但是可以使用[模糊匹配](https://oss.redislabs.com/redisearch/Query_Syntax/#fuzzy_matching)来匹配它。模糊匹配是基于[编辑距离](https://en.wikipedia.org/wiki/Levenshtein_distance)（LD）进行的。

```shell
FT.SEARCH idx:movie " %gdfather% " RETURN 2 title release_year

1) (integer) 1
2) "movie:11003"
3) 1) "title"
   2) "The Godfather"
   3) "release_year"
   4) "1972"
```

### 精确匹配

```shell
FT.SEARCH idx:movie "@genre:{Thriller}" RETURN 3 title release_year genre
1) (integer) 1
2) "movie:11004"
3) 1) "title"
   2) "Heat"
   3) "release_year"
   4) "1995"
   5) "genre"
   6) "Thriller"
```

### 或语法

```shell
FT.SEARCH idx:movie "@genre:{Thriller|Action}" RETURN 2 title release_year

1) (integer) 3
2) "movie:11004"
3) 1) "title"
   2) "Heat"
   3) "release_year"
   4) "1995"
4) "movie:11002"
5) 1) "title"
   2) "Star Wars: Episode V - The Empire Strikes Back"
   3) "release_year"
   4) "1980"
6) "movie:11005"
7) 1) "title"
   2) "Star Wars: Episode VI - Return of the Jedi"
   3) "release_year"
   4) "1983"
```

### 组合多个搜索条件

```shell
FT.SEARCH idx:movie "@genre:{Thriller|Action} @title:-jedi" RETURN 2 title release_year

1) (integer) 2
2) "movie:11004"
3) 1) "title"
   2) "Heat"
   3) "release_year"
   4) "1995"
4) "movie:11002"
5) 1) "title"
   2) "Star Wars: Episode V - The Empire Strikes Back"
   3) "release_year"
   4) "1980"
```

### 范围查询

* 使用`FILTER`参数

```shell
FT.SEARCH idx:movie * FILTER release_year 1970 1980 RETURN 2 title release_year
```

或者

* `@field`在查询字符串中使用。

```shell
FT.SEARCH idx:movie "@release_year:[1970 1980]" RETURN 2 title release_year
```

要排除某个值，请在 FILTER 或查询字符串中添加该值`(`

```shell
 FT.SEARCH idx:movie "@release_year:[1970 (1980]" RETURN 2 title release_year
```

### 分页语法

```shell
FT.SEARCH idx:movie "*" LIMIT 0 1

1) (integer) 4
2) "movie:11004"
3)  1) "plot"
    2) "A group of professional bank robbers start to feel the heat from police when they unknowingly leave a clue at their latest heist."
    3) "title"
    4) "Heat"
    5) "rating"
    6) "8.2"
    7) "votes"
    8) "559490"
    9) "imdb_id"
   10) "tt0113277"
   11) "release_year"
   12) "1995"
   13) "genre"
   14) "Thriller"
```

## 插入、更新、删除和过期文档

在创建索引时使用`idx:movie ON hash PREFIX 1 "movie:"`，会为所有查看所有现有键并为其建立索引的参数，因此新插入的数据也将被索引。

```shell
HSET movie:11033 title "Tomorrow Never Dies" plot "James Bond sets out to stop a media mogul's plan to induce war between China and the U.K in order to obtain exclusive global media coverage." release_year 1997 genre "Action" rating 6.5 votes 177732 imdb_id tt0120347
```

```shell
FT.SEARCH idx:movie "*" LIMIT 0 0

1) (integer) 5
```

当产生更新时索引也会更新

```shell
HSET movie:11033 title "Tomorrow Never Dies - 007"  
```

```shell
FT.SEARCH idx:movie "007" RETURN 2 title release_year

1) (integer) 1
2) "movie:11033"
3) 1) "title"
   2) "Tomorrow Never Dies - 007"
   3) "release_year"
   4) "1997"
```

您*删除*哈希时，索引也会更新，并且当密钥过期（TTL-生存时间）时也会发生同样的情况。

```shell
EXPIRE "movie:11033" 20
```

20s后索引将清楚该数据

```shell
FT.SEARCH idx:movie "007" RETURN 2 title release_year
1) (integer) 0
```

# 管理索引

## 查询所有索引

```shell
FT._LIST

1) idx:movie
```

## 查询索引信息

```shell
FT.INFO "idx:movie"
 1) index_name
 2) idx:movie
 3) index_options
 4) (empty array)
 5) index_definition
 6) 1) key_type
    2) HASH
    3) prefixes
    4) 1) movie:
    5) default_score
    6) "1"
```

## 更新索引

```shell
FT.ALTER idx:movie SCHEMA ADD plot TEXT WEIGHT 0.5
```

`WEIGHT`表示在计算结果准确性时的参考重要性，这是一个乘数（默认为1）。所以在这个例子中，情节（TEXT）不如（plot）重要。


## 删除索引

```shell
FT.DROPINDEX idx:movie
```

删除索引并不会影响数据

# 查询示例

## 全文索引

查询时不指定field

```shell
FT.SEARCH "idx:movie" "heat" RETURN 2 title plot
```

## 查询字段

```shell
FT.SEARCH "idx:movie" "@title:heat" RETURN 2 title plot
```

使用@field指定筛选字段

## 排除查询

排除某些条件

`-`对应条件

```shell
FT.SEARCH "idx:movie" "@title:(heat -california)" RETURN 2 title plot
```

## 组合查询

组合多个筛选条件

使用多个`@field:{value}`

```shell
FT.SEARCH "idx:movie" "@title:(heat) @genre:{Drama} " RETURN 3 title plot genre
```

以上实例中@genre是一个特殊类型，在我们创建索引时指定的是TAG类型（因为索引引擎不进行任何词干提取），因此需要使用{}进行表示这是一个TAG条件

**TAG 是当您想要对字符串/单词进行精确匹配时使用的结构**

## 或条件

这与前面的查询类似，您可以在值列表中使用`|`来表示 OR。

```shell
FT.SEARCH "idx:movie" "@title:(heat)  @genre:{Drama|Comedy} " RETURN 3 title plot genre
```

当然多个条件之间也可以使用该语法

```shell
FT.SEARCH "idx:movie" "@title:(heat) | @genre:{Drama|Comedy} " RETURN 3 title plot genre
```

## 数字查询

对于数字字段，您必须输入的条件必须是一个区间，即使你查询的是一个值，需要如下查询

```shell
FT.SEARCH "idx:movie" "@genre:{Mystery|Thriller} (@release_year:[2018 2018] | @release_year:[2014 2014] )"   RETURN 3 title release_year genre

1) (integer) 3
2) "movie:461"
3) 1) "title"
   2) "The Boat ()"
   3) "release_year"
   4) "2018"
   5) "genre"
   6) "Mystery"
4) "movie:65"
5) 1) "title"
   2) "The Loft"
   3) "release_year"
   4) "2014"
   5) "genre"
   6) "Mystery"
6) "movie:989"
7) 1) "title"
   2) "Los Angeles Overnight"
   3) "release_year"
   4) "2018"
   5) "genre"
   6) "Thriller"

```

* 无字段查询适用于所有 TEXT 字段并使用单词及其基本形式（词干提取）
* 要将条件应用于特定字段，您必须使用`@field:`符号
* 多个条件是“交集”（AND条件），要做“并集”（OR条件），就必须使用“ `|`”字符。

## 排序

SORTBY 指定字段 DESC表示降序

```shell
FT.SEARCH "idx:movie" "@genre:{Action}"  SORTBY release_year DESC RETURN 2 title release_year
```

如果要对多个字段进行排序，例如按`genre`升序和`release_year`降序对电影进行排序，则必须使用 FT.AGGREGATE（聚合查询）

[注意： SORTBY](https://oss.redislabs.com/redisearch/Sorting/#specifying_sortby)中使用的字段应该是索引架构的一部分，并定义为 SORTABLE。

## 分页

limit

```shell
FT.SEARCH "idx:movie" "@genre:{Action}" LIMIT 0 100  SORTBY release_year ASC RETURN 2 title release_year
```

## 计数

没有专门的count语法，使用LIMIT 0 0

## 地理空间查询（适用于Geo类型字段）

假设您在位于“11 W 53rd St, New York”的 MOMA，并且您想要查找位于 400m 半径内的所有剧院。

为此，您需要确定当前位置的纬度/经度位置`-73.9798156,40.7614367`，并执行以下查询：

```shell
> FT.SEARCH "idx:theater" "@location:[-73.9798156 40.7614367 400 m]" RETURN 2 name address

1) (integer) 5
 2) "theater:30"
 3) 1) "name"
    2) "Ed Sullivan Theater"
    3) "address"
    4) "1697 Broadway"
...
10) "theater:115"
11) 1) "name"
    2) "Winter Garden Theatre"
    3) "address"
    4) "1634 Broadway"
```

# 聚合查询

语句中的数字1、2应该表示的是后边有几个字符串作为参数

如GROUPBY 1 @release_year就是后边一个字符串作为查询参数

## 分组求总数

***按年份划分的电影数量***

GROUPBY 1 @release_year （指定分组字段）REDUCE COUNT 0      AS nb_of_movies

以上表示每一条计数加一，默认为0 最终作为nb_of_movies字段

```shell
 FT.AGGREGATE "idx:movie" "*" GROUPBY 1 @release_year REDUCE COUNT 0 AS nb_of_movies
```

## 分组求平均值


```shell
FT.AGGREGATE idx:movie "*" GROUPBY 1 @genre REDUCE COUNT 0 AS nb_of_movies REDUCE SUM 1 votes AS nb_of_votes REDUCE AVG 1 rating AS avg_rating SORTBY 4 @avg_rating DESC @nb_of_votes DESC


 1) (integer) 26
 2) 1) "genre"
    2) "fantasy"
    3) "nb_of_movies"
    4) "1"
    5) "nb_of_votes"
    6) "1500090"
    7) "avg_rating"
    8) "8.8"
...
11) 1) "genre"
    2) "romance"
    3) "nb_of_movies"
    4) "2"
    5) "nb_of_votes"
    6) "746"
    7) "avg_rating"
    8) "6.65"

```

## 转换函数

时间戳转换

其中APPLY year(@last_login) AS year就是将时间戳转换为年份

```shell
FT.AGGREGATE idx:user * APPLY year(@last_login) AS year APPLY "monthofyear(@last_login) + 1" AS month GROUPBY 2 @year @month REDUCE count 0 AS num_login SORTBY 4 @year ASC @month ASC

 1) (integer) 13
 2) 1) "year"
    2) "2019"
    3) "month"
    4) "9"
    5) "num_login"
    6) "230"
...
14) 1) "year"
    2) "2020"
    3) "month"
    4) "9"
    5) "num_login"
    6) "271"


```

使用日期/时间应用函数可以从时间戳中提取星期几

## FILTER删选

还可以使用与每个结果中的值相关的谓词表达式来过滤结果。这是在查询后应用的，并且与管道的当前状态相关。这是使用[FILTER](https://oss.redislabs.com/redisearch/Aggregations/#filter_expressions)参数完成的。

```shell
FT.AGGREGATE idx:user "@gender:{female}" GROUPBY 1 @country  REDUCE COUNT 0 AS nb_of_users  FILTER "@country!='china' && @nb_of_users > 100" SORTBY 2 @nb_of_users DESC

1) (integer) 163
2) 1) "country"
   2) "indonesia"
   3) "nb_of_users"
   4) "309"
...
6) 1) "country"
   2) "brazil"
   3) "nb_of_users"
   4) "108"

```

# 高级功能

## 使用过滤器创建索引

不仅可以直接针对key建立索引，还可以使用过滤器创建索引。


例如创建包含 1990 年至 2000 年（不包括 2000 年）之间发行的所有“戏剧”电影的索引。

`表达式[`FILTER`](https://oss.redislabs.com/redisearch/Aggregations/#filter_expressions)使用 [聚合过滤器语法( [https://oss.redislabs.com/redisearch/Aggregations/#filter_expressions](https://oss.redislabs.com/redisearch/Aggregations/#filter_expressions) )]，例如对于流派和发行年份，它将是

* `FILTER "@genre=='Drama' && @release_year>=1990 && @release_year<2000"`
