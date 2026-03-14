# Guía Completa de Git y GitHub CLI

Documento de referencia para trabajar con control de versiones usando
**Git** y administrar repositorios en **GitHub** usando **GitHub CLI
(gh)**.

Incluye:

-   Conceptos fundamentales
-   Explicación detallada de comandos
-   Flujos de trabajo recomendados
-   Resolución de errores comunes
-   Uso completo de GitHub CLI

------------------------------------------------------------------------

# 1. Qué es Git

Git es un sistema de control de versiones distribuido que permite:

-   Registrar cambios en archivos
-   Volver a versiones anteriores
-   Trabajar con ramas paralelas
-   Colaborar con otros desarrolladores

Cada copia del repositorio contiene todo el historial.

------------------------------------------------------------------------

# 2. Instalación

Linux:

``` bash
sudo apt install git
```

Verificar instalación:

``` bash
git --version
```

------------------------------------------------------------------------

# 3. Configuración inicial

Git necesita identificar al autor de los commits.

``` bash
git config --global user.name "TuNombre"
git config --global user.email "tu@email.com"
```

Explicación:

-   `user.name`: nombre del autor
-   `user.email`: correo asociado al commit

Ver configuración:

``` bash
git config --list
```

Editar configuración:

``` bash
git config --global --edit
```

------------------------------------------------------------------------

# 4. Crear repositorios

Inicializar repositorio:

``` bash
git init
```

Esto crea la carpeta `.git` que contiene todo el historial.

Clonar repositorio:

``` bash
git clone URL
```

Ejemplo:

``` bash
git clone https://github.com/usuario/proyecto.git
```

------------------------------------------------------------------------

# 5. Estado del repositorio

``` bash
git status
```

Muestra:

-   archivos modificados
-   archivos sin seguimiento
-   archivos preparados para commit

------------------------------------------------------------------------

# 6. Área de trabajo de Git

Git tiene tres áreas:

1.  Working Directory (archivos editados)
2.  Staging Area (archivos preparados)
3.  Repository (historial de commits)

Flujo básico:

    editar archivos
    git add
    git commit

------------------------------------------------------------------------

# 7. Agregar archivos

Agregar archivo:

``` bash
git add archivo.txt
```

Agregar todos los archivos:

``` bash
git add .
```

Agregar archivos modificados:

``` bash
git add -u
```

------------------------------------------------------------------------

# 8. Commits

Crear commit:

``` bash
git commit -m "mensaje"
```

Un commit guarda un punto en la historia.

Buenas prácticas:

-   mensajes claros
-   cambios pequeños

Ejemplo:

    feat: agregar sistema de login
    fix: corregir validación de contraseña

------------------------------------------------------------------------

# 9. Historial

Ver historial:

``` bash
git log
```

Formato compacto:

``` bash
git log --oneline
```

Historial visual:

``` bash
git log --graph --oneline --all
```

------------------------------------------------------------------------

# 10. Ramas

Las ramas permiten trabajar en paralelo.

Ver rama actual:

``` bash
git branch
```

La rama actual aparece con `*`.

Crear rama:

``` bash
git branch nueva-rama
```

Cambiar rama:

``` bash
git switch nueva-rama
```

Crear y cambiar:

``` bash
git switch -c nueva-rama
```

Eliminar rama:

``` bash
git branch -d rama
```

Forzar eliminación:

``` bash
git branch -D rama
```

------------------------------------------------------------------------

# 11. Merge

Fusionar ramas.

Ejemplo:

``` bash
git switch main
git merge feature-login
```

Esto integra los cambios de `feature-login` en `main`.

------------------------------------------------------------------------

# 12. Repositorios remotos

Ver remotos:

``` bash
git remote -v
```

Agregar remoto:

``` bash
git remote add origin URL
```

Cambiar URL:

``` bash
git remote set-url origin URL
```

------------------------------------------------------------------------

# 13. Subir cambios

``` bash
git push
```

Primera vez:

``` bash
git push -u origin main
```

Subir rama:

``` bash
git push origin rama
```

------------------------------------------------------------------------

# 14. Descargar cambios

Actualizar repositorio:

``` bash
git pull
```

Solo descargar:

``` bash
git fetch
```

Diferencia:

-   `fetch` descarga
-   `pull` descarga y fusiona

------------------------------------------------------------------------

# 15. Comparar cambios

``` bash
git diff
```

Cambios en staging:

``` bash
git diff --staged
```

Comparar commits:

``` bash
git diff commit1 commit2
```

------------------------------------------------------------------------

# 16. Deshacer cambios

Descartar cambios:

``` bash
git restore archivo
```

Quitar del staging:

``` bash
git restore --staged archivo
```

Volver a commit anterior:

``` bash
git reset --hard HEAD
```

------------------------------------------------------------------------

# 17. Stash

Guardar cambios temporales:

``` bash
git stash
```

Listar:

``` bash
git stash list
```

Restaurar:

``` bash
git stash pop
```

------------------------------------------------------------------------

# 18. Tags

Los tags marcan versiones.

Crear tag:

``` bash
git tag v1.0
```

Subir tag:

``` bash
git push origin v1.0
```

------------------------------------------------------------------------

# 19. Archivo .gitignore

Permite ignorar archivos.

Ejemplo:

    node_modules/
    .env
    dist/
    *.log

------------------------------------------------------------------------

# 20. Flujo de trabajo recomendado

Flujo común:

1 Crear rama

    git switch -c feature-nueva

2 Realizar cambios

3 Agregar cambios

    git add .

4 Commit

    git commit -m "feat: nueva funcionalidad"

5 Subir rama

    git push origin feature-nueva

6 Crear Pull Request

------------------------------------------------------------------------

# 21. Git Flow simplificado

Ramas principales:

-   main (producción)
-   develop (desarrollo)

Ramas temporales:

-   feature/\*
-   hotfix/\*
-   release/\*

Ejemplo:

    git switch develop
    git switch -c feature-login

------------------------------------------------------------------------

# 22. Errores comunes y soluciones

Repositorio remoto ya existe

    git remote remove origin
    git remote add origin URL

Error de merge

Resolver conflicto manualmente y luego:

    git add .
    git commit

Revertir commit:

    git revert ID

------------------------------------------------------------------------

# 23. GitHub CLI

Instalar:

Linux:

    sudo apt install gh

Ver versión:

    gh --version

------------------------------------------------------------------------

# 24. Autenticación

Login:

    gh auth login

Ver estado:

    gh auth status

Logout:

    gh auth logout

------------------------------------------------------------------------

# 25. Crear repositorios

Crear repo:

    gh repo create

Crear repo desde proyecto actual:

    gh repo create nombre --source=. --push

Clonar repo:

    gh repo clone usuario/repositorio

------------------------------------------------------------------------

# 26. Pull Requests

Crear PR:

    gh pr create

Listar PR:

    gh pr list

Ver PR:

    gh pr view

Merge PR:

    gh pr merge

------------------------------------------------------------------------

# 27. Issues

Crear issue:

    gh issue create

Listar:

    gh issue list

Cerrar:

    gh issue close numero

------------------------------------------------------------------------

# 28. Releases

Crear release:

    gh release create v1.0

Listar releases:

    gh release list

------------------------------------------------------------------------

# 29. Buenas prácticas

-   Commits pequeños
-   Ramas por funcionalidad
-   No trabajar directamente en main
-   Revisar cambios antes de commit
-   Usar Pull Requests

------------------------------------------------------------------------

# 30. Comandos más usados

    git status
    git add .
    git commit -m "mensaje"
    git push
    git pull
    git branch
    git switch rama
    git merge rama
    git log --oneline
