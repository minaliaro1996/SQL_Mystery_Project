# SQL_Mystery_Project

CREATE TABLE players (
 player_id INT PRIMARY KEY,
 player_name VARCHAR(50)
);

INSERT INTO players VALUES
(1, 'Alice'), (2, 'Bob'), (3, 'Charlie'), (4, 'Daisy'), (5, 'Ethan');

CREATE TABLE rooms (
 room_id INT PRIMARY KEY,
 room_name VARCHAR(100)
);

INSERT INTO rooms VALUES
(1, 'Puzzle of Shadows'),
(2, 'Code Lock Chamber'),
(3, 'Mirror Maze'),
(4, 'The Riddle Room'),
(5, 'Final Escape Gate');

CREATE TABLE player_progress (
 progress_id INT PRIMARY KEY,
 player_id INT,
 room_id INT,
 entry_time DATETIME,
 exit_time DATETIME,
 FOREIGN KEY (player_id) REFERENCES players(player_id),
 FOREIGN KEY (room_id) REFERENCES rooms(room_id)
);

INSERT INTO player_progress VALUES
-------First player journey values-----------------
(1, 1, 1, '2023-01-01 10:00:00', '2023-01-01 10:15:00'),
(2, 1, 2, '2023-01-01 10:15:00', '2023-01-01 10:30:00'),
(3, 1, 3, '2023-01-01 10:30:00', '2023-01-01 10:50:00'),
(4, 1, 4, '2023-01-01 10:50:00', '2023-01-01 11:10:00'),
(5, 1, 5, '2023-01-01 11:10:00', '2023-01-01 11:45:00');

/*Note : For every player progress journey you need to insert values (above values is only for 1st player journey (player_id=1)
---insert for another 4 players as well-----------*/

/*Q.1 Total time taken by Each player */

SELECT 
 p.player_name,
 MIN(pp.entry_time) AS start_time,
 MAX(pp.exit_time) AS end_time,
 datediff(MINUTE, MIN(pp.entry_time), MAX(pp.exit_time)) AS total_minutes
FROM 
 player_progress pp
JOIN players p ON p.player_id = pp.player_id
GROUP BY p.player_name



/*Q.2 Who escaped within 2-hour limit */

SELECT *
FROM (
 SELECT 
 p.player_name,
 DATEDIFF(MINUTE, MIN(pp.entry_time), MAX(pp.exit_time)) AS total_time_minutes
 FROM 
 player_progress pp
 JOIN 
 players p ON p.player_id = pp.player_id
 GROUP BY 
 p.player_name
) AS derived
WHERE total_time_minutes < 120;

/*Q.3 Room with the Longest Average Time*/

SELECT 
 r.room_name,
 AVG(DATEDIFF(MINUTE, pp.entry_time, pp.exit_time)) AS avg_time_minutes
FROM 
 player_progress pp
JOIN 
 rooms r ON r.room_id = pp.room_id
GROUP BY 
 r.room_name
ORDER BY 
 avg_time_minutes DESC;
