# Tasca S4.02 Api Rest amb Spring boot

## 📝 Descripció

En aquesta tasca desenvoluparàs **tres aplicacions Spring Boot independents**, cadascuna amb una API REST que implementa un CRUD complet (_Create, Read, Update, Delete_) sobre diferents entitats. Treballaràs amb **tres bases de dades diferents**: H2, MySQL i MongoDB.

A través d’aquestes pràctiques aprendràs a:

- Crear APIs REST utilitzant Spring Boot.
- Gestionar la persistència de dades amb Spring Data JPA i Spring Data MongoDB.
- Aplicar correctament els verbs HTTP (`GET`, `POST`, `PUT`, `DELETE`) i gestionar adequadament els codis d’estat de les respostes.
- Implementar rutes dinàmiques amb **Path Params** i **Query Params**.
- Gestionar les excepcions globalment mitjançant un `GlobalExceptionHandler`.
- Estructurar correctament el projecte seguint el patró **MVC (Model-View-Controller)**.
- Crear relacions entre entitats utilitzant **JPA**.
- Introduir l’ús de **DTOs** i validar les dades d’entrada amb anotacions de validació.
- Crear un `Dockerfile` per empaquetar l’aplicació en una imatge Docker preparada per a entorns de producció.
- Configurar la connexió a la base de dades a través de **variables d’entorn**.

---

## ⭐ Nivell 1 — Exercici CRUD amb H2

En aquest primer nivell desenvoluparàs una **API REST per gestionar l’estoc d’una fruiteria** mitjançant una aplicació backend construïda amb Spring Boot.  
L’objectiu és poder **registrar, consultar, modificar i eliminar fruites**, cada una identificada pel seu nom i el seu pes en quilos.  
Treballaràs amb una base de dades SQL **en memòria (H2)**, molt utilitzada en entorns de desenvolupament i proves per la seva rapidesa i simplicitat de configuració.

---
### 📖 Històries d'usuari i Criteris d'acceptació

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
| SPRING BOOT     | La darrera versió estable   |
| Group           | `cat.itacademy.s04.t02.n01` |
| Artifact / Name | `S04T02N01`                 |
| Description     | `S04T02N01GognomsNom`       |
| Package name    | `cat.itacademy.s04.t02.n01` |
| PACKAGING       | Jar                         |
| JAVA            | Mínim versió 21             |

### 📦 Dependències

- Spring Boot DevTools
- Spring Web
- Spring Data JPA
- H2 Database
- Validation

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

- Hauràs d'implementar un GlobalExceptionHandler per gestionar les excepcions globalment a l'aplicació. Això permetrà capturar i tractar errors de manera centralitzada, millorant la robustesa i la coherència en la gestió de les excepcions.

- També hauràs de crear un `Dockerfile` per al projecte, que permeti construir una imatge preparada per a entorns de producció.

-  **Opcional:** Pots complementar la tasca amb **tests d’integració** dels endpoints utilitzant `@SpringBootTest` i `MockMvc`, o/i  **tests unitaris** de serveis amb `Mockito`.

---

## ⭐⭐ Nivell 2 - Exercici CRUD amb MySQL

En aquest segon projecte ampliaràs la funcionalitat de l’aplicació anterior incorporant la gestió de **proveïdors de fruita**.  
Cada fruita haurà d’estar associada a un proveïdor, fet que et permetrà registrar l’origen de cada producte i consultar quines fruites subministra cada empresa.

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

- Crea un **Dockerfile** per empaquetar l’aplicació en una imatge Docker i permetre la configuració de la connexió a la base de dades mitjançant variables d'entorn.

- Per facilitar l'entorn de desenvolupament, afegeix un fitxer **docker compose** per aixecar la infraestructura necessària, com ara el servei de base de dades MySQL.

- **Opcional:** Pots complementar la tasca amb **tests d’integració** dels endpoints utilitzant `@SpringBootTest` i `MockMvc`, o/i  **tests unitaris** de serveis amb `Mockito`.

---

## ⭐⭐⭐ Nivell 3 - Exercici CRUD amb MongoDB

Accedeix a 👉 [https://start.spring.io/](https://start.spring.io/) i genera un projecte Spring Boot amb les següents
característiques:

### ⚙️ Configuració del projecte

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

Has de fer el mateix que al nivell 1, però persistint les dades a MongoDB.

---

## 📌 Recursos
