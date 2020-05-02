# Guía rápida Git

1.	Git
2.	Instalación y configuración de Git
3.	Comandos básicos
   3. Git log avanzado
4.	Administrando proyectos Git
5.	Ramas en Git
6.	Comandos avanzados
   6. Como crear alias en Git
   6. Descartar temporalmente cambios
   6. ‘Pull’ de un solo ‘commit’
7.	Github
8.	Conclusiones
9.	Agradecimientos

## ¿Qué es Git?
Git es una herramienta que sirve para gestionar el control de versiones. Pero ¿qué es el control de versiones? Pues es simplemente una forma de tener controlados todos los cambios que realizamos sobre cualquier tipo de archivos. Este control se puede efectuar a mano, aunque es recomendable usar una herramienta que nos facilite la vida como por ejemplo ‘git’, aunque hay muchas más.

## Instalación y configuración de Git.
Lo primero es tener instalado ‘git’ en nuestra máquina para tener acceso a todos sus comandos. Para descargar e instalar ‘git’ lo puedes hacer desde su página oficial.

https://git-scm.com/downloads

Una vez instalado ‘git’, puedes configurar tu identidad ejecutando en la terminal:

```
git config --global user.name "Tu nombre aquí"
git config --global user.email ejemplo@example.com
```

### Comandos básicos
Bien, pongámonos en la situación de que tenemos un proyecto que estamos desarrollando y queremos cambiar algo drástico. Lo mejor es usar ‘git’ para llevar el control de estos cambios por si queremos revertirlos.

El primer paso es abrir la terminal de ‘git’, o la terminal de nuestro sistema, para dirigirnos a la carpeta donde tengamos guardado el proyecto que estemos desarrollando, y ejecutar el siguiente comando:

```git init```

Con este comando de ‘git’ lo que estamos haciendo es decirle a ‘git’ que este pendiente de los cambios que se produzcan en los archivos de ese directorio.

Este comando solo lo tenemos que ejecutar una sola vez para cada proyecto que estemos realizando.

Ahora podemos continuar desarrollando nuestro proyecto y, cuando queramos guardar los cambios con ‘git’ tenemos que hacer lo siguiente:

```git status```
 
Este comando ‘git’ imprimirá el estado de los archivos, tanto los que han sido modificados como los que han sido agregados al ‘staging area’. El siguiente paso es ejecutar:

```git add .```

Con esto añadiremos al ‘staging area’ todos los archivos que aparecían anteriormente como modificados para ser guardados. Si queremos añadir un archivo o carpeta en concreto lo podemos hacer mediante:

```git add NOMBREDELARCHIVO```

Si queremos eliminar los archivos que acabamos de añadir para ser guardados lo podemos hacer con:

```
git rm .
git rm NOMBREDELARCHIVO
```

Si finalmente queremos desahcer los cambios realizados en un archivo y devolverlo a su estado original podremos utilizar el comando:

```git checkout <FILENAME>```

Con estos comandos hemos añadido o hemos quitado archivos al ‘staging área’ pero aún no han sido guardados, para ello:

```git commit -m "Nombre descriptivo del cambio que hemos realizado"```

Acabamos de hacer nuestro primer ‘commit’. Un ‘commit’ es un guardado con mensaje de los cambios que hemos realizado en un momento determinado en nuestro proyecto. Normalmente el flujo de trabajo con ‘git’ para el día a día consiste en hacer:

```
git status 	(para ver la lista de cambios)
git add 	(de todos los archivos o de los que queramos, y por último hacer)
git commit 	(para guardar los cambios).
```

Para imprimir todos los ‘commits’ que hemos realizado tenemos el comando:

```
git log
git log -n 1 (muestra solo una línea de log)
```

La cadena de números y letras (hash) que aparece al lado de la palabra “commit” es el identificador que podemos usar para revertir los cambios y volver atrás a ese punto. Para ello ejecutamos:

```git reset --hard <commitSHA> (normalmente los cuatro primeros caracteres)```

Con ‘git revert’ se crea un nuevo ‘commit’ que revierte los cambios realizados en el último ‘commit’, pero no elimina dicho ‘commit’.
Si quieres ver los cambios que has realizado desde el último ‘commit’ lo puedes hacer con:

```
git diff (compara ficheros entre ‘working directory’ y ‘staging area’)
git diff <filename> (muestra las modificaciones a un fichero antes de añadir al ‘staging area’)
git diff –staged (compara ficheros entre ‘staging area’ y el ultimo ‘commit’)
git diff <commitSHA> <commitSHA> (compara ficheros entre diferentes ‘commits’)
```

### Git log avanzado
Hay veces en las que el comando ‘git log’ ofrece demasiada información, pero esto se puede personalizar. Por ejemplo:

```git log --online```

Se imprimirá en cada linea un commit, con su identificador y el texto del ‘commit’.

Otro parámetro bastante útil del log es el de ‘git graph’.

```git log --graph --oneline```
 
Esto imprimirá la lista de ‘commits’ y mediante caracteres ASCII, representará el árbol con las ramas y los cambios entre ellas.
También podemos filtrar los ‘commits’, por ejemplo:

```
git log --author="John"
git log --after="2014-7-1"
git log -- foo.py bar.py
```

Estos comandos filtrarán los ‘commits’ por autor, por fecha y por los archivos que fueron modificados respectivamente.

## Administrando proyectos git
Ahora, imaginemos que queremos administrar un proyecto ubicado en otro servidor o en un almacén de repositorios como es Github
Si queremos bajarnos el proyecto para empezar a gestionarlo lo podemos hacer usando:

```git clone https://servidor/ruta/a/los/archivos```

Por ejemplo, para GitHub:

```git clone https://github.com/proyectogithub```

Posteriormente tendremos que conectarnos a ese repositorio remoto, para ello haremos:

```git remote add origin https://servidor/ruta/a/los/archivos```

Donde la palabra ‘origin’, o la que nosotros designemos, hará referencia al repositorio remoto.

Para conocer saber si estamos conectados a algún repositorio y su designación, simplemente tecleamos:

```git remote```

Si alguien (un colaborador) hiciera un cambio desde otro sitio a este mismo repositorio, y quisiéramos actualizar el repositorio a nuestra copia local, haríamos:

```git pull origin master```

Pero, si tenemos cambios sin guardar, tenemos que guardarlos haciendo ‘commit’, antes de bajarnos nada. Tras ello, podemos subirlos al repositorio remoto:

```git push origin master```

Donde ‘origin’ es la dirección del repositorio remoto y master es la rama a la que estamos subiendo los cambios.

En verde, saldrán las líneas que has añadido, y en rojo las que has eliminado desde la última vez.

Si tenemos ficheros o carpetas que no queremos que se suban al repositorio, y que están dentro de nuestra carpeta local de trabajo (por ejemplo, archivos de tokens, claves y usuarios, direcciones, etc.), podemos evitarlo con la creación de un fichero en el que se escribirán los nombres de dichas carpetas y ficheros a ignorar. El fichero se denominará:

```.gitignore```

## Ramas en git
Muy bien, pero antes has dicho que has subido los cambios a una rama, ¿qué es una rama?
Las ramas sirven para llevar un control de cambios independiente en el mismo repositorio, es decir, podemos crear una rama a partir de la rama ‘base’, o rama ‘master’, con otra serie de cambios. Normalmente las ramas en ‘git’ se utilizan para asilar funcionalidades, es decir, creamos una rama con el nombre de una funcionalidad, hacemos todos los ‘commits’ que queramos y al finalizar la funcionalidad hacemos un ‘merge’ de ramas. Esto permite tener equipos trabajando independientemente a la vez en el mismo proyecto sin pisar funcionalidades. Esta estrategia se conoce como ‘git flow’. Puedes consultar más información de esto en este enlace: http://aprendegit.com/que-es-git-flow/
Para listar las ramas existentes y saber cual es la rama activa usamos:
git branch
Si queremos crear una nueva rama podemos hacerlo de dos formas:
git branch nombre_de_la_rama (creación)
git checkout nombre_de_la_rama (cambio a esa rama)
O hacer lo mismo en un solo comando:
git checkout -b rama_nueva
Si queremos ver el árbol de ramas de una manera un tanto ‘grafica’ podemos hacerlo a través de:
git log –graph –online branch1 branch2
A la hora de fusionar los cambios, normalmente de una rama específica a la ‘master’, no situaremos en la rama master mediante ‘checkout’ y desde allí teclearemos:
git merge master rama_a_fusionar
Al estar en la rama ‘master’ podemos omitir el nombre de esa rama, escribiendo solo:
git merge rama_a_fusionar
Una vez hecho esto podemos borrar la referencia a la rama fusionada, pues ahora sus ‘commits’ son alcanzable desde la rama ‘master’. Para ello haremos:
git branch -d nombre_rama_fusionada
Cuando hemos hecho un ‘merge’ y no sabemos cual es el ‘commit’ anterior, pero queremos ver las diferencias con nuestro último ‘commit’, podemos ejecutar:
git show <commitSHA>
A la hora de fusionar ramas, suele pasar que ‘git’ no sabe que cambio se debe introducir cuando en una misma línea de código ha habido cambios en las dos ramas. Por ello ‘git’ mostrara un mensaje de conflicto. Para arreglarlo tienes que decidir que cambio quieres conservar (modificando manualmente el archivo) y volver a hacer ‘add’ y ‘commit’.
Comandos avanzados
Vistos estos comandos básicos de ‘git’ toca ver algo más complejo. No son comandos muy complicados pero no son recomendables si no controlas del todo git. Para ciertas situaciones estos comandos de ‘git’ son muy útiles.
Cómo crear alias en ‘git’
Para no tener que estar escribiendo todo el rato ‘commit’ o ‘checkout’ podemos crear alias. Los alias de ‘git’ sirven para decirle a ‘git’ que comando tiene que ejecutar para el alias que le hemos indicado. Estos son los alias más comunes, aunque puedes crear y configurar más:
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
De esta forma, al escribir ‘git st’, ‘git’ ejecutará el comando git status ahorrándonos mucho tiempo.
Descartar temporalmente cambios
Si estás trabajando en una rama y quieres cambiarte a otra, ‘git’ no te dejará porque tienes cambios sin guardar. Una forma de solucionar esto es haciendo un ‘commit’, pero si no queremos hacerlo lo que podemos hacer es descartar los cambios temporalmente. Para ello:
git stash
Posteriormente si los quieres volver a la rama y recuperar los cambios, haremos:
git stash pop
Este comando ‘git’ es de los más útiles para no tener que estar guardando cosas momentáneamente en otros sitios si te quieras cambiar rápido de rama.
‘Pull’ de un solo ‘commit’
Si por cualquier motivo, necesitas hacer un ‘pull’ pero solo de un determinado ‘commit’, lo que puedes hacer es usar este comando:
git cherry-pick <commitSHA>
Github
Existe una funcionalidad dentro de repositorio de Github que nos permite solicitar la inclusión de código en un repositorio que no es nuestro o que controla otra persona. Dicha funcionalidad se hace efectiva a través de ‘Pull Request’. Dicha funcionalidad se entiende mejor como la denominan en otros repositorios, haciéndolo como ‘Merge Request’, pues es mas bien eso, solicitar que fusionen tu código en la rama master.
Conclusiones
Git es una herramienta muy potente que ofrece muchísimos comandos para la gestión de versiones de nuestros proyectos. Los ‘commmits’ que he explicado son unos cuantos, me dejo muchos de ellos, pero te animo a que eches un vistazo a la documentación oficial de ‘git’ para que descubras muchos más comandos y configuraciones.
Agradecimientos
Este documento no es mas que una mejora de lo realizado por ‘codingpotions.com’, a quien agradezco el trabajo realizado, pues me ha ahorrado mucho trabajo de tecleo.
Este documento surgió como resultado de un curso sobre “Git” que realicé en ‘udacity.com’, tras el cual decidí hacer un resumen de los comandos aprendidos para recordar todo mejor y tener una guía rápida a la que acudir en caso de duda.
