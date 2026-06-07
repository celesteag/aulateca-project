# Registro de Uso Responsable y Transparente de Inteligencia Artificial

Este documento cumple con las directrices académicas y profesionales de transparencia, recopilando de forma estructurada los *prompts* y consultas realizados a herramientas de Inteligencia Artificial como apoyo técnico durante el desarrollo del sistema **Aulateca**.

El uso de la IA en este proyecto se ha enfocado de manera crítica y responsable, empleándose exclusivamente como un asistente avanzado para la resolución de errores del compilador, la optimización de algoritmos de persistencia y la generación formal de la documentación técnica.

---

## 🛠️ Registro de Prompts de Configuración y Resolución de Errores

### Consulta 1: Resolución del bug crítico de compilación en el entorno local
* **Pregunta formulada a la IA:**
  > "java: java.lang.ExceptionInInitializerError com.sun.tools.javac.code.TypeTag :: UNKNOWN"
* **Objetivo técnico:** Solucionar un bloqueo absoluto del compilador de IntelliJ que impedía ejecutar el método `main` de la interfaz gráfica tras desinstalar componentes del pom.
* **Resultado e Implementación:** La herramienta diagnosticó un conflicto severo entre las cachés del IDE y el procesamiento de anotaciones de Java. Se solucionó desactivando el *Annotation Processing*, aplicando un `Invalidate Caches and Restart` en IntelliJ y rebajando el nivel del SDK de la versión *Java 21 Preview (String Templates)* a la versión **Java 21 Estándar**.

### Consulta 2: Resolución del desfase de versiones del SDK del Módulo
* **Pregunta formulada a la IA:**
  > "cambie al 21 como dijiste pero ahora sale este fallo java: error: release version 25 not supported. Module Aulateca SDK 21 is not compatible with the source version 25. Upgrade Module SDK in project settings to 25 or higher. Open project settings."
* **Objetivo técnico:** Corregir el desajuste entre las propiedades leídas por Maven y la estructura de compilación del IDE.
* **Resultado e Implementación:** Se identificaron tres puntos donde residía el identificador de versión. Se corrigió modificando las propiedades del `<pom.xml>` fijando el compilador en `<maven.compiler.source>21</maven.compiler.source>`, modificando el parámetro de *Target bytecode* en los ajustes del compilador de Java en IntelliJ e igualando el *Language level* de los módulos.

---

## 📐 Registro de Prompts de Arquitectura y Lógica de Datos

### Consulta 3: Planteamiento conceptual y rendimiento de relaciones ORM
* **Pregunta formulada a la IA:**
  > "la parte entity que hubo que poner de uno a muchos, haz el resumen mejor"
* **Objetivo técnico:** Validar cuál era la mejor estrategia arquitectónica para mapear relaciones de tablas maestras de recursos y reservas sin degradar la memoria del sistema.
* **Resultado e Implementación:** Se descartó el uso de relaciones bidireccionales con colecciones `@OneToMany` por su ineficiencia en memoria. Se implementaron en su lugar relaciones **unidireccionales Muchos a Uno (`@ManyToOne`)** con estrategias de carga **`FetchType.EAGER`**, permitiendo que Hibernate extraiga los registros mediante sentencias SQL optimizadas con `JOIN` directo.

### Consulta 4: Validación de reglas de negocio sobre mantenimiento y solapamiento horario
* **Pregunta formulada a la IA:**
  > "lee el punto 10 y 11 que hace falta ahora"
* **Objetivo técnico:** Diseñar la lógica necesaria en el Back-end para evitar reservas dobles y bloqueos en recursos inactivos conforme al pliego de especificaciones.
* **Resultado e Implementación:** Se estructuró un flujo defensivo en la capa de lógica comercial (`ReservationService`). Se diseñó una función específica en el repositorio (`ReservationDAO`) con una consulta HQL basada en un `COUNT` parametrizado (`existsReservation`), lanzando excepciones de tipo `IllegalStateException` controladas para cortar el flujo de guardado si se violan las directrices.

---

## 🗂️ Registro de Prompts de Control de Versiones (Git)

### Consulta 5: Resolución de conflictos de colisión de repositorios remotos
* **Pregunta formulada a la IA:**
  > "puse sin querer remote add origin de otro proyecto y al poner el correcto ahora dice que ya existe un origin, como borro el anterior"
* **Objetivo técnico:** Desvincular una URL remota errónea asignada por equivocación en la consola de comandos de Git.
* **Resultado e Implementación:** Se aplicaron comandos de administración de ramas remotas. Se resolvió mediante la instrucción de sobrescritura directa de URL: `git remote set-url origin <URL_CORRECTA>` y se validó el estado de las conexiones con el modificador verboso `git remote -v`.