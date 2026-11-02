# Multiplayer Treasure Hunt Game - Mini Project Report

**Course:** Java Programming Lab  
**Student Name:** Varun Kumar S  
**Register Number:** 25011101125
**Date:** 02/11/2026

## 1. Objective

This project implements a multiplayer Treasure Hunt game using Java, Spring Boot, MySQL, and REST APIs. Multiple players can register, view a virtual treasure map, collect treasures, earn points, and view the leaderboard.

## 2. Technologies Used

- Java 21
- Spring Boot
- Spring Data JPA and Hibernate
- MySQL 8
- Maven
- VS Code
- PowerShell REST testing

## 3. System Design

The application follows a layered design:

```text
REST Controller -> Game Service -> Repository -> MySQL Database
```

- The **controller** receives API requests.
- The **service** contains game rules and score logic.
- The **repositories** access MySQL using JPA.
- The database stores players, treasures, and score history.

## 4. Database Design

The database `treasure_hunt_db` contains three tables:

- `players`: player ID, name, and total score.
- `treasures`: treasure location, point value, collection status, and collecting player.
- `score_history`: records every successful collection and points earned.

## 5. REST APIs

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/players` | Register a player |
| POST | `/api/game/start` | Start/reset the game and generate treasures |
| GET | `/api/game/map` | View all treasures on the virtual map |
| POST | `/api/game/collect/{treasureId}` | Collect a treasure |
| GET | `/api/leaderboard` | View players ranked by score |
| GET | `/api/players/{playerId}/score` | View one player's score |

## 6. Concurrency Handling

The application prevents duplicate treasure collection through database-level pessimistic locking.

When a player attempts to collect a treasure, the application uses `PESSIMISTIC_WRITE` locking inside a transaction. This locks the selected treasure row while collection is processed. If another player attempts to collect the same treasure simultaneously, only one request succeeds; the other receives HTTP `409 Conflict`.

This ensures scores and treasure ownership remain consistent even during concurrent gameplay.

## 7. Testing

The following tests were completed:

1. Registered players successfully using `POST /api/players`.
2. Started a game and generated five treasures.
3. Retrieved the game map using `GET /api/game/map`.
4. Collected treasures and verified player score updates.
5. Retrieved the leaderboard and verified ranking by score.
6. Sent simultaneous collection requests for the same treasure.
7. Verified that only one player could collect the treasure.
8. Packaged and executed the application as a standalone JAR.

Testing screenshots are included in the `evidence` folder.

## 8. Execution Instructions

1. Create the MySQL database using `database/treasure_hunt.sql`.
2. Update database credentials in `src/main/resources/application.properties` if required.
3. Run the application during development:

```powershell
.\mvnw.cmd spring-boot:run
```

4. Build the executable JAR:

```powershell
.\mvnw.cmd package
```

5. Run the JAR:

```powershell
java -jar target\treasure-hunt-0.0.1-SNAPSHOT.jar
```

## 9. Conclusion

The project demonstrates Spring Boot REST API development, MySQL persistence, exception handling, Java concurrency control, and executable JAR packaging. The application meets the required multiplayer Treasure Hunt functionality and prevents inconsistent score updates during simultaneous treasure collection.