# Tema 1: Introducció a Spring Boot

Aquest tema t’introdueix als conceptes fonamentals per entendre què és una API REST i com es crea una aplicació amb Spring Boot. L’objectiu és oferir-te una visió sòlida del funcionament d’aquest tipus d’aplicacions abans de començar a programar, i posar-te a l’abast recursos fiables que et permetin aprofundir i consolidar els coneixements quan ho necessitis.

---

## 🌐 Protocol HTTP

HTTP és el protocol que permet la comunicació entre clients (navegador, Postman, aplicacions mòbils…) i servidors. Cada interacció consta d’una petició i d’una resposta.

Conceptes essencials:
- **Mètodes HTTP**: GET, POST, PUT, DELETE…
- **Codis d’estat**: 200, 201, 400, 404, 500…
- **Headers** i contingut del cos (body)
- Diferència entre:
  - **Query params**: `?name=ana`
  - **Path variables**: `/users/123`
  - **Request body JSON**

**Recurs principal:**  
**[Generalidades del Protocolo HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Overview)**

Altres recursos recomanats:  
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview  
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status  
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods  

---

## 🔗 REST API

REST és un estil d’arquitectura per dissenyar APIs senzilles, previsibles i consistents. Es basa en recursos (com *users*, *products*) i en l’ús correcte dels verbs HTTP.

Elements clau:
- Rutes basades en **substantius** (`/users`)
- Ús semàntic dels verbs HTTP
- Respostes en **JSON**
- Codis d’estat coherents
- Diferència entre col·leccions i recursos individuals (`/users/{id}`)

**Recursos principals:**  
- **[Learn REST API Design](https://www.restapitutorial.com/)**  
- **[REST: Good Practices for API Design](https://medium.com/hashmapinc/rest-good-practices-for-api-design-881439796dc9)**

Altres recursos útils:
- https://restfulapi.net/  
- https://martinfowler.com/articles/richardsonMaturityModel.html  

---

## 🧩 Microserveis

Els microserveis són petites aplicacions independents que treballen conjuntament. Cada microservei és responsable d’una única funcionalitat i es pot desplegar de manera autònoma.

Spring Boot és molt utilitzat en aquest entorn per la seva rapidesa, flexibilitat i facilitat de desplegament.

**Recurs principal:**  
**[Microservices – Definition, Principles and Benefits](https://howtodoinjava.com/microservices/microservices-definition-principles-benefits/)**

Altres:
- https://martinfowler.com/articles/microservices.html  
- https://www.redhat.com/en/topics/microservices/what-are-microservices  

---

## 🧪 Proves amb Postman

Postman és una eina indispensable per provar manualment una API REST. Permet enviar peticions, modificar paràmetres, enviar JSON i analitzar les respostes.

**Recurs principal:**  
**[Getting Started with Postman](https://learning.postman.com/docs/getting-started/introduction/)**

Vídeo pràctic:  
**[How to Send a Request in Postman](https://www.youtube.com/watch?v=7E60ZttwIpY)**

Altres recursos:
- https://learning.postman.com/docs/sending-requests/requests/  
- https://learning.postman.com/docs/sending-requests/intro-to-collections/  

---

## 🌱 Spring i Spring Boot

Spring és un ecosistema molt complet per desenvolupar aplicacions Java. Spring Boot simplifica enormement el procés gràcies a la seva autoconfiguració i al servidor integrat.

Conceptes essencials:
- `@RestController`
- `@GetMapping`, `@PostMapping`
- Autoconfiguració amb *starters*
- Execució del projecte (`mvn spring-boot:run`)
- Servidor Tomcat integrat
- Starters principals:
  - `spring-boot-starter-web`
  - `spring-boot-starter-test`

**Recursos principals:**  
- **[What is Spring Boot?](https://www.baeldung.com/spring-boot)**  
- **[Spring Quickstart](https://spring.io/quickstart)**  

---

## 🧠 IoC i Dependency Injection (Beans de Spring)

Spring incorpora un contenidor de dependències que gestiona la creació i injecta objectes (**beans**) automàticament dins de controladors, serveis i repositoris.

Conceptes importants:
- IoC (Inversió de Control)
- Dependency Injection (DI)
- `@Component`
- `@Service`
- `@Repository`
- `@Controller`

**Recurs principal:**  
https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring  

Altres:
- https://www.baeldung.com/spring-bean  
- https://docs.spring.io/spring-framework/reference/core/beans.html  

---

## 🏛️ Arquitectura per capes  
(Controller – Service – Repository)

Una bona pràctica en aplicacions Spring és separar clarament les responsabilitats:

- **Controller** → gestiona peticions HTTP  
- **Service** → lògica de negoci (validacions, regles, processos)  
- **Repository** → accés a dades (memòria, base de dades…)  

Això millora la testabilitat i facilita el manteniment.

**Recursos recomanats:**  
- **[Spring Boot Code Structure](https://www.geeksforgeeks.org/java/spring-boot-code-structure/)**
- **[Spring Boot – Architecture (GeeksforGeeks)](https://www.geeksforgeeks.org/spring-boot-architecture/)**  
- **[Understanding Controller, Service, and Repository in Spring Boot](https://www.compilemymind.com/posts/spring-boot-layered-architecture/)**  



---

## 🔄 Jackson i JSON

Spring Boot utilitza **Jackson** com a llibreria per convertir automàticament objectes Java en JSON i a l’inrevés.

Punts importants:
- `@RequestBody` → deserialització  
- Retorn d’objectes → serialització automàtica  
- Validació de dades  
- Tractament d’errors de format

**Recursos recomanats:**  
- https://www.baeldung.com/jackson-object-mapper-tutorial
- https://www.baeldung.com/spring-boot-json  

---

## 📦 Maven (build, dependències, empaquetar .jar)

Maven gestiona:
- Les dependències del projecte  
- El cicle de build (`clean`, `test`, `package`)  
- La creació del `.jar` executable  

**Recursos recomanats:**  
- https://maven.apache.org/guides/getting-started/  

---

## ⚠️ Error Handling a APIs REST

És important controlar correctament els errors i retornar respostes clares als clients.

Eines de Spring:
- `@ResponseStatus`
- Exceptions personalitzades
- `@RestControllerAdvice`
- `@ExceptionHandler`

**Recurs recomanat:**  
https://www.baeldung.com/exception-handling-for-rest-with-spring  

---

## 🧪 Testing amb Spring Boot, MockMvc i Mockito

Hauràs d’escriure tres tipus de tests:

### 1) Tests de controladors  
`@WebMvcTest` + MockMvc  
→ Permeten provar només la capa web sense arrencar tota l’aplicació.

### 2) Tests d’integració o acceptació 
`@SpringBootTest` + `@AutoConfigureMockMvc`  
→ Simulen peticions HTTP i proven totes les capes juntes.

### 3) Tests unitaris de servei  
Amb Mockito (`@Mock` + `@InjectMocks`)  
→ Proven la lògica de negoci de forma aïllada.

**Recursos recomanats:**  
- Testing oficial: https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.testing  
- Guia oficial Web Layer: https://spring.io/guides/gs/testing-web/  
- MockMvc & SpringBootTest: https://www.baeldung.com/spring-boot-testing  
- Mockito: https://www.baeldung.com/mockito-series  

---

## 🔑 Altres conceptes útils

### UUID  
Molts endpoints generen identificadors únics amb `UUID.randomUUID()`.  
Documentació: https://docs.oracle.com/javase/8/docs/api/java/util/UUID.html

### Lombok (opcional)  
Evita escriure getters, setters i constructors manualment.  
https://projectlombok.org/

---

Aquest conjunt de temes i recursos et dona totes les bases per completar amb èxit la teva **primera API REST amb Spring Boot**.
