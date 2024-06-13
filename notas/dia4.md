
Para crear un proyecto nuevo, que controlemos en git, tenemos 2 opciones:
- Crear un repo en mi máquina (un desarrollador) -> lo publica en un remoto -> resto clona ese remoto
- Un desarrollador crea un remo directamente en un remoto -> él y el resto lo clona

En cualquiera de las 2 realmente hay que crear el repo en el remoto.
El tema es donde creo el primer commit del proyecto:
- Local de un desarrollador
- Remoto

----

Al trabajar con un remoto, git lo que hace es COPIAR ramas... de local a un remoto o de un remoto a local.
Para cada remoto con el que trabaje, para cada rama que quiera copiar/sincronizar con ese remoto
he de crear en local una rama de vinculación con remoto: una rama upstream. Es un tipo especial de rama, que se usa para las sincronizaciones.

desarrollador1
    REPO
        desa -> C1
           ^v
        miremoto/desa -> C1          (*)
        miremotogitlab/desa     (**)
github
    REPO <- miremoto
       desa -> C1                    (*)
gitlab
    REPO <- miremotogitlab
       desa                     (**)


    LOCAL                     REMOTO
    ----------------------   --------------
    desa -> miremoto/desa ->  desa
         <-               <-





----

## GITIGNORE

.gitignore
    **/*.tmp
    /tmp
    /ruta/archivo17

---

git clone <URL>
- git init en local
- git remote add origin <URL>
- remoto(desa) -> local(origin/desa) 
- git switch -c desa origin/desa


---
                                                        DIVISION
desarrollador1  
    division                                      C3' -> C5
                                                 /        \
    desa           -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7'
                                                                                
    miremoto/desa  -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7'
remoto
    desa           -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7'
desarrollador2
    origin/desa    -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7'

    desa           -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7'
                                                                        \        
                            multiplicacion                                C4'

                    ALTA          SUMA        RESTA     MULTIPLICACION


---

                                                        DIVISION
desarrollador1  
    division                                      C3' -> C5
                                                 /        \
    desa           -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7 -> C8'* -> C9'* -> C10'* -> C11'*
                                                                                
    miremoto/desa  -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7
remoto
    desa           -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7
desarrollador2
    origin/desa    -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7

    desa           -> C1 --------> C2 -------> C3' --------> C5 -----> C4' -> C7
                                                                        \        
                            multiplicacion                                C4'

                    ALTA          SUMA        RESTA     MULTIPLICACION
