

                                         <------------ Control por git ------------------->
    WORKING DIRECTORY   <------------------->  STAGING AREA               REPOSITORIO
    mi_proyecto/                                mi_proyecto/               rama1-> 
    Archivo1.txt (v4)                           Archivo1.txt (v4)          C1(Archivo1.txt (v1))
    Archivo2.txt (v1)                           Archivo2.txt (v1)          C2(Archivo2.txt (v1) 
                                                                               + Archivo1.txt (v1))
                                                                           C3(Archivo2.txt (v1) 
                                                                               + Archivo1.txt (v2))
                                                                           C4(Archivo2.txt (v1) 
                                                                               + Archivo1.txt (v3)) 
                                                                           C5(Archivo2.txt (v1)
                                                                                 + Archivo1.txt (v4))
$ git init              # Inicializa un repositorio

Os puede dar avisos:
  user.name
  user.email

Git tiene sus configuraciones.
En cada máquina donde use git debo establecer esas configuraciones.
No obstante, las configuraciones las puedo establecer a nivel de la máquina (para mi usuario) o a nivel de cada proyecto que voy a controlar con git.

$ git config -l         # Lista las configuraciones de git

Hay 2 configuraciones imprescindibles, sin las que git no va a crear commits.
Asociado a un commit, git guarda quién es el autor del commit: nombre y email.

Para establecer configuraciones:

$ git config [--global] PROP VALUE
    ---global lo que hace es guardar la configuración a nivel de la máquina... para el usuario.
              y aplicarían a todos los proyecto... a no ser que 

$ git config user.name "VUESTRO NOMBRE"
$ git config user.email "VUESTRO EMAIL"

En el repositorio, los commits se guardan dentro de ramas.

Para ver las ramas que hay creadas en el repo, y ver en la que estoy ubicado, puedo ejecutar:
$ git branch
Realmente git no crea una rama hasta que no se hace el primer commit en esa rama.

Aunque... cuando creo un proyecto desde cero... git me posiciona en una "rama" que es la rama que creará cuando hagamos el primer commit... aunque realmente esa rama aún no existe.

# Me permite ver el estado de mi proyecto:

- Compara lo que tengo en el Working Directory con lo que tengo en el Staging Area
- Y por otro lado, compara lo que tengo en el Staging Area con lo que tengo en el último commit de la rama en la que estoy ubicado.
Además de eso... me da algunas otras informaciones, como por ejemplo, la rama en la que estoy ubicado... y en la que se hará el próximo commit.

$ git status

# Creación de nuevas ramas

$ git branch NOMBRE_RAMA        Esto crea una rama
$ git switch -c NOMBRE_RAMA     Esto crea una rama y se cambia a ella
       ^^^^
$ git checkout -b NOMBRE_RAMA   Esto crea una rama y se cambia a ella (OBSOLETO)


# Copiar archivos del Working Directory al Staging Area

$ git add PATHSPEC
            nombre de archivo       Copia ese archivo al Staging Area
            *.txt                   Copia todos los archivos con extensión .txt al Staging Area
            *                       Copia todos los archivos al Staging Area de la carpeta en la
                                    que estoy... y subcarpetas SIEMPRE Y CUANDO no sean archivo OCULTOS
            .                       Copia todos los archivos al Staging Area de la carpeta en la
                                    que estoy... y subcarpetas INDEPENDIENTEMENTE de si son OCULTOS o no
            :/                      Copia todos los archivos al Staging Area INDEPENDIENTEMENTE de si son
                                    OCULTOS o no... de la carpeta raíz del proyecto y sus subcarpetas
                                    

# Creación de commits:

Se mete dentro de un commit (nos lo imaginamos como un archivo comprimido zip, todo los que hay en el Staging Area)

$ git commit -m "MENSAJE DEL COMMIT"

Cuando se creo el commit, nos ofrece un dato:           8ab2045
[desa (commit-raíz) 8ab2045] Primer commit

Ese dato es parte (la primera parte... los primeros 7 caracteres) del id del commit.

# Ver el historial de commits

$ git log
commit 8ab204555be91ee3ce923eb4c2edc1c5e5bdfc2a (HEAD -> desa)

$ git log --oneline

# Para eliminar un archivo que tengo en área de staging:

$ git rm      --cached PATHSPEC
    Borrar el archivo del área de staging.

$ git restore --staged PATHSPEC
    Volver a poner en el area de staging lo que tuviera en el último commit de mi rama

# Diferencias entre archivos

$ git diff -- Archivo1.txt
    Muestra las diferencias entre el archivo que tengo en el Working Directory y el que tengo en el Staging Area
    Git controla cambios a nivel de linea de texto. En v3, la linea 3 no tenía un salto de linea al final
    En la  v4, la linea 3 tiene un salto de linea al final... Para git esa linea ha cambiado.
    De hecho lo que me dice es que la linea que contenía "Linea3" se ha eliminado ... y en su lugar hay una que contiene "Linea3\n"... y además una linea que contiene "Linea4"

$ git diff cb05b5f -- Archivo1.txt
    Muestra las diferencias entre el archivo que tengo en el Working Directory y el que tengo en el commit cb05b5f

$ git diff --staged -- Archivo1.txt
    Muestra las diferencias entre el archivo que tengo en el Staging Area y el que tengo en el último commit de la rama en la que estoy

$ git diff 1dd3b2e..8ab2045 -- Archivo1.txt
    Muestra las diferencias entre el archivo que tengo en el commit 1dd3b2e y el que tengo en el commit 8ab2045

> Quiero comparar la version que tengo en WD con el commit1
$ git diff 8ab2045 -- Archivo1.txt

Es potente esto? HIPERPOTENTE
Es cómodo estar pasando los IDS de los commits? NO
Es un follón tener que estar buscándolos primero.

# Muchas veces en git, necesitamos referirnos a un commit concreto... y no queremos estar buscando el id del commit.

Y ahí sale la variable HEAD.

HEAD es una variable, que por defecto apunta al último commit en que estoy ubicado (el que veo en mi Working Directory).
cualquier commit que hiciera ahora, obtendría el HEAD... y el commit que tiene actualmente el HEAD, se convertiría en el padre del nuevo commit.

HEAD da bastante juego.
HEAD^ nos permite referirnos al commit padre del commit al que apunta HEAD.
HEAD^^ nos permite referirnos al commit abuelo del commit al que apunta HEAD.
HEAD^^^ nos permite referirnos al commit bisabuelo del commit al que apunta HEAD.
HEAD^^^^ nos permite referirnos al commit tatarabuelo del commit al que apunta HEAD.

HEAD~ es lo mismo que HEAD^
HEAD~2 es lo mismo que HEAD^^
HEAD~3 es lo mismo que HEAD^^^
HEAD~4 es lo mismo que HEAD^^^^

# Viajar en el tiempo

$ git checkout ID_COMMIT
    Me permite viajar en el tiempo y ver el proyecto tal y como estaba en el commit que le indico.

Git chechout, sobre un commit pasado, nos deja el repo en estado DETACHED HEAD.
Cabeza desasociada.... eso significa que no podemos hacer un commit en esta rama después de el commit en el que estamos ubicados (HEAD actual), ya que YA HAY un commit después de este commit en el que estamos ubicados.
Si que podría abrir una rama nueva desde este commit y hacer commits en esa rama.


# Git checkout es un comando de git que permite hace tropecientas cosas diferentes entre si... muy diferentes:
- Cambiar el HEAD a otra ubicación.
    > git checkout ID_COMMIT
- Restaurar archivos 
    > git checkout -- Archivo1.txt ... restaura el archivo Archivo1.txt al estado que tenía en el último commit de la rama en la que estoy
    Hoy en día tengo un comando :
    > git restore --source HEAD --staged --worktree Archivo1.txt
- Cambiar de rama
    > git checkout NOMBRE_RAMA
    Hoy en día tengo un comando:
    > git switch NOMBRE_RAMA
- Crear ramas
    > git checkout -b NOMBRE_RAMA
    Hoy en día tengo un comando:
    > git switch -c NOMBRE_RAMA

---

git init
git config user.name "VUESTRO NOMBRE"
git config user.email "VUESTRO EMAIL"
git status
git branch
git add PATH
git commit -m "MENSAJE DEL COMMIT"
git log --oneline --all
git diff -- Archivo1.txt
git diff cb05b5f -- Archivo1.txt
git diff --staged -- Archivo1.txt
git diff 1dd3b2e..8ab2045 -- Archivo1.txt
git rm --cached PATH
git restore --staged PATH
git switch -c NOMBRE_RAMA
git switch NOMBRE_RAMA
git checkout ID_COMMIT

---
resta                                   C6 -> C8 -> C10
                                       /         /     \
desa -- C1 -> C2 -> C3 -> C4 -> C5 -> C6 -> C9 -+-------> C10
                                       \
suma                                    C6 -> C7 -> C11 -> C12


---

# Fusiones de cambios:

En git hay un huevo de opciones de fusión de cambios:
- Merge de tipo fast-forward (es un merge que no genera nuevo commit)... Copia un commit a otra rama.
    $ git merge resta --ff-only
        Eso me garantiza que si fuese necesario un merge de tipo tradicional... que genere commit, se rechace el merge
- Merge de tipo recursive (es un merge que genera nuevo commit)
  - Hay que consolidar cambios de 2 ramas
    $ git merge desa -m "" 
- Rebase
    Es un comando muy potente, pero que usar con mucho cuidado. Nos encantan los rebases.
    Pero sólo donde puedo usarlos.
- Octopus
---

# Para que quiero ir generando COMMITS?

- Para tener un historial del proyecto
  Para que quiero tener un historial del proyecto?
  - Para volver a un punto anterior
  - Para rescatar una funcionalidad que quité
  - Para ver en que momento se incluyo un problema (git blame)
Para dar marcha atrás, o para recuperar una funcionalidad... etc, que es lo primero que necesito?
 - Haber hecho un commit con ese cambio
 - Saber cuál es ese commit! PROBLEMON !!!!!!

En mi proyecto voy a acabar con CIENTOS O MILES DE COMMITS.
Necesito poder buscar bien en mi historial de commits.
ES IMPRESCINDIBLE MANTENER UN HISTORIAL DE COMMITS LIMPIO Y ORGANIZADO.

QUIERO DECIR, si no hago esto, ni lo necesito, para que cojones uso GIT

# Sobre el historial de commits

Una cosa es lo que ha pasado con mi proyecto (HISTORIA REAL)
Otra cosa es lo que me interesa contar de mi proyecto (HISTORIA PUBLICADA)

Y esas cosas son distintas.
Pocas veces me interesa la historia REAL... lo que me interesa es tener commits limpios que me permitan activar, desactivar funcionalidades, consultar como estaba algo antes de meter una funcionalidad....

Estoy trabajando en la suma:
    Creo que lo tengo... Me voy a comer
        C1 (suma lista, a falta de pruebas)
    Vuelvo de comer: Hago las pruebas:
        Fallan las pruebas
        C2 (suma lista, ahora si que si)
    Refactorizo (facilitar la mantenibilidad del código)
        Guay
        C3 (Suma ahora si que si que si lista!)

ESTA HA SIDO LA HISTORIA REAL DE MI PROYECTO
    C1->C2->C3 Le importa a alguien esto una vez que todo ha pasado?

Los historiales de los proyectos hay que ir limpiándolos.
En ocasiones, me es más cómodo no ir ensuciándolo.. en otras no.

# git commit --amend -m "Suma funcionando"

Me permite reescribir el último commit (modificarlo)
    - Cambiar el mensaje del commit
    - Cambiar archivos del commit
Mucho cuidado... esto debería hacerlo solo si no he distribuido el commit... 
Si lo he distribuido (push) y otr@ se lo ha descargado.. y lo ha usado como base para su trabajo... Mi ram y la suya van a ser irreconciliables = PROBLEMON


# git commit -a

Me permite añadir en automático al área de staging todos los archivos que haya cambiado que YA estén siendo controlados por git.
    - No añade archivos nuevos

# He ido generando commit intermedios que para MI tenían valor mientras hacía el desarrollo. Una vez acabado YA NO APORTAN VALOR:
- Si considero que en el futuro esos commits me pueden seguir aportando valor a mi, cuando fusione con la rama desa, haré un squash de esos commits: LOS CONSOLIDO EN UNO SOLO
resta                                   C6 -> C8 -> C10
                                       /         /     \        (que incluye lo que hay en suma )
desa -- C1 -> C2 -> C3 -> C4 -> C5 -> C6 -> C9 -+-------> C10 -> C13
                                       \                     ¿/? subo cambios pero no historial
suma                                    C6 -> C7 -> C11 -> C12 

- Consolidar en mi rama todos los commits en uno solo... para eso:
  Necesito borrar COMMITS DEL HISTORIAL (del repo)

  git reset: BORRA COMMITS DEL HISTORIAL (Desde el último hacia atrás... quedándome en el que le diga) 

  git reset HEAD~3
  Este comando se puede ejecutar de 3 formas diferente (modalidades):
  - SOFT: Borra commit, no toca AS, no toca WD
  - MIXED (por defecto) Borra commit, restaura AS, no toca WD
  - HARD: Borra commit, restaura AS, restaura WD (PIERDO CODIGO): CUANDO QUIERO CARGARME COSAS QUE HE HECHO... pero CARGARMELAS !!!! YA NO VALEN PARA NADA
  Esos modos cambian las áreas que restauro (o no) con el reset.
  Cualquiera de ellos borra los commits del historial.
  El modo MIXED, además restaura el área de staging al estado en el que estaba en el commit que le indico.
  El modo HARD además restaura el Working Directory al estado en el que estaba en el commit que le indico.

  resta                                   C6 -> C8
                                         /        
  desa -- C1 -> C2 -> C3 -> C4 -> C5 -> C6 -> C9 
                                         \        
  suma                                    C6 -> C7


## rebase
Lo que hace rebase es reescribir commit para que ya incluyan a priori los cambios incluidos en otros commits

rebase es un comando que se usa para reescribir commits.
                                                                                        Crear un commit commit que incorpore el PATH de la resta: git cherrypick
                                            Editado README   Resta   Quito la resta     Anulo la anulación de la resta
  resta                                         v            v                 v            v
  desa -- C1 -> C2 -> C3 -> C4 -> C5 -> C6 ---> C9 ------> C8' ----> C7' ---> C9 -> C10 -> C11
  suma                                                                ^              ^
                                                                     Suma       Multiplicación

* 4b26c94 (HEAD -> desa, suma) Operacion suma acabada
* e0e841f (resta) Resta
* 58dfef9 Editado readme.md
* c3cdb9a Todo listo para empezar

Rebase REESCRIBE los commits, para que incorporen los cambios previos de otros commits.
Y me pasa igual que con el ammend... si he distribuido los commits... y los reescribo... PROBLEMON

Los rebase los uso en ramas privadas, que no comparto... o que comparto con un compañero... y reeescribo con mi compañero sentado al lado... distribuyo.. y mi compañero se refresca.

---
*   5de756c (suma) Fusion de cambios con desa
|\  
| *   7b3ecc3 Integración con cambios en desa
| |\  
| | * 58dfef9 (desa) Editado readme.md
| * | d846e00 (HEAD -> resta) Resta
| |/  
* / 47eeaf7 Operacion suma acabada
|/  
* c3cdb9a Todo listo para empezar

---

otros features
           C1 -> C4
          /        \
desa --> C1 ------> C4 -> C5 -> C6 -> C7 -> C8 -> C10 --------------------------> C11
                                                    \                           /
                                            feature1  C10 -> C2 -> C3 -> C9 -> C11

desa --> C1 -> C4 --------+---> C8 -> C9 ---+-> C12 -> C13 + ----------> C15
          \                \                 \              \         /
feature1   C1 -> C2 -> C3 -> C5*-> C6 -> C7 -> C10*-> C11 -> C14* -> C15

El merge tradicional está bien cuando quiero reflejar en el historial el trabajo de INTEGRACION de 2 ramas.

Estoy montando un sistema de cache:
- Unos tios montando una característica de la cache  \
- Unas tias montando otra característica de la cache / Hay que hacer más trabajo de integración


--- 
# quiero quitar la operación resta de desa... 
aunque no quiero que desaparezca del repo... 
por si la vuelven a pedir.

En este caso, lo que puedo hacer es un nuevo commit revirtiendo los cambios que se incorporaron en la resta: COMMIT DE ANULACION DE CAMBIOS

Pregunta, sabe git los cambios que se incorporaron en el commit RESTA? NO
Pero, puede calcularlos... haciendo un diff con el commit anterior: PATCH
Y quiero crear un nuevo commit que anule ese PATCH: git revert

                          resta         quito resta
                            v              v
----desa -> C1 -> C2 -----> C3 -> C4 -> C(-3) -> C5 -----> C(+3)
                                  suma              \       /  
                                                    C5 -> C(+3)

Git revert y git cherrypick los 2 calculan lo primero el patch del commit al que apunto C3:
    Git lo que hace para ello es mirar las diferencias entre el C3 y el anterior (su padre) C2
Y ahora es donde divergen esos comando:
    git revert crea un commit que quita ese patch
    git cherrypick crea un commit que incorpora ese patch

Solo puedo hacer un revert de un commit que tenga en la rama en la que estoy
En cambio puedo hacer un cherry pick de un commit de otra rama

                                CF1 -> CF2 (1.0.2)
                                /
    hotfix                C2-> CF1(1.0.1)
                         /
    master              C2(v1.0.0)   C4(v2.0.0)
                        /           /
    release           C2           C4 
                      /            /
----desa ----> C1 -> C2 -> C3 -> C4 -> C5 -> CHERRY PICK C2

# Para controlar el historial del proyecto... y que quede UTIL (no real.. sino UTIL)

- git commit --amend <- Reescribe el ultimo commit
- git reset [--soft|--mixed|--hard] HEAD~3 <- Borra los últimos commits del historial
                                              según la opción permitiéndome no borrar código
- git rebase <- Reescribe los commits de una rama para que incluyan los cambios de otros commits

Esos 3 me ayudan a ir manteniendo un historial limpio y útil... hay más:
- git rebase -i 

# Git cherry-pick y git revert

Nos ayudan a anular/aplicar paquetes de cambios (patches) de un commit en otro commit


----

---- desa-> C1 -> C2 -> C10
                            \
            ---- feature1    C10 -> C3 -> C4 -> C6  
                                \                  \ 
                ivan             C10 -> C3 -> C4 ---> C6


---

---- desa-> C1 -> C2 -> C10
                            \
            ---- feature1    C10 -> C3 -> C4 -> C6  
                                                 \
                menchu                            C6

---

Para sincronizar cambios Ivan y Menchu, van a usar un servidor de alojamiento de repos remotos:
- Github        Es una herramienta que se ofrece como servicio a través de internet
- Gitlab*       \ Son herramientas que se ofrecen como servicio a través de internet
- Bitbucket     / pero también las puedo desplegar en mi propia infraestructura

El proximo día: Necesitamos un servidor de remotos para trabajar.

---

Yo voy a tener en mi máquina mi repo, mi AS, mi WD
Menchu va a tener en su máquina su repo, su AS, su WD
En un remoto vamos a tener un repo.

El trabajo será copiar ramas del repo de Ivan o Menchu al remoto
Y posteriormente copiar ramas del repo remoto al repo de Ivan o de Manchu

   Subir una rama mia a un remoto:          git push
   Bajar una rama de un remoto a mi repo:   git fetch
    git pull = git fetch + [git merge | git rebase | git merge --ff-only] 
                    en función de mi configuración de git en mi máquina