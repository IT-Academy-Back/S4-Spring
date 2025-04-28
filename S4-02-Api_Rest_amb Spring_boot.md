# Tasca S4.02 Api Rest amb Spring boot

## 📝 Descripció

En aquesta tasca faràs un **CRUD (Create, Read, Update, Delete)** amb 3 bases de dades diferents.

Aprendràs a usar correctament els verbs HTTP i a gestionar els codis de resposta.

---

## ⭐ Nivell 1 — Exercici CRUD amb H2

Accedeix a 👉 [https://start.spring.io/](https://start.spring.io/) i genera un projecte Spring Boot amb les següents
característiques:

### ⚙️ Configuració del projecte

| Paràmetre           | Valor                       |
|---------------------|-----------------------------|
| **PROJECT**         | Maven o Gradle              |
| **LANGUAGE**        | Java                        |
| **SPRING BOOT**     | La darrera versió estable   |
| **Group**           | `cat.itacademy.s04.t02.n01` |
| **Artifact / Name** | `S04T02N01`                 |
| **Description**     | `S04T02N01GognomsNom`       |
| **Package name**    | `cat.itacademy.s04.t02.n01` |
| **PACKAGING**       | Jar                         |
| **JAVA**            | Mínim versió 21             |

### 📦 Dependències

- Spring Boot DevTools
- Spring Web
- Spring Data JPA
- H2 Database

---

### 🍏 Entitat: `Fruita`

Tenim una entitat anomenada "Fruita", que disposa de les següents propietats:

| Camp              | Tipus    |
|-------------------|----------|
| `id`              | `int`    |
| `nom`             | `String` |
| `quantitatQuilos` | `int`    |

---

### 🧱 Estructura de packages (seguint MVC + excepcions)

Aprofitant l’especificació JPA, hauràs de persistir aquesta entitat a una base de dades H2, seguint el patró MVC.
Per a això, depenent del Package principal, crearàs una estructura de packages, on ubicaràs les classes que necessitis:

```
cat.itacademy.s04.t02.n01.controllers
cat.itacademy.s04.t02.n01.model
cat.itacademy.s04.t02.n01.services
cat.itacademy.s04.t02.n01.repository
cat.itacademy.s04.t02.n01.exception
```

---

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

- Hauràs de tenir en compte les bones pràctiques de disseny de les API, fent servir correctament els codis d'error i les
  respostes en cas d'invocacions incorrectes. (Pots consultar informació sobre ResponseEntity).

- Hauràs d'implementar un GlobalExceptionHandler per gestionar les excepcions globalment a l'aplicació. Això permetrà
  capturar i
  tractar errors de manera centralitzada, millorant la robustesa i la coherència en la gestió de les excepcions.

- També hauràs de crear un `Dockerfile` per al projecte, que permeti construir una imatge preparada per a entorns de
  producció.

### ⚠️ Molt Important

A més de l’enllaç a Git de la tasca resolta, hauràs d’incloure almenys dos enllaços diferents dels recursos que t’hem
proporcionat al campus, que t’hagin servit o ho haguessin pogut fer, per resoldre la totalitat de la tasca o algunes
parts.


---

## ⭐⭐ Nivell 2 - Exercici CRUD amb MySQL

Accedeix a 👉 [https://start.spring.io/](https://start.spring.io/) i genera un projecte Spring Boot amb les següents
característiques:

### ⚙️ Configuració del projecte

| Paràmetre           | Valor                       |
|---------------------|-----------------------------|
| **PROJECT**         | Maven o Gradle              |
| **LANGUAGE**        | Java                        |
| **SPRING BOOT**     | La darrera versió estable   |
| **Group**           | `cat.itacademy.s04.t02.n02` |
| **Artifact / Name** | `S04T02N02`                 |
| **Description**     | `S04T02N02`                 |
| **Package name**    | `cat.itacademy.s04.t02.n02` |
| **PACKAGING**       | Jar                         |
| **JAVA**            | Mínim versió 21             |

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

| Paràmetre           | Valor                       |
|---------------------|-----------------------------|
| **PROJECT**         | Maven o Gradle              |
| **LANGUAGE**        | Java                        |
| **SPRING BOOT**     | La darrera versió estable   |
| **Group**           | `cat.itacademy.s04.t02.n03` |
| **Artifact / Name** | `S04T02N03`                 |
| **Description**     | `S04T02N03`                 |
| **Package name**    | `cat.itacademy.s04.t02.n03` |
| **PACKAGING**       | Jar                         |
| **JAVA**            | Mínim versió 21             |

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
