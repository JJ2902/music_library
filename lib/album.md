```mermaid
sequenceDiagram
    participant t as terminal
    participant app as Main program (in app.py)
    participant ar as album_repository class <br /> (in lib/album_repository.py)
    participant db_conn as DatabaseConnection class in (in lib/database_connection.py)
    participant db as Postgres database

    Note left of t: Flow of time <br />⬇ <br /> ⬇ <br /> ⬇ 

    t->>app: Runs `python app.py`
    app->>db_conn: Opens connection to database by calling connect method on DatabaseConnection
    db_conn->>db_conn: Opens database connection using PG and stores the connection
    app->>ar: Calls all method on album_repository
    ar->>db_conn: Sends SQL query by calling execute method on DatabaseConnection
    db_conn->>db: Sends query to database via the open database connection
    db->>db_conn: Returns an list of albums, one for each row of the album table

    db_conn->>ar: Returns an list of album, one for each row of the album table
    loop 
        ar->>ar: Loops through list and creates a album object for every row
    end
    ar->>app: Returns list of album objects
    app->>t: Prints list of albums to terminal
