# Guía de estilo para Git

Índice
1. [Introducción](#introducción)
1. [Ramas](#ramas)
2. [Commits](#commits)
   1. [Mensajes](#mensajes)
3. [Fusiones](#fusiones)
4. [Otros](#otros)

## Introducción

Esta guía de estilos es la que usaré en mis repositorios
de ahora en adelante. Fue creada como una propuesta para el
@LIDSOL. Esta basada en la
[*Udacity Git Commit Message Style Guide*](http://udacity.github.io/git-styleguide/)
y la [*Git Style Guide*](https://github.com/jeko2000/git-style-guide/).

## Ramas

* Usa nombres *cortos* y *descriptivos* para las ramas (*branches*):
  ```shell
  # correcto
  $ git checkout -b migracion-oauth

  # incorrecto - muy ambiguo
  $ git checkout -b arreglo_login
  ```

* También puedes usar identificadores correspondientes a problemas,
  tickets, o incidencias de un servicio externo (como Gitlab) como
  nombres para las ramas:

  ```shell
  # Gitlab issue #15
  $ git checkout -b issue-15
  ```

* Usa guiones para separar palabras

* Cuando varias personas trabajen en la misma parte o
  característica de un proyecto, es conveniente tener una rama para
  todo el equipo y otras ramas personales para cada persona:

  ```shell
  $ git checkout -b feature-a/master # Branch del equipo
  $ git checkout -b feature-a/maria  # Branch personal de María
  $ git checkout -b feature-a/nick   # Branch personal de Nick
  ```

  Se pueden hacer fusiones desde las ramas personales a la rama del
  equipo (ver ["Fusiones"](#fusiones)). Eventualmente, la rama del
  equipo será integrada a *master*.

* Elimina tu rama del repositorio remoto después de que haya sido
  fusionada en *master* a menos que haya una razón para conservarla.

Consejo: Usa el comando a continuación desde *master* para mostrar las
ramas que han sido fusionadas:

```shell
$ git branch --merged | grep -v "\*"
```

# Commits

* Un *commit* debe de ser un solo *cambio lógico*. No realices varios
  *cambios lógicos* en un *commit*. Por ejemplo, si un cambio arregla
  un *bug* y otro mejora el desempeño de una rutina, sepáralos en dos
  *commits* diferentes.
  * Consejo: Usa `git add-p` para seleccionar de forma interactiva partes
    específicas de un archivo para incluir en un *commit*.

* No separes un solo cambio lógico en varios *commits*. Por ejemplo,
  la implementación de una nueva rutina y sus pruebas deben ir
  en el mismo *commit*.

* Realiza *commits* **pronto** y **frecuentemente**. Los *commits*
  pequeños y autónomos son más fáciles de revertir si algo sale mal.

* Los *commits* deben de ser ordenados *lógicamente*. Por ejemplo,
  si el *commit A* depende de los cambios hechos en el *commit B*,
  entonces, el *commit B* debe de realizarse antes del *commit A*.

### Mensajes

* Un mensaje de *commit* consiste en tres partes distintas **separadas
  por una línea en blanco**: la línea de resumen o título, un cuerpo
  opcional y un pie de mensaje opcional:

  ```
  tipo(contexto): título

  cuerpo

  pie
  ```

#### Tipo

El tipo está contenido dentro del título y puede ser uno de los
siguientes:

* **add:** añade una nueva característica
* **rm:** elimina una característica
* **fix:** la corrección de un error
* **doc:** cambios en la documentación
* **style:** cambios en el formato del código, puntos y comas faltantes,
  correcciones ortográficas, etc. El código de programación no cambia.
* **refac:** modificaciones al código que no modifican su comportamiento,
  pero que mejoran su tiempo de ejecución, uso de memoria, claridad, etc.
* **test:** se modifican o extienden las pruebas unitarias del código,
  pero no el código mismo.
* **cfg:** se modifican archivos de configuración, construcción,
  del gestor de paquetes, de git (p.ej. .gitignore), etc. No se modifica
  el código.

Nota: Si un cambio cae dentro de dos de estos aspectos probablemente
deba ser separado en varios *commits*, siendo la excepción el caso de
nuevas rutinas y sus pruebas unitarias, o modificaciones al código o
a archivos de configuración que incluyen documentación en comentarios.

#### Contexto (opcional)

Si el proyecto es lo bastante complejo, como para que no sea obvio,
puedes usar una palabra clave que deje inmediatamente claro en que parte
del proyecto ocurrió el cambio.

#### Título

* El título o línea de resumen, es decir, la primera línea del mensaje de
  *commit*, debe ser *descriptiva* y *concisa* al mismo tiempo.

* Idealmente no debe tener más de 50 caracteres, y jamás debe tener más
  de 72. Debe comenzar con mayúsculas y no debe terminar con un punto,
  puesto que sirve como el *título* del *commit*.

* Utiliza un tono imperativo para describir lo que hace el *commit*, en
  lugar de lo que hizo. Formula la oración como si empezara con "Este
  commit". Por ejemplo, utiliza "cambia"; no "se cambió" o "se cambia":

  ```
  # correcto - en imperativo, presente, con mayúscula inicial y ligeramente
  # mayor a 50 caracteres y menor a 72 caracteres
  fix: Marca registros como obsoletos al limpiar los errores

  # incorrecto
  Se corrigen los mensajes de obsoleto de ActiveModel::Errors.
  ```

#### Cuerpo

* **Después del título debe ir una línea en blanco** seguida por una
  descripción más a fondo del commit.

* Cada línea debe de tener 72 caracteres o menos. Y nuevos párrafos
  deben separarse por una línea en blanco.

* Debe de explicar la *razón* por la cual el cambio era necesario,
  *consideraciones* para resolver el problema, y posibles *efectos
  secundarios* que el cambio pudiera tener. En resumen el cuerpo debe
  explicar el qué y el por qué, no el cómo. Mientras escribes el
  cuerpo piensa en la información que necesite saber alguien ajeno al
  proyecto o tú dentro de un año para entender el contexto del cambio.

* Si un *commit A* depende de *commit B*, la dependencia puede de ser
  escrita en el mensaje de *commit A* refiriéndote a *commit B* por su
  respectivo hash.

* Similarmente, si un *commit A* corrige un problema introducido por
  *commit B*, éste también debe ser referido en el mensaje de *commit
  A*.

* Si un commit ha de entrar al proceso de *squash* con respecto a otro,
  debe ser escrito como tal para hacer la intención clara:

  ```shell
  $ git commit --squash f387cab2
  ```

  * Consejo: Usa `--autosquash` al realizar un *rebase*. Los *commits*
  marcados entraran al proceso de *squash* automáticamente.

* No todos los *commits* son lo suficientemente complejos como para
  justificar un cuerpo, por lo tanto es opcional y sólo se usa cuando
  se requiere un poco de explicación y contexto del por qué es necesario
  el cambio.

#### El pie de mensaje

* El pie de página es opcional y se utiliza para hacer referencia a los
  identificadores del rastreador de incidencias, o alguna fuente relevante
  de la cuál tomaste código o inspiración para un cambio.

#### Ejemplo completo

```
ft: Resumir los cambios en alrededor de 50 caracteres o menos

Texto explicativo más detallado, si es necesario. Limitado a
aproximadamente 72 caracteres más o menos. En algunos contextos,
la primera línea es tratada como el título de la confirmación y el
resto del texto como cuerpo. La línea en blanco que separa el título
del cuerpo es importante (a menos que omitas el cuerpo por completo);
varias herramientas como `log`, `shortlog`, y `rebase' puede confundirse
si no se separan los parrafos.

Explique el problema que este commit está resolviendo. **Concéntrate
en por qué están haciendo este cambio en vez de cómo.**

¿Existen efectos secundarios u otras consecuencias no intuitivas de
este cambio? Este es el lugar para explicarlos.

Los párrafos siguientes vienen después de las líneas en blanco.

- Tambien puedes usar sintaxis de markdown en el cuerpo del commit
- Si utiliza un rastreador de problemas, ponga referencias a ellos en
  la parte inferior.

Resuelve: #123
Ver también: #456, #789

fuente: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
```

## Fusiones

* No reescribas las historia del repositorio remoto. El historial de un
  repositorio es muy valioso para saber qué ocurrió exactamente en un
  momento dado. Alteraciones al historial es una fuente común de errores.

* Sin embargo existen un par de excepciones en las que es válido
  reescribir la historia de cambios:

  * Tú eres el único trabajando en la rama y la rama no será
    revisada por otra persona.
  * Quieres reorganizar tu ramas (p. ej. quieres hacer *squash* de varios
    *commits*) y/o vas a hacer un rebase de tu rama con respecto
    a *master*.

  Dicho esto, *nunca reescribas la historia de la rama master* o de
  cualquier otra rama especial como aquellas usadas por equipos, en
  producción o en los servidores.

* Mantén el historial *limpio* y *simple*. Antes de solicitar una fusión
  (*merge request*) en *master*:

  1. Asegúrate que cumple con esta guía de estilo.
  2. Haz un *rebase* con respecto a la rama que va a recibir la fusión
     de cambios:

     ```shell
     [mi-rama] $ git fetch
     [mi-rama] $ git rebase origin/master
     # Ahora haz el merge
     ```

     Esto resulta en una rama que puede ser directamente aplicada al
     final de la rama *master* lo cual simplifica el historial del
     proyecto.

     * Nota: Esta estrategia es más adecuada para proyectos con
     ramas de corta duración. De otra forma, es posible que sea
     mejor ocasionalmente hacer un merge en vez de un rebase con la
     rama *master*

* Si tu rama incluye más de un commit, no hagas *merge* usando
  la estrategia *fast-forward*:

  ```shell
  # correcto - se asegura que un commit del merge sea creado
  $ git merge --no-ff mi-branch

  # incorrecto
  $ git merge mi-branch
  ```

## Otros

* Existen varios estilos o flujos de trabajo y cada uno tiene
  sus ventajas y desventajas. El hecho de que un flujo encaje con un
  proyecto en específico depende del equipo, de aspectos del proyecto en
  sí y de sus procesos de desarrollo. *Lo que es importante es escoger
  un estilo y usarlo de forma permanente en el proyecto.*

* *Sé consistente.* Esto es relacionado con el flujo de trabajo,
  e incluye cosas como mensajes de commit, nombres de tus ramas y
  etiquetas (*tags*). El tener un estilo consistente en el repositorio
  facilita su entendimiento a través de `git log` y de los mensajes de
  sus *commits*.

* **Prueba tus cambios antes de hacer *push*.** No hagas *push* de un
  trabajo a medias o sin terminar. No importa que commit se elija de la
  historia, la ejecución del proyecto debe ser exitosa.

* Usa [Etiquetas Anotadas](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags)
  para marcar lanzamientos (*releases*) u otros puntos importantes
  (hitos/*milestones*) en el historial.

* Usa [Etiquetas Ligeras](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)
  para uso personal, como para marcar *commits* importantes y facilitar
  su búsqueda en el futuro.

* Mantén tus repositorios en buenas condiciones haciendo
  mantenimiento ocasionalmente:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)
