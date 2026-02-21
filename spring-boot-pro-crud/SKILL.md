---
name: spring-boot-pro-crud
description: Professional-grade CRUD generation featuring MapStruct for DTO mapping, Java Records, Global Exception Handling, and Data Transfer Objects to avoid Exposing Entities.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
category: backend
tags: [spring-boot, java, crud, mapstruct, dtos, records, exception-handling, pro-level]
version: 1.0.0
---

# Spring Boot Professional CRUD (Nivel Pro)

## Overview

Elevate the standard CRUD API to a professional level by completely decoupling the database layer from the client. This skill implements the **DTO Pattern** using immutable Java Records, automates the mapping process intelligently with **MapStruct**, and controls all error outputs via a **Global Exception Handler** (`@RestControllerAdvice`).

## When to Use

- When building enterprise-ready REST APIs.
- When you need to hide sensitive database fields (like `id`, `createdAt`, `password`) from the API output.
- When validating input data securely without polluting the Entity model.
- When you want to eliminate the "Whitelabel Error Page" and return standardized JSON error messages instead.
- Trigger phrases: **"implement pro level CRUD for [Entity]"**, **"crear CRUD profesional con DTOs y MapStruct"**, **"refactorizar a nivel pro con manejo de excepciones"**.

## Instructions

Follow these steps strictly to implement the Pro CRUD pattern:

### 0. Dependency Verification & Critical MapStruct Setup
Before implementing, verify the standard dependencies in `pom.xml` (`spring-boot-starter-web`, `data-jpa`, `validation`, `lombok`).

**⚠️ CRITICO PARA EL AGENTE:** Debes indicarle al usuario que se agregarán nuevas configuraciones y que **DEBE recargar Maven** (Load Maven Changes) al terminar.

Añade la dependencia de MapStruct:
```xml
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct</artifactId>
    <version>1.5.5.Final</version>
</dependency>
```

**⚠️ MODIFICACIÓN OBLIGATORIA DEL PLUGIN:** Para que MapStruct funcione con Lombok, debes modificar la configuración del `maven-compiler-plugin` en el bloque `<build><plugins>` del `pom.xml`. Debe lucir exactamente así:
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
*Dile explícitamente al usuario: "Recarga tu proyecto Maven ahora mismo para que MapStruct funcione correctamente".*

### 1. Manejo Global de Errores (El Guardián)
Crea las clases en el paquete `.exception`:
- **`ErrorResponse.java`**: Un `record` simple con `(String mensaje, String detalles, LocalDateTime fecha)`.
- **`ResourceNotFoundException.java`**: Extiende de `RuntimeException` para errores "404 Not Found".
- **`GlobalExceptionHandler.java`**: Anotado con `@RestControllerAdvice`. Debe capturar `ResourceNotFoundException` (devuelve 404) y `MethodArgumentNotValidException` (devuelve 400 mapeando los errores de `@Valid`).

### 2. Patrón DTO (Java Records)
Crea el paquete `.dto`:
- **`[Entity]RequestDTO.java`**: Es un `record`. SOLO contiene las validaciones (`@NotBlank`, `@Email`, etc.) y NO lleva anotaciones JPA (`@Column`). NO contiene el ID (lo genera la BD).
- **`[Entity]ResponseDTO.java`**: Es un `record`. SÍ contiene el ID. NO lleva anotaciones de validación. Contiene solo los campos seguros para mostrar al público.

### 3. Entidad y Repositorio
- La Entity (`.model`) sigue siendo la clase `@Entity`, pero asegúrate de removerle todas las validaciones asumiendo que ya pasaron por el DTO (excepto las constraints `@Column(nullable=false, unique=true)`).
- El Repository es un simple `JpaRepository`.

### 4. El Mapper Automático
Crea el paquete `.mapper`:
- Crea la interfaz **`[Entity]Mapper.java`**:
  ```java
  @Mapper(componentModel = "spring")
  public interface [Entity]Mapper {
      [Entity]ResponseDTO toResponseDTO([Entity] entity);
      
      @Mapping(target = "id", ignore = true)
      [Entity] toEntity([Entity]RequestDTO dto);
      
      @Mapping(target = "id", ignore = true)
      void updateEntity([Entity]RequestDTO dto, @MappingTarget [Entity] entity);
  }
  ```

### 5. Service Layer (El Coordinador)
El Service implementa la lógica usando el Mapper:
- Inyecta tanto el Repository como el Mapper.
- Para **Crear**: usa `mapper.toEntity(dto)`, guarda, e inmediatamente usa `mapper.toResponseDTO(guardado)`.
- Para **Actualizar**: usa `repository.findById()` (lanza `ResourceNotFoundException` si falla), usa `mapper.updateEntity(dto, entidadExistente)`, guarda y devuelve el `ResponseDTO`.
- El Entity `Cliente` u objeto de negocio *nunca* debe salir del Service al Controller.

### 6. Controller (El Despachador Ciego)
- El Controller *solo* recibe `RequestDTO` usando `@Valid @RequestBody` y *solo* retorna `ResponseDTO`.
- No inyecta el `Repository`, ni conoce el `Mapper`. Solo delega al `Service`.

## Mejores Prácticas Integradas
- **DTOs como Records**: Menos código basura, más seguridad por ser inmutables.
- **Fail-Fast Validation**: El `GlobalExceptionHandler` ataja la petición mala en el controlador antes de que toque la capa Service.
- **MapStruct > Mapeo Manual**: Evita escribir código "espagueti" al pasar datos `a.setNome(b.getNome())`. 
