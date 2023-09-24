## Database Structure

The `nba-api` interfaces with a backend MSSQL database designed to comprehensively capture details related to NBA games, players, shots, and other associated entities. The database is normalized and consists of various tables that store information about games played, individual players, the shots they've taken, the arenas where games are hosted, and more.

Below is an Entity Relationship Diagram (ERD) that visualizes the structure of the database:

```mermaid
erDiagram
    GamesPlayed {
        GameID string PK
        DateTime datetime
        VisitorID string FK
        HomeID string FK
        VisitorPTS integer
        HomePTS integer
        OT string
        Attendance integer
        ArenaID string FK
        GameTypeID string FK
    }

    Player {
        PlayerID string PK
        PlayerName string
        From integer
        To integer
        PositionID string FK
        Height string
        Weight(lbs) int
        BirthDate datetime
        College string
    }

    Positions {
        PositionID string PK
        PositionName string
    }

    PlayerTeams {
        PlayerID string FK
        TeamID string FK
        StartDate datetime
        EndDate datetime
    }

    Teams {
        TeamID string PK
        TeamName string
        TeamShort string
    }

    ShotsTaken {
        ShotID uuid PK
        PlayerName string FK
        TimeLeft string
        TeamName string FK
        ScoreStatus string FK
        xShotPos integer
        yShotPos integer
        Quarter string
        FullText string
        GameDate datetime
        GameID string FK
    }

    ShotTypes {
        ShotTypeID string PK
        Description string
    }

    GameTypes {
        GameTypeID string PK
        Description string
    }

    Arena {
        ArenaID string PK
        ArenaName string
        Capacity integer
        BuiltYear integer
        LocationID string FK
    }

    Location {
        LocationID string PK
        City string
        State string
        Country string
    }

    Player |o--o{ PlayerTeams : "plays for"
    Teams ||--o{ PlayerTeams : "has"
    Player ||--o{ ShotsTaken : "takes"
    Teams ||--o{ ShotsTaken : "team of"
    GamesPlayed ||--o{ ShotsTaken : "has"
    Positions ||--o{ Player : "position of"
    GameTypes ||--o{ GamesPlayed : "type of"
    Teams ||--o{ GamesPlayed : "visitor team of"
    Teams ||--o{ GamesPlayed : "home team of"
    Arena ||--o{ GamesPlayed : "played at"
    Location ||--o{ Arena : "located at"
    ShotTypes ||--o{ ShotsTaken : "type of shot"

```

### Key Points:
- **GamesPlayed**: This table captures details about individual games, including the teams that played, scores, attendance, and the type of game.
- **Player**: Information about individual NBA players, including their name, active years, physical attributes, and the position they play.
- **ShotsTaken**: This table records every shot taken, detailing the player, the game in which the shot was taken, the position on the court, and the outcome.
- **Teams**: Details of NBA teams.
- **Arena**: Information about the arenas where games are played.
- **Location**: Captures the location of each arena.
- **PlayerTeams**: A bridging table that records which player played for which team and during what period.
- **Positions**, **ShotTypes**, and **GameTypes**: These are lookup tables that provide descriptions for various categories.

Relationships between these entities are depicted in the ERD, showing how data is interrelated and can be queried to provide comprehensive insights.
