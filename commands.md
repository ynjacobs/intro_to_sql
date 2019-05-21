intro_to_sql=# select * from robots where source = 'Star Wars';
 name |  source   | personality | id 
------+-----------+-------------+----
 C3PO | Star Wars | anxious     |  4
 R2D2 | Star Wars | loyal       |  8
(2 rows)

intro_to_sql=# select * from robots where source = "Star Wars";
ERROR:  column "Star Wars" does not exist
LINE 1: select * from robots where source = "Star Wars";
                                            ^
intro_to_sql=# select * from robots where source like 'T%'
intro_to_sql-# ;
     name     |                source                | personality | id 
--------------+--------------------------------------+-------------+----
 Marvin       | The Hitchhiker's Guide to the Galaxy | pessimistic |  3
 Deep Thought | The Hitchhiker's Guide to the Galaxy | thorough    |  7
(2 rows)

intro_to_sql=# select * from robots where personality = 'anxious';
 name |  source   | personality | id 
------+-----------+-------------+----
 C3PO | Star Wars | anxious     |  4
(1 row)

intro_to_sql=# select * from recipes
intro_to_sql-# ;
intro_to_sql=# select * from recipes where nut free= True
intro_to_sql-# ;
ERROR:  syntax error at or near "free"
LINE 1: select * from recipes where nut free= True
                                        ^
intro_to_sql=# \d+ recipes
intro_to_sql=# select * from recipes where column='nut_free';
ERROR:  syntax error at or near "column"
LINE 1: select * from recipes where column='nut_free';
                                    ^
intro_to_sql=# select * from recipes where nut_free = True;
intro_to_sql=# 
intro_to_sql=# select recipes from recipes where nut_free = True;
intro_to_sql=# select nut_free from recipes where nut_free = True;
 nut_free 
----------
 t
 t
 t
 t
 t
 t
 t
(7 rows)

intro_to_sql=# select recipes from recipes where nut_free = True;
intro_to_sql=# select * from recipes where nut_free = True;
intro_to_sql=# select Count(*) from recipes 
intro_to_sql-# ;
 count 
-------
     9
(1 row)

intro_to_sql=# select Count(*) from recipes where nut_free = True;
 count 
-------
     7
(1 row)

intro_to_sql=# select name,nut_free from recipes where nut_free = True; 
                   name                    | nut_free 
-------------------------------------------+----------
 Butternut Squash Bake                     | t
 Vegetarian Bibimbap                       | t
 French Veggie Loaf                        | t
 Quinoa and Black Beans                    | t
 Juicy Roasted Chicken                     | t
 Garlic Green Beans                        | t
 Stout Slow Cooker Corned Beef and Veggies | t
(7 rows)
intro_to_sql=# 
intro_to_sql=# 
intro_to_sql=# \q
yjacobs@yaels-computer:~/bitmaker/projects/intro_to_sql$ psql intro_to_sql
psql (10.8 (Ubuntu 10.8-0ubuntu0.18.04.1))
Type "help" for help.

intro_to_sql=# select Count(*) from recipes
intro_to_sql-# where gluten_free = True
intro_to_sql-# and vegetarian = False;
 count 
-------
     2
(1 row)

intro_to_sql=# select name from recipes
where gluten_free = True
and vegetarian = False;
                   name                    
-------------------------------------------
 Juicy Roasted Chicken
 Stout Slow Cooker Corned Beef and Veggies
(2 rows)

intro_to_sql=# \d+ animal;
Did not find any relation named "animal".
intro_to_sql=# \d+ animals;
                                                            Table "public.animals"
     Column     |          Type          | Collation | Nullable |               Default               | Storage  | Stats target | Description 
----------------+------------------------+-----------+----------+-------------------------------------+----------+--------------+-------------
 name           | character varying(255) |           |          |                                     | extended |              | 
 number_of_legs | integer                |           |          |                                     | plain    |              | 
 flying         | boolean                |           |          |                                     | plain    |              | 
 swimming       | boolean                |           |          |                                     | plain    |              | 
 egg_laying     | boolean                |           |          |                                     | plain    |              | 
 id             | integer                |           | not null | nextval('animals_id_seq'::regclass) | plain    |              | 
Indexes:
    "animals_pkey" PRIMARY KEY, btree (id)

intro_to_sql=# select max(number_of_legs) from animals;
 max 
-----
   8
(1 row)

intro_to_sql=# select name, max(number_of_legs) from animals;
ERROR:  column "animals.name" must appear in the GROUP BY clause or be used in an aggregate function
LINE 1: select name, max(number_of_legs) from animals;
               ^
intro_to_sql=# select name, max(number_of_legs) from animals group by name
intro_to_sql-# ;
       name       | max 
------------------+-----
 cow              |   4
 whale            |   0
 octopus          |   8
 hammerhead shark |   0
 chicken          |   2
 duck             |   2
 owl              |   2
 sheep            |   4
 cat              |   4
 bat              |   2
(10 rows)

intro_to_sql=# select min(time_to_play) from board_game;
ERROR:  relation "board_game" does not exist
LINE 1: select min(time_to_play) from board_game;
                                      ^
intro_to_sql=# select min(time_to_play) from board_games;
ERROR:  column "time_to_play" does not exist
LINE 1: select min(time_to_play) from board_games;
                   ^
HINT:  Perhaps you meant to reference the column "board_games.mins_to_play".
intro_to_sql=# \d+ board_games
intro_to_sql=# select min(mins_to_play) from board_games;
 min 
-----
  15
(1 row)

intro_to_sql=# \d+ recipes
intro_to_sql=# select max(minutes_required) from recipes;
 max 
-----
 390
(1 row)

intro_to_sql=# select * from robots where source like 'M%'
;
 name | source | personality | id 
------+--------+-------------+----
(0 rows)

intro_to_sql=# select * from robots where source like 'm%'
;
 name | source | personality | id 
------+--------+-------------+----
(0 rows)

intro_to_sql=# select * from robots where name like 'M%'
;
      name      |                source                |  personality  | id 
----------------+--------------------------------------+---------------+----
 Marvin         | The Hitchhiker's Guide to the Galaxy | pessimistic   |  3
 Mr. Butlertron | Clone High                           | compassionate |  5
(2 rows)

intro_to_sql=# select * from robots where name = 'M%'
;
 name | source | personality | id 
------+--------+-------------+----
(0 rows)

intro_to_sql=# select * from board_games where min_player>= 8 and max_player <=8;
ERROR:  column "min_player" does not exist
LINE 1: select * from board_games where min_player>= 8 and max_playe...
                                        ^
HINT:  Perhaps you meant to reference the column "board_games.min_players".
intro_to_sql=# select * from board_games where min_players>= 8 and max_players <=8;
 name | max_players | min_players | category | mins_to_play | id 
------+-------------+-------------+----------+--------------+----
(0 rows)

intro_to_sql=# select * from board_games 
intro_to_sql-# ;
          name          | max_players | min_players |    category    | mins_to_play | id 
------------------------+-------------+-------------+----------------+--------------+----
 Arkham Horror          |           8 |           1 | strategy       |          240 |  1
 Dixit                  |           6 |           3 | party          |           30 |  2
 Carcassonne            |           5 |           2 | light strategy |           45 |  3
 Quarto                 |           2 |           2 | abstract       |           20 |  4
 7 Wonders              |           7 |           2 | strategy       |           30 |  5
 Agricola               |           5 |           1 | strategy       |          120 |  6
 Bohnanza               |           7 |           2 | light strategy |           45 |  7
 Cards Against Humanity |          30 |           5 | party          |           30 |  8
 Game of Things         |          15 |           2 | party          |           45 |  9
 Sushi Go               |           5 |           2 | party          |           15 | 10
 Quixo                  |           4 |           2 | abstract       |           15 | 11
 7 Wonders              |           7 |           2 | strategy       |           30 | 12
(12 rows)

intro_to_sql=# select * from board_games where min_players<= 8 and max_players >=8;
          name          | max_players | min_players | category | mins_to_play | id 
------------------------+-------------+-------------+----------+--------------+----
 Arkham Horror          |           8 |           1 | strategy |          240 |  1
 Cards Against Humanity |          30 |           5 | party    |           30 |  8
 Game of Things         |          15 |           2 | party    |           45 |  9
(3 rows)

intro_to_sql=# select * from board_games where 8 between min_players and max_players
intro_to_sql-# ;
          name          | max_players | min_players | category | mins_to_play | id 
------------------------+-------------+-------------+----------+--------------+----
 Arkham Horror          |           8 |           1 | strategy |          240 |  1
 Cards Against Humanity |          30 |           5 | party    |           30 |  8
 Game of Things         |          15 |           2 | party    |           45 |  9
(3 rows)

intro_to_sql=# select * from animals where swimming = True and egg_laying = True and flying = Flase;
ERROR:  column "flase" does not exist
LINE 1: ...re swimming = True and egg_laying = True and flying = Flase;
                                                                 ^
intro_to_sql=# select * from animals where swimming = True and egg_laying = True and flying = False;
  name   | number_of_legs | flying | swimming | egg_laying | id 
---------+----------------+--------+----------+------------+----
 octopus |              8 | f      | t        | t          |  3
(1 row)

intro_to_sql=# select * max(max_player) from board_games;
;
ERROR:  syntax error at or near "max"
LINE 1: select * max(max_player) from board_games;
                 ^
intro_to_sql=# select max(max_players) from board_games;
 max 
-----
  30
(1 row)