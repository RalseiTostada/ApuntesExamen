# ApuntesExamen
## 1. Git, Branques i Tags (Terminal)

```bash
# Inicializar y rama master
mkdir -p ~/git/web
cd ~/git/web
git init
git branch -m master

# Primer commit
echo "# Mi sitio" > Readme.md
git add Readme.md
git commit -m "Primer commit"

# Crear y cambiar a rama
git switch -c contacte

# Ver estado y commits
git status
git log --oneline
git log --oneline --graph --all

# Volver a master, hacer cambio y fusionar
git switch master
# (hacer cambio...)
git add .
git commit -m "Cambio en master"
git merge contacte
# (Para salir de Vim en el merge: Esc -> :wq -> Enter)

# Tag anotado y borrado de rama
git tag -a entrega-Abel -m "Entrega"
git branch -d contacte

# Subir a GitHub
git remote add origin [https://github.com/TU_USUARIO/REPO.git](https://github.com/TU_USUARIO/REPO.git)
git push -u origin master
git push origin entrega-Abel # Subir el tag explícitamente


2. GitHub Pages
Subir archivos web a la rama principal (main o master).

Ir a Settings -> Pages (menú lateral izquierdo).

En Build and deployment / Source: seleccionar Deploy from a branch.

En Branch: elegir main / /(root) (o master) y pulsar Save.


GitHub Actions (.github/workflows/ci.yml)
Ruta del archivo: .github/workflows/ci.yml

name: CI Workflow

on:
  push:
    branches: [ "main" ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout del código
        uses: actions/checkout@v4

      - name: Ejecutar Tests
        id: run-tests
        run: npm test

      # Step para capturar error si los tests fallan:
      - name: Notificar error si fallan los tests
        if: failure()
        run: echo "ERROR: Los tests han fallado durante la ejecución"


Ruta del archivo: Jenkinsfile en la raíz del proyecto.

pipeline {
    agent any

    stages {
        stage('Checkout y JS') {
            steps {
                checkout scm
                sh 'node index.js'
            }
        }

        stage('Stages Paralelos') {
            parallel {
                stage('Rama Paralela A') {
                    steps {
                        sh 'echo "Ejecutando proceso A"'
                    }
                }
                stage('Rama Paralela B') {
                    steps {
                        sh 'echo "Ejecutando proceso B"'
                    }
                }
            }
        }
    }
}


# ¿Te has equivocado en el último mensaje de commit?
git commit --amend -m "Nuevo mensaje correcto"

# ¿Te pide credenciales o token al hacer git push?
# Si te pide contraseña, GitHub ya no acepta la clave normal, exige un Personal Access Token (PAT).
# Se crea en GitHub: Settings -> Developer Settings -> Personal access tokens -> Tokens (classic) -> Generate new token (con permiso 'repo').

# Comprobar a qué repositorio remoto estás apuntando:
git remote -v

# Si te equivocaste con la URL del remoto y quieres cambiarla:
git remote set-url origin https://github.com/TU_USUARIO/REPO_CORRECTO.git

# Ver en qué rama estás y qué ramas existen:
git branch -a


Si en la pregunta de GitHub Pages te piden crear un archivo HTML básico o verificar los archivos:

Recuerda que el archivo principal DEBE llamarse obligatoriamente index.html (en minúsculas) para que GitHub Pages lo reconozca como la página de inicio.

Estructura mínima si tienes que crear uno rápido desde la terminal:

cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Mi Web</title>
</head>
<body>
    <h1>¡Página desplegada correctamente en GitHub Pages!</h1>
</body>
</html>
EOF


pwd              # Muestra en qué carpeta exacta estás metido
ls -la           # Lista TODOS los archivos (incluyendo .git y .github que son ocultos)
rm -rf nombre    # Borra una carpeta y todo su contenido (ÚSALO CON CUIDADO)
cat archivo      # Muestra el contenido de un archivo en la terminal sin abrirlo  


crear carpeta para el ejercicio:
mkdir examen-git
cd examen-git

Ver qué hay en el directorio: ls -la (el -a es clave para ver archivos ocultos como la carpeta .git o .github).


# 1. Inicializar Git en la carpeta actual
git init

# 2. Configurar usuario/email si la máquina virtual/terminal está limpia
git config user.name "TuNombre"
git config user.email "tu@email.com"

# 3. Crear o añadir archivos
git add .

# 4. Hacer el primer commit
git commit -m "Initial commit"

# 5. Cambiar el nombre de la rama a main (si no se llama así ya)
git branch -M main

# 6. Conectar tu repositorio local con el remoto de GitHub
git remote add origin https://github.com/tu-usuario/nombre-del-repo.git

# 7. Subir todo a GitHub
git push -u origin main

Si ya te dan el repositorio:
git clone https://github.com/profesor/repositorio.git
cd repositorio


crear repo github actions:
# 1. Crear las carpetas de workflows (el parámetro -p crea la ruta completa)
mkdir -p .github/workflows

# 2. Entrar o crear el archivo del flujo de trabajo (workflow)
nano .github/workflows/mi-action.yml


En el editor nano:

Escribes o pegas la configuración YAML que te pida la pregunta.

Guardas con Ctrl + O, confirmas con Enter.

Sales con Ctrl + X.


Luego, solo queda subirlos a GitHub para que la Action se dispare:
git add .
git commit -m "Añadida GitHub Action"
git push


¿Qué pide el enunciado paso a paso?
Partir de un repositorio existente: Te basas en una actividad anterior donde ya hay tests configurados.

Modificar el workflow (.github/workflows/...): Tienes que añadir un nuevo step que se ejecute solo si los tests fallan, y que imprima por pantalla un mensaje de error.

Hacer push en la rama concreta para activar el flujo.

Verificar que se ejecuta, que detecta el fallo y muestra el mensaje.

Entregar: Enlaces del repositorio y capturas de pantalla (en el examen real esto último dependerá de cómo os pidan la entrega, pero la parte técnica es la misma).

cd nombre-del-repositorio
nano .github/workflows/tu-archivo.yml
Añades la condición if: failure() y el comando run: echo "..." en el paso que te piden.

Guardas (Ctrl + O, Enter) y sales (Ctrl + X).
git status
git add .
git commit -m "Modificado workflow para detectar fallos en los tests"
git push origin main

Vas a tu repositorio en la web de GitHub $\rightarrow$ pestaña Actions.Verás el workflow ejecutándose. Si los tests fallan, el paso que programaste con if: failure() se pondrá en marcha de color verde (porque hizo lo que tenía que hacer) y mostrará el mensaje en la consola del job.

1. Configuración de la rama principal (main vs master)
A veces git init crea la rama como main y el examen te pide master (o al revés).

Para comprobar la rama actual: git branch --show-current

Para cambiar de nombre a la rama: git branch -m master

2. Crear y cambiar de rama
En lugar del comando antiguo (git checkout), la guía usa el comando moderno:

Crear y cambiar a una rama nueva: git switch -c nombre-rama

Moverte a una rama que ya existe: git switch nombre-rama

3. El editor Vim en el git merge
Cuando haces git merge entre dos ramas que han divergido (como en el paso 4), Git te abrirá un editor por defecto en la terminal (normalmente Vim) para confirmar el mensaje de commit.

No te asustes si se "bloquea" la pantalla:

Presiona Esc.

Escribe :wq (significa write and quit, guardar y salir).

Presiona Enter.

4. Crear un Tag anotado (Etiqueta)
Las etiquetas sirven para "marcar" una versión o entrega exacta en la historia de commits:
git tag -a entrega-tuNombre -m "Mensaje de la entrega"









Comandos de Linux y Git para subir cualquier proyecto a GitHub
1. Crear y prepararlo en la terminal de Linux:
# 1. Crear la carpeta e ingresar en ella
mkdir ~/apuntes-examen
cd ~/apuntes-examen

# 2. Inicializar el repositorio local de Git
git init

# 3. Cambiar el nombre de la rama principal a main (o master, según te pida)
git branch -M main

# 4. Crear el archivo de apuntes o de proyecto
nano README.md
# (Escribes el contenido, guardas con Ctrl+O, Enter y sales con Ctrl+X)

# 5. Guardar los cambios localmente
git add .
git commit -m "Añadidos apuntes del examen"

2. Conectar con GitHub y subir los archivos:
# 6. Vincular tu carpeta local con el repositorio remoto de GitHub
git remote add origin https://github.com/TU_USUARIO/NOMBRE_REPO.git

# 7. Subir todo por primera vez
git push -u origin main

¿Cómo hacer cambios y volver a subir? (El ciclo continuo)
Si más adelante modificas algún archivo o añades cosas nuevas, solo necesitas repetir estos 3 comandos:
git add .
git commit -m "Actualizados los apuntes"
git push

Resumen de lo que tienes que recordar
ejecutar la secuencia git init $\rightarrow$ git add $\rightarrow$ git commit $\rightarrow$ git remote add $\rightarrow$ git push, ya tienes el núcleo de la asignatura superado.
