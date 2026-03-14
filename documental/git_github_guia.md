# Guía Completa de Git y GitHub CLI

Esta guía contiene los comandos esenciales para controlar versiones,
trabajar con ramas y subir código a GitHub usando Git y GitHub CLI (gh).

------------------------------------------------------------------------

# 1. Configuración inicial de Git

Configurar usuario global:

``` bash
git config --global user.name "TuNombre"
git config --global user.email "tu@email.com"
```

Ver configuración actual:

``` bash
git config --list
```

Editar configuración:

``` bash
git config --global --edit
```

------------------------------------------------------------------------

# 2. Crear o clonar repositorios

Crear repositorio nuevo:

``` bash
git init
```

Clonar repositorio existente:

``` bash
git clone URL_DEL_REPOSITORIO
```

Ejemplo:

``` bash
git clone https://github.com/usuario/proyecto.git
```

------------------------------------------------------------------------

# 3. Estado del repositorio

``` bash
git status
git log
git log --oneline
git log --graph --oneline --all
```

------------------------------------------------------------------------

# 4. Manejo de archivos

``` bash
git add archivo.txt
git add .
git add -u
git rm archivo.txt
git rm --cached archivo.txt
```

------------------------------------------------------------------------

# 5. Crear commits

``` bash
git commit -m "mensaje del commit"
git commit -am "mensaje"
git commit --amend
```

------------------------------------------------------------------------

# 6. Ramas

Ver en qué rama estás:

``` bash
git branch
git status
```

Ver todas las ramas:

``` bash
git branch -a
```

Crear rama:

``` bash
git branch nombre-rama
```

Cambiar de rama:

``` bash
git switch nombre-rama
```

Crear y cambiar a una rama:

``` bash
git switch -c nombre-rama
```

Eliminar rama:

``` bash
git branch -d nombre-rama
git branch -D nombre-rama
```

------------------------------------------------------------------------

# 7. Fusionar ramas

``` bash
git switch main
git merge nombre-rama
```

------------------------------------------------------------------------

# 8. Manejo de repositorios remotos

``` bash
git remote -v
git remote add origin URL
git remote set-url origin URL
git remote remove origin
```

------------------------------------------------------------------------

# 9. Subir y bajar cambios

``` bash
git push
git push -u origin main
git push origin nombre-rama
git pull
git fetch
```

------------------------------------------------------------------------

# 10. Ver diferencias

``` bash
git diff
git diff --staged
git diff commit1 commit2
```

------------------------------------------------------------------------

# 11. Deshacer cambios

``` bash
git restore archivo.txt
git restore --staged archivo.txt
git reset --hard HEAD
git reset --hard ID_COMMIT
```

------------------------------------------------------------------------

# 12. Stash

``` bash
git stash
git stash list
git stash pop
```

------------------------------------------------------------------------

# 13. Tags

``` bash
git tag v1.0
git tag
git push origin v1.0
git push --tags
```

------------------------------------------------------------------------

# 14. .gitignore

Ejemplo:

    node_modules/
    .env
    *.log
    dist/

Aplicar cambios:

``` bash
git rm -r --cached .
git add .
git commit -m "apply gitignore"
```

------------------------------------------------------------------------

# 15. GitHub CLI

Login:

``` bash
gh auth login
```

Estado:

``` bash
gh auth status
```

Logout:

``` bash
gh auth logout
```

------------------------------------------------------------------------

# 16. Crear repositorios con GitHub CLI

``` bash
gh repo create
gh repo create nombre-repo --source=. --push
gh repo clone usuario/repositorio
```

------------------------------------------------------------------------

# 17. Pull Requests

``` bash
gh pr create
gh pr list
gh pr view
gh pr merge
```

------------------------------------------------------------------------

# 18. Issues

``` bash
gh issue create
gh issue list
gh issue view NUMERO
gh issue close NUMERO
```

------------------------------------------------------------------------

# 19. Releases

``` bash
gh release create v1.0
gh release list
```

------------------------------------------------------------------------

# 20. Flujo de trabajo recomendado

``` bash
git pull
git switch -c nueva-feature
git add .
git commit -m "feat: nueva funcionalidad"
git push origin nueva-feature
gh pr create
```

------------------------------------------------------------------------

# 21. Flujo típico

1.  Actualizar repositorio
2.  Crear rama
3.  Hacer cambios
4.  Commit
5.  Push
6.  Pull Request
7.  Merge

------------------------------------------------------------------------

# 22. Alias útiles

``` bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --all"
```

Uso:

``` bash
git st
git lg
```

------------------------------------------------------------------------

# 23. Buenas prácticas

-   Commits pequeños
-   Mensajes claros
-   Usar ramas para features
-   No trabajar directamente en main
-   Usar Pull Requests
-   Revisar git status antes de commit

------------------------------------------------------------------------

# 24. Mensajes de commit recomendados

    feat: agregar sistema de login
    fix: corregir error
    docs: actualizar README
    refactor: simplificar función
    style: formatear código

------------------------------------------------------------------------

# 25. Comandos más usados

``` bash
git status
git add .
git commit -m "mensaje"
git push
git pull
git branch
git switch rama
git switch -c nueva-rama
git merge rama
git log --oneline
```
