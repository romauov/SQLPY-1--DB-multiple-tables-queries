## Домашнее задание по теме «Группировки, выборки из нескольких таблиц»
#### студент: Сергей Романов

#### группа: SQLPY-1

[предыдущая работа с созданием таблиц БД](https://github.com/romauov/SQLPY-1--DB-design-and-queries)

[предыдущая работа с заполнением таблиц БД](https://github.com/romauov/SQLPY-1--BD-filling-in)


SQL-запросы:

1. количество исполнителей в каждом жанре;
```
SELECT genrename, COUNT(name) FROM genre g
JOIN genreartist ga ON g.genreid = ga.genreid
JOIN artist a ON ga.artistid = a.artistid
GROUP BY genrename;
```
2. количество треков, вошедших в альбомы 2019-2020 годов;
```
SELECT albumname, COUNT(trackname) FROM album a
JOIN track t ON a.albumid = t.albumid
WHERE year BETWEEN 2019 AND 2020
GROUP BY albumname;
```
3. средняя продолжительность треков по каждому альбому;
```
SELECT albumname, AVG(tracklength) FROM album a
JOIN track t ON a.albumid = t.albumid
GROUP BY albumname;
```
4. все исполнители, которые не выпустили альбомы в 2020 году;
```
SELECT name, year FROM artist ar
JOIN artistalbum aa ON ar.artistid = aa.artistid
JOIN album al ON aa.albumid = al.albumid
WHERE year != 2020
```
5. названия сборников, в которых присутствует конкретный исполнитель (выберите сами);
```
SELECT c.name FROM collection c
JOIN collectiontracks ct ON c.collectionid = ct.collectionid
JOIN track t ON ct.trackid = t.trackid
JOIN album al ON t.albumid = al.albumid
JOIN artistalbum aa ON al.albumid = aa.albumid
JOIN artist ar ON aa.artistid = ar.artistid
WHERE ar.name = 'Doja Cat'
```
6. название альбомов, в которых присутствуют исполнители более 1 жанра;
```
SELECT albumname, COUNT(genrename) FROM album al
JOIN artistalbum aa ON al.albumid = aa.albumid
JOIN artist ar ON aa.artistid = ar.artistid
JOIN genreartist ga ON ar.artistid = ga.artistid
JOIN genre g ON ga.genreid = g.genreid
GROUP BY albumname
HAVING COUNT(genrename) > 1
```
7. наименование треков, которые не входят в сборники;
```
SELECT trackname FROM track t
FULL OUTER JOIN collectiontracks ct ON t.trackid = ct.trackid
LEFT JOIN collection c ON ct.collectionid = c.collectionid
WHERE c.collectionid IS NULL
```
8. исполнителя(-ей), написавшего самый короткий по продолжительности трек (теоретически таких треков может быть несколько);
```
SELECT ar.name, MIN(t.tracklength) from artist ar
JOIN artistalbum aa ON ar.artistid = aa.artistid
JOIN album al ON aa.albumid = al.albumid
JOIN track t ON al.albumid = t.trackid
GROUP BY ar.name, t.tracklength
ORDER BY t.tracklength
LIMIT 1
```
9. название альбомов, содержащих наименьшее количество треков.
```
SELECT albumname, COUNT(trackname) from album al
JOIN track t ON al.albumid = t.albumid
GROUP BY albumname
ORDER BY COUNT(trackname) ASC
LIMIT 1
```







	
