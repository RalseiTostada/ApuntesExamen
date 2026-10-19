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
git tag -a entrega -m "Entrega"
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



Para recordarlo de memoria, el truco es seguir siempre esta misma secuencia:

git init (Iniciar el repositorio en la carpeta).

git branch -M main (Asegurarte de que la rama se llama main).

git add . (Preparar todos los archivos nuevos o modificados).

git commit -m "Mensaje descriptivo" (Guardar los cambios localmente con una nota).

git remote add origin <URL> (Solo la primera vez que vinculas el repo con GitHub).

git push -u origin main (Subirlo a la nube).

haces un cambio a futuro:

git add .
git commit -m "Actualizando archivos"
git push











      Ejercicio 1:

Paso 1: Crear el repositorio local y mostrar su contenido
Primero, creamos la carpeta del proyecto, accedemos a ella e inicializamos el repositorio de Git.

# 1. Crear la carpeta del proyecto
mkdir activitatsPrimeraClaseDAW

# 2. Entrar en la carpeta
cd activitatsPrimeraClaseDAW

# 3. Inicializar el repositorio de Git local
git init

# 4. Mostrar todo el contenido (opción -a para ocultos, -l para detalle)
ls -la

Paso 2: Crear el archivo README.md y comprobar estados
Crearemos el archivo README.md con el texto que has especificado.

# 1. Crear el archivo README.md con el texto solicitado
cat << 'EOF' > README.md
Aquest repositori conté una recopilació d'activitats molt senzilles realitzades a la primera classe sobre Sistemes de control de versions.
1. Configuració d'algunes preferències
2. Instruccions per crear un primer repositori
3. Afegiu un fitxer al repositori movent-lo del staged a la zona local comprovant l'estat.
EOF

luego comprobamos con:
# 2. Comprovación:
git status

A continuación, lo movemos a la zona de preparación (Staging Area / Index):
# 3. Añadir el archivo al staging area
git add README.md

# 4. Volver a comprobar el estado
git status

Paso 3: Realizar el commit
Guardamos el estado actual del área de preparación en el historial de Git.
git commit -m "Afegit fitxer Readme"

Paso 4: Mostrar la traça de log en una sola línea personalizada
Para mostrar el registro de commits (log) formateado en una sola línea con el hash, el autor, la data y la hora del cambio, utilizamos el parámetro --pretty=format:
git log --pretty=format:"%h - %an, %ad : %s" --date=format:"%Y-%m-%d %H:%M:%S"

Explicación de la sintaxis:

%h: Hash corto del commit.

%an: Nombre del autor (Author Name).

%ad: Fecha de la aportación (Author Date).

%s: Mensaje del commit (Subject).

--date=format:"%Y-%m-%d %H:%M:%S": Define el formato específico de fecha y hora.

Paso 5: Introducir un error y desfer el commit a l'stage (Mixed/Soft Reset)
Introducimos un error en el archivo, creamos el commit y posteriormente lo deshacemos manteniendo los cambios en el Staging Area.

# 1. Introducir un error en el archivo
echo "Això és un error d'introducció" >> README.md

# 2. Añadir al stage y realizar commit del error
git add README.md
git commit -m "Añadido error intencionado"

# 3. Deshacer el último commit dejando los cambios en el Staging Area (Soft Reset)
git reset --soft HEAD~1


Paso 6: Introducir un nuevo error y desfer completamente el commit (Hard Reset)
Para descartar un cambio por completo tanto del historial como del área de trabajo y el Staging Area, se utiliza un reseteo duro (hard reset).

# 1. Volver a hacer commit del cambio erróneo
git commit -m "Añadido segundo error"

# 2. Deshacer el commit Y eliminar los cambios definitivamente
git reset --hard HEAD~1

Paso 7: Configurar nombre de usuario, correo y editor por defecto
Para configurar la identidad global de Git y su editor de texto predeterminado, se utiliza el comando git config:

# 1. Configurar el nombre de usuario
git config --global user.name "Tu Nombre Y Apellidos"

# 2. Configurar el correo electrónico
git config --global user.email "tu_email@ejemplo.com"

# 3. Configurar el editor por defecto (ejemplos comunes):
# Para VS Code:
git config --global core.editor "code --wait"

# Para Nano (más común en terminales simples):
# git config --global core.editor "nano"

# Para Vim:
# git config --global core.editor "vim"

Para verificar que la configuración se ha guardado correctamente:
git config --list




        Ejercicio 2:

0. Crear el repositorio e inicializarlo
Creamos las carpetas correspondientes dentro del directorio personal (~), inicializamos el repositorio y añadimos el primer archivo.

# 1. Crear el directorio ~/git/web y acceder a él
mkdir -p ~/git/web
cd ~/git/web

# 2. Inicializar como repositorio Git
git init

# 3. Crear el archivo Readme.md y guardarle un contenido inicial
echo "# Proyecto Web" > Readme.md

# 4. Añadir al staging area y hacer el primer commit
git add Readme.md
git commit -m "Commit inicial: afegit Readme.md"

1. Punto de partida
Comprobamos el estado y creamos la nueva rama contacte.

# 1. Comprobar en qué rama estamos y que el área de trabajo está limpia
git status

# 2. Crear la rama 'contacte' y cambiar a ella
git checkout -b contacte

2. Trabajar en la rama contacte
Creamos el archivo index.html, agregamos la sección solicitada e iremos realizando cambios progresivos para tener dos commits distintos.

Primer commit (Crear la estructura del HTML y la sección):
# 1. Crear index.html con la estructura inicial
cat << 'EOF' > index.html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Contacto</title>
</head>
<body>
    <section>
        <h2>Contacte</h2>
    </section>
</body>
</html>
EOF

# 2. Añadir e iniciar el primer commit en la rama contacte
git add index.html
git commit -m "Afegida secció inicial de contacte en index.html"

Segundo commit (Agregar el enlace/correo electrónico):
# 3. Añadir el contenido del correo electrónico
cat << 'EOF' > index.html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Contacto</title>
</head>
<body>
    <section>
        <h2>Contacte</h2>
        <p>Escriu-me a <a href="mailto:tu@exemple.com">tu@exemple.com</a></p>
    </section>
</body>
</html>
EOF

# 4. Añadir e iniciar el segundo commit
git add index.html
git commit -m "Afegit correu de contacte a la secció"


Ver el historial de commits hechos en esta rama:
git log --oneline


3. Avanzar master en paralelo
Regresamos a la rama principal para hacer un cambio completamente independiente sin fusionar todavía contacte.

# 1. Volver a la rama master (o main según tu configuración)
git checkout master

# 2. Crear un archivo independiente new.html
echo "<h1>Pàgina Nova</h1>" > new.html

# 3. Guardar el cambio en master
git add new.html
git commit -m "Afegit fitxer new.html a master"

4. Fusionar la rama contacte dentro de master
Estando posicionados en master, realizamos la fusión (merge) de la rama contacte.

# Asegurándonos de estar en master, ejecutamos la fusión:
git merge contacte -m "Fusionada branca contacte a master"

5. Mostrar el contenido del repositorio para cada rama
Comprobamos cómo varían los archivos según la rama en la que nos encontremos.

# 1. Ver archivos en master (deben estar Readme.md, new.html e index.html)
ls -la

# 2. Cambiar temporalmente a contacte para comprobar
git checkout contacte
ls -la

# 3. Volver a master
git checkout master

6. Marcar la entrega con un Tag anotado
Creamos la etiqueta (tag) en la rama master con tu nombre.

# 1. Crear el tag anotado (-a) con mensaje (-m)
git tag -a entrega-tuNombre -m "Entrega de la pràctica de Git"
Para verificar que el tag se ha creado correctamente:
git tag -n

7. Limpiar ramas secundarias
Una vez integrados los cambios de contacte en master, procedemos a eliminar la rama local.
git branch -d contacte

8. Entrega final en GitHub
Para subir el proyecto a un repositorio remoto en GitHub junto con la etiqueta (tag) generada:

# 1. Vincular el repositorio local con el remoto de GitHub
# (Sustituye la URL por la de tu repositorio creado en GitHub)
git remote add origin https://github.com/tu-usuario/web.git

# 2. Subir la rama principal al remoto
git push -u origin master

# 3. Subir explícitamente el tag creado
git push origin entrega-tuNombre



      Ejercicio 4:
Paso 1: Crear la lista de URLs de prueba (Opcional pero recomendado)
Para probar el ejecutable generado de forma automática, creamos un archivo llamado urls.txt en la raíz del repositorio con contenido de ejemplo:


name: Compilar y Probar Ejecutables

# Dispara el workflow con cada commit (push) al repositorio
on:
  push:
    branches: [ "main", "master" ]
  workflow_dispatch:

jobs:
  # -------------------------------------------------------------
  # JOB 1: Compilar en Linux
  # -------------------------------------------------------------
  build-linux:
    runs-on: ubuntu-latest

    steps:
      - name: Descargar el código fuente
        uses: actions/checkout@v4

      - name: Configurar Python 3.12
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Instalar PyInstaller
        run: |
          python -m pip install --upgrade pip
          pip install pyinstaller

      - name: Generar el ejecutable para Linux
        run: pyinstaller --onefile comprova_desplegaments.py

      - name: Subir el artefacto de Linux
        uses: actions/upload-artifact@v4
        with:
          name: executable-linux
          path: dist/comprova_desplegaments

  # -------------------------------------------------------------
  # JOB 2: Compilar en macOS
  # -------------------------------------------------------------
  build-macos:
    runs-on: macos-latest

    steps:
      - name: Descargar el código fuente
        uses: actions/checkout@v4

      - name: Configurar Python 3.12
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Instalar PyInstaller
        run: |
          python -m pip install --upgrade pip
          pip install pyinstaller

      - name: Generar el ejecutable para macOS
        run: pyinstaller --onefile comprova_desplegaments.py

      - name: Subir el artefacto de macOS
        uses: actions/upload-artifact@v4
        with:
          name: executable-macos
          path: dist/comprova_desplegaments

  # -------------------------------------------------------------
  # JOB 3: Compilar en Windows
  # -------------------------------------------------------------
  build-windows:
    runs-on: windows-latest

    steps:
      - name: Descargar el código fuente
        uses: actions/checkout@v4

      - name: Configurar Python 3.12
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Instalar PyInstaller
        run: |
          python -m pip install --upgrade pip
          pip install pyinstaller

      - name: Generar el ejecutable para Windows
        run: pyinstaller --onefile comprova_desplegaments.py

      - name: Subir el artefacto de Windows
        uses: actions/upload-artifact@v4
        with:
          name: executable-windows
          path: dist/comprova_desplegaments.exe

  # -------------------------------------------------------------
  # JOB 4: Probar el ejecutable generado
  # -------------------------------------------------------------
  test-executable:
    runs-on: ubuntu-latest
    needs: build-linux  # Espera a que el job de compilar en Linux termine con éxito

    steps:
      - name: Descargar el código fuente (para obtener urls.txt si existe)
        uses: actions/checkout@v4

      - name: Descargar el artefacto de Linux
        uses: actions/download-artifact@v4
        with:
          name: executable-linux
          path: bin

      - name: Conceder permisos de ejecución y probar el ejecutable
        run: |
          chmod +x bin/comprova_desplegaments
          echo "=== Prueba 1: URL como argumento ==="
          ./bin/comprova_desplegaments example.com httpbin.org/status/200
          
          echo -e "\n=== Prueba 2: Generación de informe CSV ==="
          ./bin/comprova_desplegaments example.com --eixida informe_test.csv
          
          echo -e "\n=== Comprobación del contenido del CSV ==="
          cat informe_test.csv



Explicación detallada de la solución
Compilación en diferentes S.O. (build-linux, build-macos, build-windows):
Cada job utiliza el runner correspondiente (ubuntu-latest, macos-latest y windows-latest).Se utiliza actions/checkout@v4 para descargar el código y actions/setup-python@v5 configurado con la versión 3.12 de Python.
Se instala pyinstaller vía pip install pyinstaller y se ejecuta el comando pyinstaller --onefile comprova_desplegaments.py.
El resultado generado por PyInstaller se guarda en la carpeta dist/. Después utilizamos actions/upload-artifact@v4 para almacenar los ejecutables de cada sistema.
Comprobación del ejecutable (test-executable):
Se ejecuta tras finalizar el job de compilación de Linux (needs: build-linux).
Descarga el artefacto executable-linux mediante actions/download-artifact@v4.
Le concede permisos de ejecución (chmod +x) y ejecuta comprobaciones reales pasándole URLs y verificando la generación del archivo CSV, confirmando así que funciona correctamente sin necesidad de tener un entorno de Python configurado.





