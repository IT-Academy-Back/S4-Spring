# Tasca S4.02 Api Rest amb Spring boot

## 📝 Descripció

En aquesta tasca desenvoluparàs **tres aplicacions Spring Boot independents**, cadascuna amb una API REST que implementa un CRUD complet (_Create, Read, Update, Delete_) sobre diferents entitats. Treballaràs amb **tres bases de dades diferents**: H2, MySQL i MongoDB.

A través d’aquestes pràctiques aprendràs a:

- Crear APIs REST utilitzant Spring Boot.
- Gestionar la persistència de dades amb Spring Data JPA i Spring Data MongoDB.
- Aplicar correctament els verbs HTTP (`GET`, `POST`, `PUT`, `DELETE`) i gestionar adequadament els codis d’estat de les respostes.
- Implementar rutes dinàmiques amb **Path Params** i **Query Params**.
- Escriure i executar **tests automatitzats** utilitzant **TDD (Test-Driven Development)** per assegurar el comportament esperat de la lògica i dels endpoints.
- Gestionar les excepcions globalment mitjançant un `GlobalExceptionHandler`.
- Estructurar correctament el projecte seguint el patró **MVC (Model-View-Controller)**.
- Crear relacions entre entitats utilitzant **JPA**.
- Introduir l’ús de **DTOs** i validar les dades d’entrada amb anotacions de validació.
- Crear un `Dockerfile` per empaquetar l’aplicació en una imatge Docker preparada per a entorns de producció.
- Configurar la connexió a la base de dades a través de **variables d’entorn**.

---

## ⭐ Nivell 1 — Exercici CRUD amb H2

En aquest primer nivell desenvoluparàs una **API REST per gestionar l’estoc d’una fruiteria** mitjançant una aplicació backend construïda amb Spring Boot.  

L’objectiu és permetre **gestionar les entrades d’estoc de fruita**, registrant per a cada una el nom del producte i el seu pes en quilos.

Treballaràs amb una base de dades SQL **en memòria (H2)**, molt utilitzada en entorns de desenvolupament i proves per la seva rapidesa i simplicitat de configuració.

---
### 📖 Històries d'usuari i Criteris d'acceptació

```
⚠️ Et recomanem fer un seguiment de cada una de les següents històries d'usuari utilitzant un tauler Kanban (com GitHub Projects, Trello, etc.). A més, és bona pràctica fer un commit clar i descriptiu un cop completada cada història.
```


#### 1. Registrar una fruita nova

> **Com a** responsable de l’inventari,  
>  **vull** poder afegir una nova entrada de fruita indicant el seu nom i el pes en quilos,  
> **per tal de** mantenir un registre actualitzat del producte entrant.


**Criteris d’acceptació:**
- Si les dades són vàlides, el sistema retorna HTTP 201 Created amb el detall de la fruita.
- Si el nom està buit o el pes no és vàlid, es retorna HTTP 400 Bad Request.

#### 2. Consultar totes les fruites

> **Com a** responsable de l’inventari,  
> **vull** poder visualitzar una llista amb totes les fruites registrades,  
> **per tal de** tenir una visió global de l’estoc disponible.


**Criteris d’acceptació:**
- El sistema retorna HTTP 200 OK i un array JSON amb totes les fruites.
- Si no hi ha fruites registrades, retorna un array buit amb HTTP 200 OK.

#### 3. Consultar una fruita específica

> **Com a** responsable de l’inventari,  
> **vull** poder consultar els detalls d’una fruita concreta a partir del seu identificador,  
> **per tal de** accedir a la informació d’un producte específic de manera eficient.


**Criteris d’acceptació:**
- Si l’ID existeix, el sistema retorna HTTP 200 OK amb el detall de la fruita.
- Si l’ID no existeix, retorna HTTP 404 Not Found amb un missatge indicatiu.

#### 4. Modificar una fruita existent

> **Com a** responsable de l’inventari,  
> **vull** poder actualitzar el nom o el pes registrat d’una fruita,  
> **per tal de** corregir errors o reflectir canvis en la informació del producte.


**Criteris d’acceptació:**
- Si les dades són vàlides, el sistema retorna HTTP 200 OK amb la fruita actualitzada.
- Si l’ID no existeix, retorna HTTP 404 Not Found.
- Si les dades no són vàlides, retorna HTTP 400 Bad Request.

#### 5. Eliminar una fruita

> **Com a** responsable de l’inventari,  
> **vull** poder eliminar una fruita a partir del seu identificador,  
> **per tal de** garantir que l’estoc només contingui informació rellevant i actualitzada.


**Criteris d’acceptació:**
- Si l’ID existeix, el sistema elimina la fruita i retorna HTTP 204 No Content.
- Si l’ID no existeix, el sistema retorna HTTP 404 Not Found amb un missatge d’error.

---
### ⚙️ Configuració del projecte

Accedeix a 👉 [https://start.spring.io/](https://start.spring.io/) i genera un projecte Spring Boot amb les següents
característiques:

| Paràmetre       | Valor                       |
|-----------------|-----------------------------|
| PROJECT         | Maven o Gradle              |
| LANGUAGE        | Java                        |
| SPRING BOOT     | 3.x.x (latest stable)   |
| Group           | `cat.itacademy.s04.t02.n01` |
| Artifact / Name | `fruit-api-h2`                 |
| Description     | `REST API for managing fruit stock with H2`       |
| Package name    | `cat.itacademy.s04.s02.n01.fruit` |
| PACKAGING       | Jar                         |
| JAVA            | 21 (LTS)             |

### 📦 Dependències

- Spring Boot DevTools
- Spring Web
- Spring Data JPA
- H2 Database
- Validation
- Lombok (opcional, però recomanat per reduir codi boilerplate)

----

### 🧩 Enunciat tècnic

Treballaràs amb una entitat anomenada **Fruit**, que tindrà les propietats següents:
#### `Fruit`

```java
Long id  
String name  
int weightInKilos  
```

Aprofitant l'especificació **JPA**, hauràs de persistir aquesta entitat en una base de dades **H2**, seguint l'arquitectura **MVC**.  

Recorda que JPA s’encarregarà de generar automàticament la taula i els valors de l’ID per a cada fruita, utilitzant l’anotació ```@Id``` juntament amb ```@GeneratedValue```.

Organitza el projecte creant els packages següents, segons el teu package principal:

```
cat.itacademy.s04.t02.n01.controllers
cat.itacademy.s04.t02.n01.model
cat.itacademy.s04.t02.n01.services
cat.itacademy.s04.t02.n01.repository
cat.itacademy.s04.t02.n01.exception
```

La classe ubicada dins el package `controllers` (**FruitController**, per exemple) haurà de ser capaç de gestionar les següents operacions a través d'**endpoints** REST:

### 🌐 Endpoints esperats

| Mètode | Endpoint       | Descripció                |
|--------|----------------|---------------------------|
| POST   | `/fruits`      | Crear fruita              |
| PUT    | `/fruits/{id}` | Actualitzar fruita        |
| DELETE | `/fruits/{id}` | Eliminar per id           |
| GET    | `/fruits/{id}` | Obtenir una fruita per id |
| GET    | `/fruits`      | Obtenir totes les fruites |

---

### ⚠️ Important

- Hauràs de tenir en compte les bones pràctiques de disseny de les API, fent servir correctament els codis d'error i les respostes en cas d'invocacions incorrectes. (Pots consultar informació sobre ResponseEntity).

- Hauràs d’evitar exposar directament les entitats JPA als controladors, utilitzant el patró DTO per gestionar les dades d’entrada i sortida, i validant-les amb anotacions de Bean Validation com ```@NotBlank```, ```@NotNull``` o ```@Positive```.

- Hauràs d'implementar un GlobalExceptionHandler per gestionar les excepcions globalment a l'aplicació. Això permetrà capturar i tractar errors de manera centralitzada, millorant la robustesa i la coherència en la gestió de les excepcions.

- Usant una **IA generativa**, hauràs de crear un `Dockerfile` per al projecte que permeti construir una imatge **optimitzada per a entorns de producció**. L’objectiu és entendre **línia per línia** com es genera una imatge mitjançant un **multi-stage build** dividit en dues etapes:
	1. **Etapa de construcció:** compilar l’aplicació i generar l’arxiu `.jar`.
	2. **Etapa final:** copiar només el `.jar` a una imatge lleugera per executar-lo en producció.

- Hauràs de desenvolupar el projecte seguint l’enfocament **TDD (Test-Driven Development)**.  És a dir, abans d’implementar cada funcionalitat, hauràs d’escriure el test corresponent que en defineixi el comportament esperat. Pots utilitzar:
	- `@SpringBootTest` amb `MockMvc`, o bé llibreries com **RestAssured**, per provar els endpoints REST.
	- `Mockito` per testejar els serveis de forma aïllada (unit test).


---

## ⭐⭐ Nivell 2 - Exercici CRUD amb MySQL

En aquest segon projecte ampliaràs la funcionalitat de l’aplicació anterior incorporant la gestió de **proveïdors de fruita** (pots partir del que ja teníes al Nivell 1).
Cada registre de fruita haurà d’estar associat a un proveïdor, fet que et permetrà registrar l’origen de cada producte i consultar quines fruites subministra cada empresa.

Aquest nou projecte utilitzarà **MySQL** com a base de dades i introduirà una relació entre entitats mitjançant **JPA**, concretament una associació de tipus **@ManyToOne** entre `Fruit` i `Provider`.

---

### 📖 Històries d’usuari i criteris d’acceptació

#### 1. Registrar un proveïdor

> **Com a** responsable de compres,  
> **vull** poder afegir nous proveïdors indicant el seu nom i país,  
> **per tal de** portar el control de qui subministra les fruites.

**Criteris d’acceptació:**
- El sistema ha de permetre registrar proveïdors amb nom i país.
- No es poden registrar proveïdors amb el nom en blanc.
- Si el proveïdor s’ha registrat correctament, es retorna HTTP 201 Created.

#### 2. Afegir una fruita amb proveïdor

> **Com a** responsable de compres,  
> **vull** afegir una nova fruita associada a un proveïdor existent,  
> **per tal de** registrar correctament l’origen de cada producte.

**Criteris d’acceptació:**
- Quan es crea una fruita, cal indicar l’ID d’un proveïdor vàlid.
- No es poden afegir fruites sense proveïdor.
- Si el proveïdor no existeix, es retorna HTTP 404 Not Found.
- Si les dades són vàlides, retorna HTTP 201 Created.

#### 3. Filtrar fruites per un proveïdor

> **Com a** gestor d’estoc,  
> **vull** poder veure totes les fruites subministrades per un proveïdor,  
> **per tal de** fer seguiment del seu subministrament.

**Criteris d’acceptació:**
- El sistema ha de permetre consultar fruites filtrant per ID de proveïdor.
- Si el proveïdor existeix, es retorna HTTP 200 OK amb les fruites.
- Si no existeix, es retorna HTTP 404 Not Found.

---
### ⚙️ Configuració del projecte

Accedeix a 👉 [https://start.spring.io/](https://start.spring.io/) i genera un projecte Spring Boot amb les següents
característiques:

| Paràmetre       | Valor                       |
|-----------------|-----------------------------|
| PROJECT         | Maven o Gradle              |
| LANGUAGE        | Java                        |
| SPRING BOOT     | La darrera versió estable   |
| Group           | `cat.itacademy.s04.t02.n02` |
| Artifact / Name | `S04T02N02`                 |
| Description     | `S04T02N02`                 |
| Package name    | `cat.itacademy.s04.t02.n02` |
| PACKAGING       | Jar                         |
| JAVA            | Mínim versió 21             |

### 📦 Dependències

- Spring Boot DevTools
- Spring Web
- Spring Data JPA
- MySQL Driver
- Validation

### 🧩 Enunciat tècnic

Treballaràs amb dues entitats relacionades:
#### `Provider`

```java
Long id  
String name  
String country
```

#### `Fruit`

```java
Long id  
String name  
int weightInKilos  
Provider provider
```

Has de persistir aquestes entitats a una base de dades **MySQL**, gestionant la relació amb **JPA** (`@ManyToOne`).  

---

### 🌐 Endpoints mínims esperats

| Mètode | Endpoint                  | Descripció                     |
| ------ | ------------------------- | ------------------------------ |
| POST   | `/providers`              | Crear proveïdor                |
| GET    | `/providers`              | Llistar proveïdors             |
| POST   | `/fruits`                 | Crear fruita amb proveïdor     |
| GET    | `/fruits?providerId={id}` | Obtenir fruites d’un proveïdor |
| GET    | `/fruits`                 | Llistar totes les fruites      |

A més dels nous endpoints relacionats amb proveïdors, cal que tots els **endpoints del Nivell 1** continuïn funcionant correctament amb la nova estructura de dades.

---

### ⚠️ Important

- Assegura’t de complir també amb tots els requisits no funcionals establerts al Nivell 1.

- Utilitza **DTOs** per gestionar la informació d’entrada i sortida, evitant exposar directament les entitats del model.

- Aplica **validacions** sobre els camps dels DTOs utilitzant anotacions com `@NotBlank`, `@Positive` o `@NotNull`, amb el suport de la llibreria de validació de Spring.

- Utilitzant una **IA Generativa**, crea un `Dockerfile` per empaquetar l’aplicació en una imatge **Docker optimitzada per a producció**, permetent la **configuració de la connexió a la base de dades MySQL mitjançant variables d'entorn**.

- Per facilitar l'entorn de desenvolupament, afegeix un fitxer **docker compose** per aixecar la infraestructura necessària, com ara el servei de base de dades MySQL.

- **Opcional:** Pots complementar la tasca amb **tests d’integració** dels endpoints utilitzant `@SpringBootTest` i `MockMvc`, o/i  **tests unitaris** de serveis amb `Mockito`.

---

## ⭐⭐⭐ Nivell 3 - Exercici CRUD amb MongoDB

En aquest tercer projecte desenvoluparàs una **API REST per gestionar comandes de fruita** realitzades per clients, utilitzant MongoDB com a sistema de persistència.

Cada comanda inclourà el **nom del client**, la **data de lliurament** i una llista de productes amb el seu nom i quantitat en quilos.

Aquest projecte et servirà per practicar la persistència de documents en MongoDB utilitzant documents embeguts.

---

### 📖 Històries d’usuari i criteris d’acceptació

#### 1. Crear una nova comanda

> **Com a** client,  
> **vull** fer una comanda indicant les fruites i quantitats que necessito,  
> **per tal de** rebre la comanda el dia indicat.

**Criteris d’acceptació:**
- El client ha d’indicar el seu nom, la data i almenys una fruita.
- Cada fruita ha de tenir nom i quantitat positiva.
- Si la data és **anterior a demà**, es retorna **HTTP 400 Bad Request** amb un missatge d’error.
- Retorna HTTP 201 Created amb la comanda guardada.


#### 2. Consultar totes les comandes

> **Com a** gestor de comandes,  
> **vull** veure totes les comandes registrades,  
> **per tal de** revisar l’activitat recent.

**Criteris d’acceptació:**
- Retorna HTTP 200 OK amb totes les comandes.
- Si no n’hi ha, retorna una llista buida.


#### 3. Consultar una comanda per ID

> **Com a** gestor,  
> **vull** consultar els detalls d’una comanda específica,  
> **per tal de** revisar-ne el contingut.

**Criteris d’acceptació:**
- Si l’ID existeix, retorna HTTP 200 OK amb la comanda.
- Si no, retorna HTTP 404 Not Found.


#### 4. Modificar una comanda

> **Com a** client,  
> **vull** modificar una comanda ja feta si m’he equivocat,  
> **per tal de** assegurar-me que arribi el que he demanat.

**Criteris d’acceptació:**
- Només es pot modificar si es proporciona un ID vàlid.
- Si les dades són vàlides, retorna HTTP 200 OK.
- Si l’ID no existeix, retorna 404.


#### 5. Eliminar una comanda

> **Com a** gestor,  
> **vull** eliminar una comanda si ha estat cancel·lada,  
> **per tal de** mantenir el sistema actualitzat.

**Criteris d’acceptació:**
- Si l’ID existeix, elimina la comanda i retorna HTTP 204 No Content.
- Si no existeix, retorna HTTP 404 Not Found.


---
### ⚙️ Configuració del projecte

Accedeix a 👉 [https://start.spring.io/](https://start.spring.io/) i genera un projecte Spring Boot amb les següents
característiques:

| Paràmetre       | Valor                       |
|-----------------|-----------------------------|
| PROJECT         | Maven o Gradle              |
| LANGUAGE        | Java                        |
| SPRING BOOT     | La darrera versió estable   |
| Group           | `cat.itacademy.s04.t02.n03` |
| Artifact / Name | `S04T02N03`                 |
| Description     | `S04T02N03`                 |
| Package name    | `cat.itacademy.s04.t02.n03` |
| PACKAGING       | Jar                         |
| JAVA            | Mínim versió 21             |

### 📦 Dependències

- Spring Boot DevTools
- Spring Web
- Spring Data MongoDB
- Validation

### 🧩 Enunciat tècnic

Treballaràs amb una entitat principal anomenada **Order**, que representarà una comanda de fruita realitzada per un client. Cada comanda estarà formada per:

- El nom del client.
- Una data de lliurament (que ha de ser com a mínim demà).
- Una llista d’items, cada un amb el nom de la fruita i la quantitat en quilos.

Utilitzaràs MongoDB per emmagatzemar cada comanda com **un únic document** dins la col·lecció `orders`.

#### `Order` (document principal)

```java
String id;
String clientName;
LocalDate deliveryDate;
List<OrderItem> items;
```

#### `OrderItem` (subdocument embegut)
```java
String fruitName;
int quantityInKilos;
```

### 🌐 Endpoints mínims esperats
| Mètode | Endpoint       | Descripció                              |
| ------ | -------------- | --------------------------------------- |
| POST   | `/orders`      | Crear una nova comanda                  |
| GET    | `/orders`      | Llistar totes les comandes              |
| GET    | `/orders/{id}` | Consultar una comanda per identificador |
| PUT    | `/orders/{id}` | Actualitzar una comanda existent        |
| DELETE | `/orders/{id}` | Eliminar una comanda                    |

---

## 📌 Recursos
#### 🧱 Spring Boot i APIs REST

- [Spring Boot Documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
- [Building a RESTful Web Service (Spring Guides)](https://spring.io/guides/gs/rest-service/)
- [Building REST services with Spring](https://spring.io/guides/tutorials/rest)
- [Spring Web Annotations](https://www.baeldung.com/spring-mvc-annotations)

#### 💾 Persistència de dades

- [Spring Data JPA - Reference Documentation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- [Spring Data MongoDB - Reference Documentation](https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/)
- [Accessing MongoDB Data with REST](https://spring.io/guides/gs/accessing-mongodb-data-rest)
- [Accessing JPA Data with REST](https://spring.io/guides/gs/accessing-data-rest)

#### 🎯 Validació i DTOs

- [Validation in Spring Boot (Baeldung)](https://www.baeldung.com/spring-boot-bean-validation)
- [DTO Pattern - Baeldung](https://www.baeldung.com/java-dto-pattern)
- [Entity To DTO Conversion - Baeldung](https://www.baeldung.com/entity-to-and-from-dto-for-a-java-spring-application)

#### 🐳 Docker

- [Docker Official Documentation](https://docs.docker.com/)
- [Spring Boot with Docker (Spring Guides)](https://spring.io/guides/gs/spring-boot-docker)
- [How To Dockerize A Spring Boot Application With Maven](https://www.geeksforgeeks.org/how-to-dockerize-a-spring-boot-application-with-maven/)
