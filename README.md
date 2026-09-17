# Práctica 1: Modelo Entidad-Relación

**Alumno:** José Luis Villanueva Morales
**Boleta:** 2024630016
**Grupo:** _(completa tu grupo, p. ej. 3CM4)_
**Carrera:** Ingeniería en Sistemas Computacionales
**Unidad de aprendizaje:** Bases de Datos — ESCOM, IPN

## Índice

| # | Contenido | Ubicación |
|---|---|---|
| 1 | Investigación y práctica de Git/GitHub | Este README (sección Ejercicio 1) |
| 2 | Investigación y práctica de Docker | Este README (sección Ejercicio 2) y [`entorno/compose.yaml`](entorno/compose.yaml) |
| 3 | Investigación: qué es una base de datos | [`docs/investigacion-bases-de-datos.pdf`](docs/investigacion-bases-de-datos.pdf) |
| 4 | Estado del arte: tres artículos científicos | [`docs/estado-del-arte.pdf`](docs/estado-del-arte.pdf) |
| 5 | Caso de estudio y modelo entidad-relación | [`docs/caso-de-estudio.pdf`](docs/caso-de-estudio.pdf) y [`modelo/diagrama-er.png`](modelo/diagrama-er.png) |

---

## Ejercicio 1. Control de versiones con Git y GitHub

### Parte A. Investigación

**1. ¿Qué es un sistema de control de versiones y qué problema resuelve en un trabajo en equipo?**

Un sistema de control de versiones (VCS) es una herramienta que registra los cambios realizados sobre un conjunto de archivos a lo largo del tiempo, permitiendo recuperar cualquier versión anterior, comparar cambios y saber quién modificó qué y cuándo. En un trabajo en equipo, el problema central que resuelve es la edición concurrente: sin un VCS, dos personas que trabajan sobre el mismo archivo terminan sobrescribiendo el trabajo de la otra, o se ven obligadas a coordinarse manualmente (por ejemplo, mandándose el archivo por correo con nombres como "proyecto_final_v3_bueno.zip"). Git resuelve esto permitiendo que cada quien trabaje en su propia copia y luego integrando ("fusionando") los cambios de forma controlada, detectando automáticamente cuándo dos cambios chocan.

**2. ¿Cuál es la diferencia entre Git y GitHub?**

Git es el sistema de control de versiones en sí: un programa que se ejecuta localmente en la computadora de cada desarrollador y que no depende de ningún servidor para funcionar. GitHub, en cambio, es un servicio web que aloja repositorios de Git en la nube y añade funciones de colaboración que Git por sí mismo no tiene: interfaz gráfica, Pull Requests, gestión de incidencias (Issues), control de acceso por equipos, integración continua, etc. Es decir, Git es la herramienta; GitHub es una plataforma que usa esa herramienta como base. Podría usarse Git sin GitHub (por ejemplo, con un repositorio solo local, o alojado en GitLab o Bitbucket).

**3. Conceptos clave**

- **Repositorio:** carpeta cuyo historial de cambios es rastreado por Git. Ejemplo: la carpeta `practica1-bd/` de este proyecto.
- **Confirmación (commit):** una "fotografía" del estado de los archivos en un momento dado, con un mensaje que describe qué cambió. Ejemplo: `git commit -m "Agrega compose.yaml con servicio de PostgreSQL"`.
- **Rama (branch):** una línea de desarrollo independiente que parte de un punto del historial. Permite trabajar en una función nueva sin afectar la rama principal. Ejemplo: crear la rama `feature/gitignore` para agregar el `.gitignore` sin tocar `main` directamente.
- **Fusión (merge):** el proceso de integrar los cambios de una rama en otra. Ejemplo: fusionar `feature/gitignore` en `main` una vez revisado el cambio.
- **Conflicto de fusión:** ocurre cuando dos ramas modificaron las mismas líneas de un archivo de forma distinta y Git no puede decidir automáticamente cuál conservar; requiere que una persona lo resuelva a mano.
- **Pull Request:** una solicitud, hecha en GitHub, para fusionar los cambios de una rama en otra, que además sirve como espacio de discusión y revisión de código antes de aceptarlos.
- **Archivo `.gitignore`:** lista de patrones de archivos o carpetas que Git debe ignorar (no rastrear), como archivos temporales, credenciales o carpetas de dependencias (`node_modules/`).
- **Archivo `README.md`:** documento de presentación del repositorio; explica qué es el proyecto, cómo usarlo y quién lo hizo.

**4. Flujo de trabajo basado en ramas y revisión entre pares**

Un flujo basado en ramas consiste en mantener una rama principal (`main`) siempre estable y funcional, mientras cada cambio nuevo (una función, una corrección) se desarrolla en una rama separada creada a partir de `main`. Cuando el cambio está terminado, se abre un Pull Request para fusionarlo de vuelta. El código se revisa entre pares antes de fusionarse porque una segunda persona detecta errores, malas prácticas o casos no considerados que el autor original, por estar inmerso en el problema, puede pasar por alto; además, la revisión distribuye el conocimiento del código entre el equipo y evita que la calidad del proyecto dependa del criterio de una sola persona.

### Parte B. Práctica

> Pasos a realizar en tu computadora (no pueden ejecutarse por ti):

1. Crea una cuenta en GitHub (si no la tienes) y un repositorio llamado, por ejemplo, `practica1-bd`.
2. Clona el repositorio y copia dentro esta estructura (README.md, .gitignore, compose.yaml, carpetas docs/, entorno/, modelo/, evidencias/).
3. Realiza al menos **cinco commits descriptivos**, por ejemplo:
   - `git commit -m "Agrega README con datos del alumno e índice"`
   - `git commit -m "Agrega .gitignore para el proyecto"`
   - `git commit -m "Agrega compose.yaml con servicio de PostgreSQL"`
   - `git commit -m "Agrega investigación de la Unidad Temática I"`
   - `git commit -m "Agrega estado del arte y caso de estudio"`
4. Crea una rama (`git checkout -b feature/ajuste-readme`), haz un cambio pequeño (por ejemplo, corrige el nombre de tu grupo en el README), súbela (`git push -u origin feature/ajuste-readme`) y abre un Pull Request en GitHub describiéndolo. Fusiónalo desde la interfaz.
5. Toma las capturas de evidencia solicitadas.

**Evidencia a incluir en `evidencias/git/`:** URL del repositorio, captura de `git log --oneline --graph --all`, captura del Pull Request ya fusionado.

---

## Ejercicio 2. El sistema gestor en un contenedor: Docker

### Parte A. Investigación

**1. Contenedor vs. máquina virtual**

Una máquina virtual (VM) virtualiza hardware completo: incluye su propio sistema operativo (kernel independiente), lo que implica un arranque de decenas de segundos a minutos, un tamaño de varios gigabytes y un aislamiento fuerte a nivel de hardware, ya que corre sobre un hipervisor. Un contenedor, en cambio, virtualiza a nivel de sistema operativo: comparte el kernel del sistema anfitrión y solo empaqueta el software y sus dependencias. Esto hace que arranque en segundos (o menos), pese apenas decenas o cientos de megabytes, y su aislamiento sea más ligero (a nivel de procesos, mediante namespaces y cgroups en Linux), aunque suficiente para la mayoría de los casos de desarrollo.

**2. Definiciones**

- **Imagen:** plantilla inmutable, de solo lectura, que contiene el sistema de archivos y la configuración necesarios para ejecutar una aplicación (por ejemplo, `postgres:16`).
- **Contenedor:** una instancia en ejecución de una imagen; es el proceso vivo con su propio sistema de archivos en capas sobre la imagen.
- **Volumen:** un mecanismo de almacenamiento gestionado por Docker que persiste datos fuera del ciclo de vida del contenedor, de modo que sobreviven aunque el contenedor se elimine.
- **Puerto publicado:** el mapeo de un puerto interno del contenedor a un puerto de la máquina anfitriona (por ejemplo, `-p 5432:5432`), que permite que aplicaciones externas se conecten al servicio dentro del contenedor.

**3. Por qué es indispensable el volumen**

El sistema de archivos de un contenedor es efímero: vive solo mientras el contenedor exista. Un sistema gestor de bases de datos guarda su información en archivos dentro de ese sistema de archivos (en PostgreSQL, típicamente en `/var/lib/postgresql/data`). Si no se declara un volumen, al eliminar el contenedor (`docker rm`) se elimina también esa carpeta y, con ella, todos los datos almacenados de forma irrecuperable. Declarar un volumen (`-v pgdata:/var/lib/postgresql/data`) desacopla los datos del ciclo de vida del contenedor: el contenedor puede destruirse y recrearse cuantas veces sea necesario y, mientras el volumen no se borre explícitamente, la base de datos persiste.

### Parte B. Práctica

> Pasos a realizar en tu computadora (no pueden ejecutarse por ti):

1. Instala Docker Desktop (o Podman).
2. Usa el archivo [`entorno/compose.yaml`](entorno/compose.yaml) incluido en este repositorio: `docker compose -f entorno/compose.yaml up -d`.
3. Conéctate con `psql`, DBeaver o pgAdmin al puerto 5432 y crea una base de datos vacía llamada `practica1`.
4. Demuestra la persistencia:
   ```
   docker compose -f entorno/compose.yaml down
   docker compose -f entorno/compose.yaml up -d
   ```
   y verifica que la base `practica1` sigue existiendo.
5. Toma las capturas de evidencia solicitadas.

**Evidencia a incluir en `evidencias/docker/`:** captura de la conexión al gestor, captura de la prueba de persistencia.
