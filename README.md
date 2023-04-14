# Project Overview 

This project utilises SQL Server to explore statistics and match results from games from the FIFA World Cup 1930 - 2018. 

*Before the data could be explored, some data cleaning had to be done*

-----Data Cleaning

SELECT *
FROM worldcups

SELECT *
FROM wcmatches

-- Remove " from Japan and South Korea

UPDATE worldcups
SET host = REPLACE(host, '"', '')
WHERE host = '"Japan'

UPDATE worldcups
SET winner = REPLACE(LTRIM(RTRIM(winner)), '"', '')
WHERE winner = ' South Korea"'

-- Changing column type from VARCHAR to INT

ALTER TABLE worldcups
ALTER COLUMN year INT;

ALTER TABLE wcmatches
ALTER COLUMN year INT;

ALTER TABLE wcmatches
ALTER COLUMN home_score INT;

ALTER TABLE wcmatches
ALTER COLUMN away_score INT;

ALTER TABLE worldcups
ALTER COLUMN attendance INT;

-- 

UPDATE worldcups
SET goals_scored = '161'
WHERE goals_scored = 'South Korea';

UPDATE worldcups
SET teams = '32'
WHERE teams = '161';

UPDATE worldcups
SET games = '32'
WHERE games = '64';

UPDATE worldcups
SET attendance = '2724604'
WHERE attendance = '64,2724604';


-- **Questions**

**--1. How many World Cup games are there in this data?**

SELECT COUNT(*) as total_games
from wcmatches;


**--2. How many games have been played in each World Cup?**

SELECT year, COUNT(year) as total_games
FROM wcmatches
GROUP BY year
ORDER BY year;


**--3. How many goals have been scored in each World Cup?**

SELECT year, SUM(home_score + away_score) AS total_goals_scored
FROM wcmatches
GROUP BY year
ORDER BY year;


**--4. List the countries that have won the World Cup and no.of times they have won it.**

SELECT winner as country, COUNT(winner) as total_world_cups_won
FROM worldcups
GROUP BY winner
ORDER BY total_world_cups_won DESC;


**--5. Which country has won the most matches**

SELECT winning_team as country, COUNT(winning_team) as total_wins
FROM wcmatches
GROUP BY winning_team
ORDER BY total_wins DESC;


**--6. List all the matches and World Cup winners.**

SELECT wcmatches.*, worldcups.*
FROM wcmatches
JOIN worldcups 
ON wcmatches.year = worldcups.year;


**--7. List the country that has finished in second place the most times.**

SELECT TOP 1 second as country, COUNT(*) AS total_second_place
FROM worldcups
GROUP BY second
ORDER BY total_second_place DESC;


**--8. List the country that has finished in third place the most times.**

SELECT TOP 1 third as country, COUNT(*) AS total_third_place
FROM worldcups
GROUP BY third
ORDER BY total_third_place DESC;

**--9. List the country that has finished in fourth place the most times.**

SELECT TOP 1 fourth as country, COUNT(*) AS total_fourth_place
FROM worldcups
GROUP BY fourth
ORDER BY total_fourth_place DESC;

**--10. Find the average number of goals scored per World Cup tournament.**

SELECT AVG(goals_scored) AS avg_goals_per_tournament
FROM worldcups;

--need to use 'CAST' function to convert goals_scored column to an 'INT'

SELECT AVG(CAST(goals_scored AS INT)) AS avg_goals_per_tournament
FROM worldcups;

**--11. Find the top 5 World Cup finals with the highest attendance**

SELECT TOP 5 year, host, winner, attendance
FROM worldcups
ORDER BY attendance DESC;

**--12. Which country has hosted the World Cup the most?**

SELECT host, COUNT(host) as no_of_times
FROM worldcups
GROUP BY host
ORDER BY COUNT(host) DESC

**--13. Find the day of the week with the highest number of matches played.**

SELECT TOP 1 dayofweek, COUNT(*) as match_count
FROM wcmatches
GROUP BY dayofweek
ORDER BY match_count DESC;

**--14. List the World Cup with the highest number of goals scored by a single team.**

SELECT TOP 1 year, SUM(home_score + away_score) AS highest_goals_scored
FROM wcmatches
GROUP BY year
ORDER BY highest_goals_scored DESC;

**--15. List the countries that finished in top three positions in each World Cup.**

SELECT year, winner AS first, second, third 
FROM worldcups
ORDER BY year;


















