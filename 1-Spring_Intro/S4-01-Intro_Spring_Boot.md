# Tasca S4.01 Introducció a Spring Boot [IN PROGRESS]

## 🎯 Objectius

Aquest exercici és la teva primera presa de contacte amb **Spring Boot** i el desenvolupament d’una **API REST**.
L’objectiu és construir una API mínima però funcional, que permeti rebre i retornar dades en format **JSON**, utilitzant mètodes HTTP i aplicant bones pràctiques des del primer moment.
Treballaràs amb els següents conceptes clau, que hauràs d’entendre i investigar:

- Què és una **API REST** i com funciona.
- Com definir **endpoints** a través de controladors amb `@RestController`.
- Ús dels mètodes HTTP **GET** i **POST** per recuperar i enviar informació.
- Com rebre dades a través de la URL amb `@PathVariable` i `@RequestParam`.
- Com **rebre dades JSON** a través del cos de la petició amb `@RequestBody`.
- Com **retornar respostes** en format JSON.
- Com **provar manualment** la teva API amb [Postman](https://www.postman.com/) (eina per enviar peticions HTTP).
- Com **provar automàticament** la capa HTTP amb **MockMvc** i `@WebMvcTest`.
- Com compilar i executar el `.jar` generat amb Maven (Spring Boot inclou el servidor integrat Apache Tomcat).
- Què és el concepte d’**Inversió de Control (IoC)** i com es creen i injecten **Beans**.
- Introducció a l’**arquitectura per capes**, i als patrons **Service Layer** i **Repository**.

### 🧱 Configuració del projecte

Crea el projecte a 👉 [https://start.spring.io/](https://start.spring.io/) amb els següents valors:

| Configuració     | Valor                                              |
| ---------------- | -------------------------------------------------- |
| **PROJECT**      | Maven                                              |
| **LANGUAGE**     | Java                                               |
| **SPRING BOOT**  | La darrera versió estable                          |
| **Group**        | `cat.itacademy.s04.t01.n01`                        |
| **Artifact**     | `userapi`                                          |
| **Name**         | `UserAPI`                                          |
| **Description**  | `Primera API d'usuaris`                            |
| **Package name** | `cat.itacademy.s04.t01.n01.userapi`                |
| **PACKAGING**    | Jar                                                |
| **JAVA**         | Versió 21                                          |
| **Dependències** | Spring Web, Spring Boot DevTools, Spring Boot Test |

---

Configura el port en `src/main/resources/application.properties`:

```
server.port=9000
```

---
## ⭐ Nivell 1 — Primera API Rest

Abans de començar a desenvolupar funcionalitats més avançades, ens assegurarem que l’aplicació arrenca correctament i que respon com esperem.

Ho comprovarem de tres maneres:
- Des del navegador
- Amb un client REST com **Postman**
- Mitjançant un **test automatitzat**

Per fer-ho, crearem un **endpoint de health check**: un punt d’entrada molt senzill que retorna una resposta bàsica com ara `"OK"`. Aquest patró és habitual en sistemes reals per verificar que l’aplicació és viva i funcional.

### 👥 Endpoint GET – health

#### 🛠️ Passos a seguir

1. **Crea un package nou anomenat** `controllers` dins el teu `src/main/java/...`
2. Dins aquest package, **crea una classe** `HealthController` i anota-la amb `@RestController`
3. Afegeix un mètode públic, anotal amb `@GetMapping("/health")` i fes que retorni el text `"OK"`

---

### 🧪 Prova des del navegador

1. Executa l’aplicació (`mvn spring-boot:run` o des de l’IDE)
2. Obre el teu navegador preferit i ves a:

```
<http://localhost:9000/health>
```

Si veus el missatge `OK`, tot està funcionant correctament

---

### 🧪 Prova amb Postman

Ara farem la mateixa prova usant **Postman**, un client REST per fer peticions HTTP.

1. Descarrega i instal·la [Postman](https://www.postman.com/downloads/)
2. Crea una nova petició `GET` al mateix endpoint
3. Prem **Send** i comprova que reps el text `OK` com a resposta.

> ✅ Quan hagis confirmat que tot funciona correctament, fes un commit per no perdre els canvis. Recorda utilitzar el format de [**conventional commits**](https://www.conventionalcommits.org) i escriure un missatge clar i en anglès.

📦 Exemple de commit:

```
feat: add basic health check endpoint
```

---

### 🔄 Millora: retornar JSON en comptes de text pla

Fins ara retornaves simplement un `String` amb el text `"OK"`. Tot i que és funcional, en el món real és molt més habitual que les APIs **retornin objectes JSON estructurats**.

L’objectiu és que la teva resposta tingui aquest format:

```json
{
  "status": "OK"
}
```

Això facilita la integració amb altres serveis, la monitorització, i manté una estructura coherent en tota l’API.

### 🛠️ Què has de fer?

1. Crea una nova **classe o `record`** amb una propietat anomenada `status`. Jackson automàticament la convertirà a JSON.
2. Modifica el teu `controller` perquè retorni una instància d’aquest objecte en lloc d’un `String`.

> Un cop ho tinguis, torna a provar el teu endpoint i comprova que reps una resposta JSON amb status a "OK" i fes un altre commit que expliqui el que s’ha fet.

--- 

### 🧪 Primer test bàsic del controlador

Ara que ja tens un endpoint que retorna JSON, és un bon moment per afegir el primer test automàtic.

Farem un test molt bàsic per comprovar que l’endpoint `/health` retorna una resposta que conté un `status`: `OK`.

Aquest tipus de test serveix per **comprovar que el controlador respon correctament a una petició HTTP**, sense necessitat d’aixecar tota l’aplicació. És un test molt comú en Spring Boot, conegut com a **test de la web layer.**

A continuació tens un exemple complet del test **amb comentaris** perquè entenguis cada pas.

```java
// Indiquem que aquest test només carrega la capa web (controladors)
@WebMvcTest
class HealthControllerTest {

    // Injectem MockMvc, que ens permet simular peticions HTTP
    @Autowired
    private MockMvc mockMvc;

    @Test
    void shouldReturnOkStatus() throws Exception {
        // Simulem una petició GET a /health
        mockMvc.perform(get("/health"))
            // Verifiquem que el codi de resposta és 200 OK
            .andExpect(status().isOk())
            // Comprovem que la resposta JSON conté "status": "OK"
            .andExpect(jsonPath("$.status").value("OK"));
    }
}
```

> **Executa el test** des d’IntelliJ o amb Maven: `mvn test` Si el test passa, vol dir que la teva API ja pot ser comprovada automàticament. 👉🏽 Fes un commit amb un missatge clar com: `test: verify /health returns status OK`

---
### 🚀 Executar la teva API com a `.jar`

Spring Boot genera un arxiu `.jar` executable amb tot el necessari (incloent el servidor Tomcat) perquè puguis **executar la teva aplicació com si fos un programa independent**.

#### 🛠️ Passos per empaquetar i executar

1. Obre un terminal i col·loca’t a l’arrel del projecte.
2. Executa la comanda següent per generar el `.jar`:

   ```bash
   mvn clean package
   ```

3. Si tot ha anat bé, trobaràs un arxiu `.jar` dins la carpeta `target/`. L’arxiu es dirà:

   ```
   userapi-0.0.1-SNAPSHOT.jar
   ```

4. Ara pots executar la teva aplicació amb:

   ```bash
   java -jar target/userapi-0.0.1-SNAPSHOT.jar
   ```

5. Un cop arrencada, torna al navegador o Postman i comprova que el teu endpoint `/health` segueix funcionant a:

   ```
   http://localhost:9000/health
   ```

> ✅ Fes una captura de pantalla de la terminal amb l'execució del `.jar` i guarda-la al teu repositori com a evidència del funcionament.


---

## ⭐⭐ Nivell 2