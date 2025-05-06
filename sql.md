## SQL Scripts for Olympics Data

👉 [Medal Count Per Country](https://github.com/Blake-Allan-Smith/Olympics-Data/releases/download/scripts/medals.sql)  
👉 [Gender Participation Per Olympic Games](https://github.com/Blake-Allan-Smith/Olympics-Data/releases/download/scripts/gender_participation.sql)  
👉 [Most Successful Olympic Athletes](https://github.com/Blake-Allan-Smith/Olympics-Data/releases/download/scripts/athlete_success.sql)  
👉 [Most Successful Sport Per Country](https://github.com/Blake-Allan-Smith/Olympics-Data/releases/download/scripts/most_successful_sport_per_country.sql)


DROP TABLE IF EXISTS `massive-catfish-453323-c9.olympics_dataset.medals_per_year`;

CREATE TABLE `massive-catfish-453323-c9.olympics_dataset.medals_per_year` AS 
SELECT Year, NOC, COUNT(*) AS Medal_count, COUNTIF(Medal = 'Bronze') as Bronze_count, COUNTIF(Medal = 'Silver') as Silver_count, COUNTIF(Medal = 'Gold') as Gold_count
FROM `massive-catfish-453323-c9.olympics_dataset.olympics_data`
WHERE Medal != 'NA'
GROUP BY NOC, Year
ORDER BY Year;

DROP TABLE IF EXISTS `massive-catfish-453323-c9.olympics_dataset.gender_participation`;

CREATE TABLE `massive-catfish-453323-c9.olympics_dataset.gender_participation` AS
SELECT COUNTIF(Sex = 'M') AS Male, COUNTIF(Sex = 'F') AS Female, Year, Season FROM `massive-catfish-453323-c9.olympics_dataset.olympics_data`
GROUP BY Year, Season
ORDER BY Year;

DROP TABLE IF EXISTS `massive-catfish-453323-c9.olympics_dataset.athlete_success`;

CREATE TABLE `massive-catfish-453323-c9.olympics_dataset.athlete_success` AS
  WITH athlete_success AS (
    SELECT
    Name,
    COUNT(*) AS appearences,
    COUNTIF(Medal != 'NA') as medals
    FROM `massive-catfish-453323-c9.olympics_dataset.olympics_data`
    GROUP BY Name
  ),
  athlete_rank AS (
    SELECT
    *,
    RANK() OVER (PARTITION BY Name ORDER BY medals) AS top_athletes
    FROM athlete_success
  )
  SELECT
  Name,
  top_athletes,
  appearences,
  medals
  FROM athlete_rank
  ORDER BY medals DESC;

  
