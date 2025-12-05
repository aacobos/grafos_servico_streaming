# Modelagem de Dados em Grafos de um Serviço de Streaming

Problema:
Você foi contratado por um novo serviço de streaming de filmes e séries e sua primeira tarefa é projetar o banco de dados.
Diferente dos sistemas tradicionais, a empresa quer focar nos relacionamentos para criar um sistema de recomendação poderoso.

Modele e crie um pequeno grafo de conhecimento para este serviço.

O modelo deve incluir:
- Entidades (nós): User, Movie, Series, Genre, Actor, Director
- Conexão (relacionamentos): WATCHED (com propriedade rating), ACTED_IN, DIRECTED, IN_GENRE.

Entrega:
- Um diagrama ou esboço do seu modelo de grafo.
- Um script cypher (.cypher) que cria constraints para os nós (ex. UNIQUE para IDs) e popula o banco com pelo menos 10 usuários, 10 filmes/séries e seus respectivos relacionamentos.

---
### CONSTRAINTS
CREATE CONSTRAINT user_id IF NOT EXISTS
FOR (u:User) REQUIRE u.id IS UNIQUE;

CREATE CONSTRAINT movie_id IF NOT EXISTS
FOR (m:Movie) REQUIRE m.id IS UNIQUE;

CREATE CONSTRAINT series_id IF NOT EXISTS
FOR (s:Series) REQUIRE s.id IS UNIQUE;

CREATE CONSTRAINT actor_id IF NOT EXISTS
FOR (a:Actor) REQUIRE a.id IS UNIQUE;

CREATE CONSTRAINT director_id IF NOT EXISTS
FOR (d:Director) REQUIRE d.id IS UNIQUE;

CREATE CONSTRAINT genre_id IF NOT EXISTS
FOR (g:Genre) REQUIRE g.name IS UNIQUE;

---
### NODES
#### Create User:
- CREATE (:User {id: 1, name: "Alice"});
- CREATE (:User {id: 2, name: "Bruno"});
- CREATE (:User {id: 3, name: "Carla"});
- CREATE (:User {id: 4, name: "Diego"});
- CREATE (:User {id: 5, name: "Eva"});
- CREATE (:User {id: 6, name: "Fernando"});
- CREATE (:User {id: 7, name: "Gabriela"});
- CREATE (:User {id: 8, name: "Henrique"});
- CREATE (:User {id: 9, name: "Isabela"});
- CREATE (:User {id: 10, name: "João"});

#### Create Genre:
- CREATE (:Genre {name: "Action"});
- CREATE (:Genre {name: "Drama"});
- CREATE (:Genre {name: "Comedy"});
- CREATE (:Genre {name: "Sci-Fi"});
- CREATE (:Genre {name: "Thriller"});
- CREATE (:Genre {name: "Animation"});

#### Create Movie:
- CREATE (:Movie {id: 101, title: "Matrix"});
- CREATE (:Movie {id: 102, title: "Inception"});
- CREATE (:Movie {id: 103, title: "Interstellar"});
- CREATE (:Movie {id: 104, title: "Toy Story"});
- CREATE (:Movie {id: 105, title: "The Dark Knight"});

#### Create Series:
- CREATE (:Series {id: 201, title: "Breaking Bad"});
- CREATE (:Series {id: 202, title: "Stranger Things"});
- CREATE (:Series {id: 203, title: "The Office"});
- CREATE (:Series {id: 204, title: "The Mandalorian"});
- CREATE (:Series {id: 205, title: "Sherlock"});

#### Create Actor:
- CREATE (:Actor {id: 301, name: "Keanu Reeves"});
- CREATE (:Actor {id: 302, name: "Leonardo DiCaprio"});
- CREATE (:Actor {id: 303, name: "Matthew McConaughey"});
- CREATE (:Actor {id: 304, name: "Tom Hanks"});
- CREATE (:Actor {id: 305, name: "Bryan Cranston"});
- CREATE (:Actor {id: 306, name: "Millie Bobby Brown"});
- CREATE (:Actor {id: 307, name: "Steve Carell"});
- CREATE (:Actor {id: 308, name: "Benedict Cumberbatch"});

#### Create Director:
- CREATE (:Director {id: 401, name: "Lana Wachowski"});
- CREATE (:Director {id: 402, name: "Christopher Nolan"});
- CREATE (:Director {id: 403, name: "John Lasseter"});
- CREATE (:Director {id: 404, name: "Vince Gilligan"});
- CREATE (:Director {id: 405, name: "Jon Favreau"});

---
### RELATIONSHIPS

#### Movie in Genre
- MATCH (m:Movie {id: 101}), (g:Genre {name: "Sci-Fi"}) CREATE (m)-[:IN_GENRE]->(g);
- MATCH (m:Movie {id: 102}), (g:Genre {name: "Thriller"}) CREATE (m)-[:IN_GENRE]->(g);
- MATCH (m:Movie {id: 103}), (g:Genre {name: "Sci-Fi"}) CREATE (m)-[:IN_GENRE]->(g);
- MATCH (m:Movie {id: 104}), (g:Genre {name: "Animation"}) CREATE (m)-[:IN_GENRE]->(g);
- MATCH (m:Movie {id: 105}), (g:Genre {name: "Action"}) CREATE (m)-[:IN_GENRE]->(g);

#### Series in Genre
- MATCH (s:Series {id: 201}), (g:Genre {name: "Drama"}) CREATE (s)-[:IN_GENRE]->(g);
- MATCH (s:Series {id: 202}), (g:Genre {name: "Sci-Fi"}) CREATE (s)-[:IN_GENRE]->(g);
- MATCH (s:Series {id: 203}), (g:Genre {name: "Comedy"}) CREATE (s)-[:IN_GENRE]->(g);
- MATCH (s:Series {id: 204}), (g:Genre {name: "Sci-Fi"}) CREATE (s)-[:IN_GENRE]->(g);
- MATCH (s:Series {id: 205}), (g:Genre {name: "Thriller"}) CREATE (s)-[:IN_GENRE]->(g);

#### Actor in Movie
- MATCH (a:Actor {id: 301}), (m:Movie {id: 101}) CREATE (a)-[:ACTED_IN]->(m);
- MATCH (a:Actor {id: 302}), (m:Movie {id: 102}) CREATE (a)-[:ACTED_IN]->(m);
- MATCH (a:Actor {id: 303}), (m:Movie {id: 103}) CREATE (a)-[:ACTED_IN]->(m);
- MATCH (a:Actor {id: 304}), (m:Movie {id: 104}) CREATE (a)-[:ACTED_IN]->(m);
- MATCH (a:Actor {id: 302}), (m:Movie {id: 105}) CREATE (a)-[:ACTED_IN]->(m);

#### Actor in Series
- MATCH (a:Actor {id: 305}), (s:Series {id: 201}) CREATE (a)-[:ACTED_IN]->(s);
- MATCH (a:Actor {id: 306}), (s:Series {id: 202}) CREATE (a)-[:ACTED_IN]->(s);
- MATCH (a:Actor {id: 307}), (s:Series {id: 203}) CREATE (a)-[:ACTED_IN]->(s);
- MATCH (a:Actor {id: 308}), (s:Series {id: 205}) CREATE (a)-[:ACTED_IN]->(s);

#### Director in Movie
- MATCH (d:Director {id: 401}), (m:Movie {id: 101}) CREATE (d)-[:DIRECTED]->(m);
- MATCH (d:Director {id: 402}), (m:Movie {id: 102}) CREATE (d)-[:DIRECTED]->(m);
- MATCH (d:Director {id: 402}), (m:Movie {id: 103}) CREATE (d)-[:DIRECTED]->(m);
- MATCH (d:Director {id: 403}), (m:Movie {id: 104}) CREATE (d)-[:DIRECTED]->(m);
- MATCH (d:Director {id: 402}), (m:Movie {id: 105}) CREATE (d)-[:DIRECTED]->(m);

#### Director in Serie
- MATCH (d:Director {id: 404}), (s:Series {id: 201}) CREATE (d)-[:DIRECTED]->(s);
- MATCH (d:Director {id: 405}), (s:Series {id: 204}) CREATE (d)-[:DIRECTED]->(s);
- MATCH (d:Director {id: 402}), (s:Series {id: 205}) CREATE (d)-[:DIRECTED]->(s);

### Rating in Movie
- MATCH (u:User {id: 1}), (m:Movie {id: 101}) CREATE (u)-[:WATCHED {rating: 5}]->(m);
- MATCH (u:User {id: 2}), (m:Movie {id: 102}) CREATE (u)-[:WATCHED {rating: 4}]->(m);
- MATCH (u:User {id: 3}), (m:Movie {id: 103}) CREATE (u)-[:WATCHED {rating: 5}]->(m);
- MATCH (u:User {id: 4}), (m:Movie {id: 104}) CREATE (u)-[:WATCHED {rating: 3}]->(m);
- MATCH (u:User {id: 5}), (m:Movie {id: 105}) CREATE (u)-[:WATCHED {rating: 5}]->(m);

### Rating in Series
- MATCH (u:User {id: 6}),  (s:Series {id: 201}) CREATE (u)-[:WATCHED {rating: 5}]->(s);
- MATCH (u:User {id: 7}),  (s:Series {id: 202}) CREATE (u)-[:WATCHED {rating: 4}]->(s);
- MATCH (u:User {id: 8}),  (s:Series {id: 203}) CREATE (u)-[:WATCHED {rating: 4}]->(s);
- MATCH (u:User {id: 9}),  (s:Series {id: 204}) CREATE (u)-[:WATCHED {rating: 5}]->(s);
- MATCH (u:User {id: 10}), (s:Series {id: 205}) CREATE (u)-[:WATCHED {rating: 5}]->(s);
