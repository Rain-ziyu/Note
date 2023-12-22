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
