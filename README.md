# My Spring Boot Skills Hub 🚀

Bienvenido a mi colección de habilidades (skills) para agentes de IA, diseñadas para potenciar el desarrollo con Spring Boot. Este repositorio funciona como un **Hub**, donde puedes encontrar diferentes herramientas para automatizar tareas repetitivas y mejorar la calidad del código.

## 📚 Catálogo de Skills

| Skill | Descripción | Comando de Instalación |
| :--- | :--- | :--- |
| **Standard CRUD** | El estándar de oro para CRUDs básicos: Capas, Validaciones, Manejo de Errores y H2. | `npx skills add juandmj82/my-spring-boot-skills --skill spring-boot-standard-crud` |
| **Pro CRUD** | Arquitectura Profesional: DTOs con Records, Mapeo Automático (MapStruct), y Manejo Global de Excepciones. | `npx skills add juandmj82/my-spring-boot-skills --skill spring-boot-pro-crud` |

---

## 🛠 Prerrequisitos y Dependencias

Cada skill tiene requerimientos específicos. Al crear tu proyecto en [Spring Initializr](https://start.spring.io/), ten en cuenta lo siguiente:

### 1. Standard CRUD
Para la skill básica, asegúrate de incluir:
- **Spring Web**: Para crear los controladores y la API REST.
- **Spring Data JPA**: Para la persistencia de datos.
- **H2 Database**: Para la base de datos en memoria (ideal para pruebas).
- **Lombok**: Para reducir el código repetitivo (Boilerplate).
- **Validation**: Para el uso de anotaciones como `@NotBlank`, `@Email`, etc.
- **Spring Boot DevTools**: (Opcional) Para reinicio automático del servidor.

### 2. Pro CRUD
Requiere **todas las dependencias del Standard CRUD, MÁS la configuración de MapStruct**.

**⚠️ PASO CRÍTICO: Modificación del Plugin en `pom.xml`**
Para que el mapeo automático (`MapStruct`) funcione correctamente en conjunto con `Lombok`, **DEBES** modificar la configuración del `maven-compiler-plugin` dentro de la etiqueta `<plugins>` de tu archivo `pom.xml`. Debe quedar estructurado de esta forma exacta:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <annotationProcessorPaths>
            <path>
                <groupId>org.mapstruct</groupId>
                <artifactId>mapstruct-processor</artifactId>
                <version>1.5.5.Final</version>
            </path>
            <path>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
            </path>
            <path>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok-mapstruct-binding</artifactId>
                <version>0.2.0</version>
            </path>
        </annotationProcessorPaths>
    </configuration>
</plugin>
```
*(Nota: Cuando la IA ejecute esta skill, también te recordará hacer esta modificación antes de continuar).*

---

## 🛠 Instalación Global
Si quieres instalar **todas** las habilidades de este repositorio de una sola vez, usa:
```bash
npx skills add juandmj82/my-spring-boot-skills
```

---
### 👨‍💻 Sobre el Autor
**JuandiDev** es un apasionado del código en plena transición hacia el desarrollo de software. Tras una larga trayectoria en el sector educativo, a sus 43 años ha decidido volcar su experiencia en el ecosistema **Java & Spring Boot**, formándose en programas como **Oracle Next Education** y **Amazon Junior Software Developer**.

Este repositorio nace con la intención de compartir las herramientas y habilidades que él mismo utiliza para potenciar su aprendizaje y crecimiento profesional. Su enfoque se centra en crear soluciones claras, funcionales y bien estructuradas que ayuden a otros desarrolladores en su camino.

🔗 **Portafolio:** [juandiegodev.vercel.app](https://juandiegodev.vercel.app/)

---
*Diseñado para aprender, construido para escalar.*
