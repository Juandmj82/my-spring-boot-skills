# My Spring Boot Skills 🚀

Este repositorio contiene una colección de skills para agentes de IA, diseñadas para automatizar y estandarizar el desarrollo con Spring Boot.

## Skill: Standard CRUD

El objetivo principal de esta skill es **facilitar la práctica básica de un CRUD** funcional, siguiendo los estándares de la industria que se esperan de un desarrollador **Nivel Junior**.

### ¿Qué la hace especial?
Aunque el enfoque es **básico y directo**, no sacrifica la calidad. La skill implementa un "Estándar de Oro" para principiantes:

- **Estructura de Capas Limpia**: Controller -> Service -> Repository -> Entity.
- **Validación Profesional**: Uso de Jakarta Validation para asegurar que los datos sean correctos desde el inicio.
- **Manejo de Errores Global**: Una API que no "explota", sino que responde con JSONs claros y legibles.
- **Configuración "Zero-Setup"**: Configura automáticamente H2 en memoria y logueo de SQL formateado para que puedas probar tu código al instante.

### ¿Cómo usarla?
Si tienes instalada la CLI de skills, puedes agregar este repositorio a tu entorno:

```bash
npx skills add juandmj82/my-spring-boot-skills
```

Luego, en cualquier proyecto Spring Boot, simplemente pide al agente:
> "Implementa un CRUD estándar para [NombreEntidad]"

---
*Hecho para aprender, diseñado para construir.*

Desarrollado por [JuandiDev](https://juandiegodev.vercel.app/) 💻
