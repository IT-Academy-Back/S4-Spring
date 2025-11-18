# Tema 2: Desenvolupament d’APIs avançades amb Spring Boot

Aquest segon tema aprofundeix en tot allò que comença a aparèixer quan una API deixa de ser un simple exercici i comença a assemblar-se a un projecte real: persistència, validació, relacions entre dades, gestió d’errors, proves automatitzades i desplegament.  
L’objectiu no és només entendre la tecnologia, sinó comprendre els **motivat** que hi ha darrere de cada decisió arquitectònica i de disseny.

A partir d’aquí, el que construeixis amb Spring Boot ja no serà “un CRUD”, sinó una aplicació backend amb criteri i estructura.

---

# 🧱 1. Persistència avançada en Spring Boot

A mesura que una API creix, la persistència es converteix en un punt central. Spring ofereix dos camins principals: **bases de dades SQL** (H2, MySQL) i **bases de dades NoSQL** (MongoDB). Cada enfoc té la seva lògica i els seus avantatges.

## 1.1 Bases de dades SQL amb JPA (H2 i MySQL)

Les bases de dades relacionals continuen essent l’opció natural quan necessites consistència, relacions ben definides i consultes estructurades.

**Per què utilitzar JPA:**
- Evita escriure SQL repetitiu
- Proporciona un model de dades coherent amb les classes Java
- Permet mapar relacions (OneToMany, ManyToOne…)
- Simplifica enormement les operacions CRUD

**H2** s’utilitza sovint en entorns de desenvolupament i proves perquè:
- És lleugera
- No requereix instal·lació
- Permet una integració immediata amb tests

**MySQL**, en canvi, aporta:
- Persistència real
- Integració amb entorns de producció
- Control més fi de rendiment i configuració

**Recursos recomanats:**
- https://docs.spring.io/spring-data/jpa/docs/current/reference/html/
- https://spring.io/guides/gs/accessing-data-jpa/
- https://www.baeldung.com/spring-data-jpa-tutorial

---

## 1.2 Persistència NoSQL amb MongoDB

MongoDB adopta un model documental, més flexible i menys restrictiu que les bases de dades SQL. Aquest enfoc funciona molt bé en casos com:
- Models que evolucionen ràpidament
- Continguts semiestructurats
- Col·leccions grans amb consultes senzilles
- Documents enriquits amb subdocuments

No s’hi defineixen esquemes rígids: tu decideixes la forma del document.

**Recursos recomanats:**
- https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/
- https://spring.io/guides/gs/accessing-mongodb-data-rest/
- https://www.mongodb.com/docs/manual/core/data-model-design/

---

# 🧬 2. Spring Data: un motor per unificar accés a dades

Una de les fortaleses de Spring és **Spring Data**, que proporciona una capa comuna per treballar amb diferents tecnologies de persistència.

Spring Data funciona seguint filosofia **“repositoris derivats”**, que permet escriure interfícies i obtenir funcionalitats completes:

```java
interface FruitRepository extends JpaRepository<Fruit, Long> {
    List<Fruit> findByName(String name);
}
```

Això evita haver de crear DAOs manuals i et permet centrar-te en el model i la lògica.

**Recursos recomanats:**
- https://spring.io/projects/spring-data
- https://www.baeldung.com/spring-data-repositories

---

# 🔗 3. Relacions i models de dades

Quan el teu projecte comença a tenir entitats relacionades, has de prendre decisions arquitectòniques que marcaran la qualitat del codi.

## 3.1 Relacions en SQL

### Les anotacions més rellevants:
- `@OneToMany`
- `@ManyToOne`
- `@OneToOne`
- `@ManyToMany`

Però més enllà de l’anotació, el que importa és entendre:

- Quan vols **propagar** els canvis (`cascade`)  
- Quan vols **carregar** dades automàticament (`fetch = EAGER`) o sota demanda (`LAZY`)
- Quan té sentit un **bidirectional mapping** i quan no

La regla general en APIs REST és mantenir models nets, evitar relacions bidireccionals quan no són necessàries i usar DTOs per evitar exposar de forma incontrolada l’arbre complet d’objectes.

## 3.2 Documents embeguts en MongoDB

En MongoDB, és habitual tenir subdocuments, com ara:

```java
class Order {
    List<OrderItem> items;
}
```

A diferència de JPA, aquí no hi ha “relacions” estrictes ni joins. Aquest model és ideal quan:
- Les dades tenen sentit conjuntament
- La lectura del document és més freqüent que la seva modificació
- No cal consultar els subdocuments de manera aïllada

---

# 🧰 4. DTOs, Validació i Disseny d’entrada/sortida

És temptador treballar directament amb entitats, però no és una bona pràctica. A mesura que una API creix, exposar el model intern es converteix en un problema.

## Quan utilitzar DTOs:
- Per controlar quines dades exposes
- Per validar les entrades del client
- Per mantenir desacoblat el model persistent del model públic
- Per evitar problemes amb relacions internes

## Validació
Spring permet validar dades automàticament amb anotacions com:
- `@NotBlank`
- `@NotNull`
- `@Positive`
- `@Size`

I es pot integrar fàcilment amb un `@RestControllerAdvice` per capturar errors i oferir respostes clares.

**Recursos recomanats:**
- https://www.baeldung.com/spring-boot-bean-validation
- https://www.baeldung.com/java-dto-pattern

---

# 🏗️ 5. Arquitectura avançada en Spring Boot

El patró MVC continua essent útil, però en projectes que creixen ràpidament pot ser insuficient. Quan tens múltiples dominis, l’estructura tradicional…

```
controller/
service/
repository/
model/
```

…pot acabar generant carpetes massives on tot està barrejat.

## Opció recomanada: *Feature architecture*

En comptes de separar per tipus de classe, es separa per funcionalitat:

```
fruit/
   controller/
   service/
   dto/
   repository/
provider/
   controller/
   service/
   ...
```

Això té avantatges clars:
- És molt més modular
- Evita carpetes infinites
- Permet eliminar funcionalitats senceres sense afectar la resta
- Escala molt millor en equips grans i en microserveis

No és obligatori, però sí una pràctica totalment alineada amb com treballen els equips professionals.

**Recursos recomanats:**
- https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/
- https://www.baeldung.com/spring-boot-clean-architecture
- https://martinfowler.com/bliki/PackageByFeature.html

---

# 🛡️ 6. Gestió professional d’errors

En una API complexa no només vols retornar errors correctes, sinó fer-ho de forma coherent, previsible i uniforme.

## Spring proporciona:
- `@ResponseStatus` → per indicar l’estat de forma simple
- `@ExceptionHandler` → per interceptar errors específics
- `@RestControllerAdvice` → per una gestió global

Un bon handler global:
- evita repeticions
- et permet definir un **model d’error** consistent
- converteix excepcions de validació en respostes clares

**Recursos recomanats:**
- https://www.baeldung.com/exception-handling-for-rest-with-spring

---

# 🧪 7. Testing amb Spring Boot i TDD

Quan una aplicació madura, els tests deixen de ser una opció i passen a ser una eina de treball. La qualitat d’un backend es veu molt més en els tests que en el nombre de funcionalitats.

## Tres nivells de proves recomanats:

### 1. Tests de servei  
Amb Mockito, útils per provar la lògica en aïllament.

### 2. Tests del web layer  
Amb `@WebMvcTest` i MockMvc, útils per assegurar que els endpoints funcionen com toca.

### 3. Tests d’integració  
`@SpringBootTest` + repositori real o H2, per comprovar el flux complet.

TDD no és només “fer tests abans”, sinó **pensar amb la mentalitat de validar el comportament**, no la implementació.

**Recursos recomanats:**
- https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.testing
- https://spring.io/guides/gs/testing-web/
- https://www.baeldung.com/spring-boot-testing

---

# 🐳 8. Docker i Multi-Stage Build

Quan prepares una aplicació per executar-la fora del teu IDE, necessites empaquetar-la correctament. Docker permet fer-ho d’una manera neta, portable i controlada.

## Per què fer servir un *multi-stage build*:
- Elimina tot allò innecessari de la imatge final
- Redueix la mida de la imatge
- Millora temps de desplegament
- Aïlla dependències de desenvolupament de les de producció

En Spring Boot, els multi-stage builds acostumen a tenir:
1. **Etapa de construcció** → compila i crea el JAR
2. **Etapa final** → només conté el JAR

**Recursos recomanats:**
- https://docs.docker.com/
- https://spring.io/guides/gs/spring-boot-docker/

---

# 📚 9. Recursos generals recomanats

### Spring Boot i APIs REST
- https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/
- https://spring.io/guides/gs/rest-service/
- https://spring.io/guides/tutorials/rest/

### Persistència
- JPA: https://spring.io/guides/gs/accessing-data-jpa/
- MongoDB: https://spring.io/guides/gs/accessing-mongodb-data-rest/

### Validació i DTOs
- https://www.baeldung.com/spring-boot-bean-validation
- https://www.baeldung.com/java-dto-pattern

### Docker
- https://docs.docker.com/
- https://spring.io/guides/gs/spring-boot-docker/

