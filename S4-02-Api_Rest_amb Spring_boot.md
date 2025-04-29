# Tasca S4.02 Api Rest amb Spring boot

## 📝 Descripció

En aquesta tasca desenvoluparàs tres aplicacions Spring Boot independents, cadascuna serà una API REST amb un CRUD complet (Create, Read, Update, Delete) sobre una entitat, utilitzant tres bases de dades diferents: H2, MySQL i MongoDB.

A través d’aquesta pràctica aprendràs a:

- Crear APIs REST utilitzant Spring Boot.
- Gestionar la persistència de dades amb Spring Data JPA i Spring Data MongoDB.
- Aplicar correctament els verbs HTTP (`GET`, `POST`, `PUT`, `DELETE`) i gestionar adequadament els codis d'estat de les respostes.
- Implementar un `GlobalExceptionHandler` per gestionar les excepcions de manera centralitzada.
- Estructurar correctament el projecte segons el patró MVC (Model-View-Controller).
- Crear un `Dockerfile` per empaquetar el projecte en una imatge Docker preparada per a entorns de producció.
- Configurar la connexió a bases de dades a través de variables d'entorn.

Cada nivell correspondrà a un projecte diferent, amb les seves pròpies configuracions i especificacions.

---

## ⭐ Nivell 1 — Exercici CRUD amb H2

En aquest primer nivell traballarem amb una base de dades SQL en memoria. Molt usada per a desenvolupament ràpid i test.

Accedeix a 👉 [https://start.spring.io/](https://start.spring.io/) i genera un projecte Spring Boot amb les següents
característiques:

### ⚙️ Configuració del projecte

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

---

### 📖 Històries d'usuari

1. **Crear una fruita nova**

> **Com a** fruiter,  
> **vull** poder registrar una nova fruita amb el seu nom i pes en quilos,  
> **per a** tenir control de les entrades al rebost.

2. **Consultar totes les fruites**

> **Com a** fruiter,  
> **vull** poder veure una llista de totes les fruites registrades,  
> **per a** consultar ràpidament l'estoc disponible.

3. **Consultar una fruita concreta**

> **Com a** fruiter,  
> **vull** poder consultar la informació d'una fruita concreta a partir del seu identificador,  
> **per a** veure els detalls d'una entrada específica.

4. **Editar una fruita**

> **Com a** fruiter,  
> **vull** poder actualitzar el nom o el pes d'una fruita,  
> **per a** corregir errors d'entrada de dades.

5. **Eliminar una fruita**

> **Com a** fruiter,  
> **vull** poder eliminar una fruita a partir del seu identificador,  
> **per a** mantenir el rebost actualitzat.

---

### ✅ Criteris d'acceptació

- Quan creo una fruita amb un nom vàlid i un pes positiu, el sistema ha de retornar un codi **HTTP 201 Created** i el detall de la fruita creada en format JSON.
    
- Si intento crear una fruita amb un **nom que ja existeix**, el sistema ha de retornar un error **HTTP 409 Conflict** amb un missatge indicant que la fruita ja està registrada.
    
- Si intento crear una fruita amb un **nom buit** o un **pes negatiu o zero**, el sistema ha de retornar un error **HTTP 400 Bad Request**.
    
- Quan faig una petició per obtenir totes les fruites (`GET /fruits`), el sistema ha de retornar un **HTTP 200 OK** i un array JSON amb totes les fruites existents.
    
- Quan faig una petició per obtenir una fruita concreta (`GET /fruits/{id}`) amb un ID vàlid, el sistema ha de retornar un **HTTP 200 OK** amb la informació de la fruita.
    
- Si consulto una fruita amb un **ID que no existeix**, el sistema ha de retornar un error **HTTP 404 Not Found**.
    
- Quan actualitzo una fruita existent (`PUT /fruits/{id}`) amb dades vàlides, el sistema ha de retornar un **HTTP 200 OK** i la fruita actualitzada.
    
- Si intento actualitzar una fruita que **no existeix**, el sistema ha de retornar un error **HTTP 404 Not Found**.
    
- Quan elimino una fruita existent (`DELETE /fruits/{id}`), el sistema ha de retornar un **HTTP 204 No Content**.
    
- Si intento eliminar una fruita amb un **ID inexistent**, el sistema ha de retornar un error **HTTP 404 Not Found**.

### 🧩 Enunciat

Treballaràs amb una entitat anomenada **Fruit**, que tindrà les propietats següents:

- `Long id`
- `String name`
- `int weightInKilos`

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

---

## ⭐⭐ Nivell 2 - Exercici CRUD amb MySQL

Accedeix a 👉 [https://start.spring.io/](https://start.spring.io/) i genera un projecte Spring Boot amb les següents
característiques:

### ⚙️ Configuració del projecte

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

Has de fer el mateix que al nivell 1, però persistint les dades a MySQL.

### ⚠️ Molt Important

A més de l’enllaç a Git de la tasca resolta, hauràs d’incloure almenys dos enllaços diferents dels recursos que t’hem
proporcionat al campus, que t’hagin servit o ho haguessin pogut fer, per resoldre la totalitat de la tasca o algunes
parts.

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

### ⚠️ Molt Important

A més de l’enllaç a Git de la tasca resolta, hauràs d’incloure almenys dos enllaços diferents dels recursos que t’hem
proporcionat al campus, que t’hagin servit o ho haguessin pogut fer, per resoldre la totalitat de la tasca o algunes
parts.

---

## 📌 Recursos
