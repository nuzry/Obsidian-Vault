
### Setting up

1. I started a fire-up a spring boot
![[Pasted image 20260912130611.png]]

2. Added all the dependencies for the project
![[Pasted image 20260912130708.png]]

  - Spring Web - Used this dependency for HTTP server, request routing and JSON conversion
  - Spring Data JPA - SQL Queries, database connection pooling and object mapping
  - PostgreSQL Driver - Raw TCP protocol to talk to PostgreSQL
  - Lombok - Hundreds of getter/setter/constructor methods by hand
  - DevTools - Manually restart the app on every code change

3. Connected the database
![[Pasted image 20260912131957.png]]

 - connected the database to the project with the these values that creates a connection between  the project and the db.

3. Designing the Database Schema

![[diagram.mattrimony.drawio.png]]

 - Spent a good amount of time refining the schema to avoid any changes after starting the project build.

4. As the next step of the project i started with creating ENUMs that are going to be needed to the project with there values.
![[Pasted image 20260912132313.png]]

5. Next i started creating models of entities that will help me to map database structure when saving and retrieving data from the db.