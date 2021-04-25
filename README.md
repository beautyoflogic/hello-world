
### Data Engineering Nanodegree Program

## PROJECT 1: DATA MODELING with POSTGRES

#### DATABASE PURPOSE

The Sparkify starup created a new music streaming application. In an effort 
to make this application more effective, Spyrkify's analytics team collected 
information about user activity and music listened to. All information is 
stored as JSON log files and JSON metadata files. The format of the data and 
its large volume makes direct analysis difficult. In this regard, it was 
decided to involve a data engineer to create a PostgreSQL specific schema 
designed database with the purpose to optimize queries.

---

#### SCHEMA DESIGN
 
As a data engineer I am agree with the _star schema design_ that includes
one Fact Table and four Dimension Tables.

1. The Fact Table is songplays table.
2. Dimension Tables are users, songs, artists, time tables.

![\"sparkify\" database ERD](images/project1a_erd.png)
 

Using the song and log datasets a star schema optimized for queries on song 
play analysis.

---

#### DATA SET

The complete Data Set to be processed is the 'data' folder that contains two subfolders:
log_data and song_data. Both of them is a JSON files directories.

- Song data
The song data set contains metadata about a song and the artist of that song. 
The files are partitioned by the first three letters of each song's track ID.
For example, here is the song data structure:

![example of the song_data directory structure](images/project1a_example_of_the_song_data_directory_structure.png)

Here is the example of the one song file content (data/song_data/A/A/A/TRAAAAW128F429D538.json):

```json
{"num_songs": 1, "artist_id": "ARD7TVE1187B99BFB1", "artist_latitude": null, "artist_longitude": null, "artist_location": "California - LA", "artist_name": "Casual", "song_id": "SOMZWCG12A8C13C480", "title": "I Didn't Mean To", "duration": 218.93179, "year": 0}
```

- Log data
The log files dataset is partitioned by year and month.
For example, here is the structure of the log data set:

![example of the log_data structure](images/project1a_example_of_the_log_data_directory_structure.png)

Here is the example of the one log file content (data/log_data/2018/11/2018-11-01-events.json):

```json
{"artist":null,"auth":"Logged In","firstName":"Walter","gender":"M","itemInSession":0,"lastName":"Frye","length":null,"level":"free","location":"San Francisco-Oakland-Hayward, CA","method":"GET","page":"Home","registration":1540919166796.0,"sessionId":38,"song":null,"status":200,"ts":1541105830796,"userAgent":"\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"","userId":"39"}
{"artist":null,"auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":0,"lastName":"Summers","length":null,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"GET","page":"Home","registration":1540344794796.0,"sessionId":139,"song":null,"status":200,"ts":1541106106796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":"Des'ree","auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":1,"lastName":"Summers","length":246.30812,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"PUT","page":"NextSong","registration":1540344794796.0,"sessionId":139,"song":"You Gotta Be","status":200,"ts":1541106106796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":null,"auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":2,"lastName":"Summers","length":null,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"GET","page":"Upgrade","registration":1540344794796.0,"sessionId":139,"song":null,"status":200,"ts":1541106132796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":"Mr Oizo","auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":3,"lastName":"Summers","length":144.03873,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"PUT","page":"NextSong","registration":1540344794796.0,"sessionId":139,"song":"Flat 55","status":200,"ts":1541106352796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":"Tamba Trio","auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":4,"lastName":"Summers","length":177.18812,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"PUT","page":"NextSong","registration":1540344794796.0,"sessionId":139,"song":"Quem Quiser Encontrar O Amor","status":200,"ts":1541106496796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":"The Mars Volta","auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":5,"lastName":"Summers","length":380.42077,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"PUT","page":"NextSong","registration":1540344794796.0,"sessionId":139,"song":"Eriatarka","status":200,"ts":1541106673796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":"Infected Mushroom","auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":6,"lastName":"Summers","length":440.2673,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"PUT","page":"NextSong","registration":1540344794796.0,"sessionId":139,"song":"Becoming Insane","status":200,"ts":1541107053796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":"Blue October \/ Imogen Heap","auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":7,"lastName":"Summers","length":241.3971,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"PUT","page":"NextSong","registration":1540344794796.0,"sessionId":139,"song":"Congratulations","status":200,"ts":1541107493796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":"Girl Talk","auth":"Logged In","firstName":"Kaylee","gender":"F","itemInSession":8,"lastName":"Summers","length":160.15628,"level":"free","location":"Phoenix-Mesa-Scottsdale, AZ","method":"PUT","page":"NextSong","registration":1540344794796.0,"sessionId":139,"song":"Once again","status":200,"ts":1541107734796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/35.0.1916.153 Safari\/537.36\"","userId":"8"}
{"artist":"Black Eyed Peas","auth":"Logged In","firstName":"Sylvie","gender":"F","itemInSession":0,"lastName":"Cruz","length":214.93506,"level":"free","location":"Washington-Arlington-Alexandria, DC-VA-MD-WV","method":"PUT","page":"NextSong","registration":1540266185796.0,"sessionId":9,"song":"Pump It","status":200,"ts":1541108520796,"userAgent":"\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.77.4 (KHTML, like Gecko) Version\/7.0.5 Safari\/537.77.4\"","userId":"10"}
{"artist":null,"auth":"Logged In","firstName":"Ryan","gender":"M","itemInSession":0,"lastName":"Smith","length":null,"level":"free","location":"San Jose-Sunnyvale-Santa Clara, CA","method":"GET","page":"Home","registration":1541016707796.0,"sessionId":169,"song":null,"status":200,"ts":1541109015796,"userAgent":"\"Mozilla\/5.0 (X11; Linux x86_64) AppleWebKit\/537.36 (KHTML, like Gecko) Ubuntu Chromium\/36.0.1985.125 Chrome\/36.0.1985.125 Safari\/537.36\"","userId":"26"}
{"artist":"Fall Out Boy","auth":"Logged In","firstName":"Ryan","gender":"M","itemInSession":1,"lastName":"Smith","length":200.72444,"level":"free","location":"San Jose-Sunnyvale-Santa Clara, CA","method":"PUT","page":"NextSong","registration":1541016707796.0,"sessionId":169,"song":"Nobody Puts Baby In The Corner","status":200,"ts":1541109125796,"userAgent":"\"Mozilla\/5.0 (X11; Linux x86_64) AppleWebKit\/537.36 (KHTML, like Gecko) Ubuntu Chromium\/36.0.1985.125 Chrome\/36.0.1985.125 Safari\/537.36\"","userId":"26"}
{"artist":"M.I.A.","auth":"Logged In","firstName":"Ryan","gender":"M","itemInSession":2,"lastName":"Smith","length":233.7171,"level":"free","location":"San Jose-Sunnyvale-Santa Clara, CA","method":"PUT","page":"NextSong","registration":1541016707796.0,"sessionId":169,"song":"Mango Pickle Down River (With The Wilcannia Mob)","status":200,"ts":1541109325796,"userAgent":"\"Mozilla\/5.0 (X11; Linux x86_64) AppleWebKit\/537.36 (KHTML, like Gecko) Ubuntu Chromium\/36.0.1985.125 Chrome\/36.0.1985.125 Safari\/537.36\"","userId":"26"}
{"artist":"Survivor","auth":"Logged In","firstName":"Jayden","gender":"M","itemInSession":0,"lastName":"Fox","length":245.36771,"level":"free","location":"New Orleans-Metairie, LA","method":"PUT","page":"NextSong","registration":1541033612796.0,"sessionId":100,"song":"Eye Of The Tiger","status":200,"ts":1541110994796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.3; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"","userId":"101"}
```

---

#### DATABASE and TABLES CREATION
    
The create_tables.py and DROP and CREATE queries of the 
sql_queries.py script are completed to create "sparkifydb" database with songplays,
users, songs, artists, time tables. The running of create_tables.py have to be done.

The running test.ipynb is recommended after running create_tables.py 
to confirm the creation of the tables with the correct columns.


---

#### ETL PROCESS

To develop ETL processes for each table was recommended the following to 
instruction in the etl.ipynb notebook and running test.ipynb at the end of each section
to confirm that tables were successfully filled. Also was recommended 
"to rerun create_tables.py to reset your tables before each time you run this notebook".

The pincipal part of the ETL development is the etl.py script.
The etl.ipynb and test.ipynb scripts are auxiliary. They are designed only for using by 
for the data engineer during project development.

The etl.py facilitates the processing of the entire datasets.
It reads files from song_data and log_data JSON files and loads them into 
corresponding tables. 
To complete etl.py development the INSERT queries and SELECT finding songs query
of the sql_queries.py are filled.

---

#### DATABASE QUERIES examples

Here are some examples of the queries that can be used by startup analytical team to
analyze the complete dataset. They are simple and clear. They can be vey util because
created database and ETL pipline have been developed with the goal to optimize 
analytics.

For example,
 - SELECT count(song_id) FROM songs
 
 ![example of the log_data structure](images/project1a_query1.png)
 
 - SELECT count(user_id) FROM users
 
 ![example of the log_data structure](images/project1a_query2.png)
 
 - SELECT count(level) FROM users
 
 ![example of the log_data structure](images/project1a_query3.png)
 
 

---
