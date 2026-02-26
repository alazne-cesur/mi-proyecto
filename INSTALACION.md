# DOCUMENTACION.md

## Introducción

Este documento describe de forma detallada el proceso de instalación, configuración y uso básico de Git, así como la creación de un repositorio local y su conexión con un repositorio remoto en GitHub. También se incluye la configuración de seguridad mediante clave SSH, la protección de la rama principal y la implementación de integración continua mediante GitHub Actions.

El objetivo es dejar constancia clara y reproducible de todos los pasos necesarios para trabajar correctamente con control de versiones en un entorno GNU/Linux.

---

## 1. Instalación de Git

En sistemas GNU/Linux basados en Debian o Ubuntu, la instalación se realiza con:

sudo apt update
sudo apt install git

En sistemas basados en RedHat o Fedora:

sudo dnf install git

Para comprobar que Git se ha instalado correctamente:

git --version

El sistema debe devolver la versión instalada.

---

## 2. Configuración inicial de Git

Antes de comenzar a trabajar, es obligatorio configurar el nombre y correo electrónico del usuario:

git config --global user.name "Nombre Apellidos"
git config --global user.email "correo@ejemplo.com"

Configurar el editor por defecto (ejemplo con nano):

git config --global core.editor "nano"

Verificar configuración:

git config --list

---

## 3. Creación del repositorio local

Crear directorio del proyecto:

mkdir mi_proyecto
cd mi_proyecto

Inicializar repositorio:

git init

Crear archivo README.md:

touch README.md

Crear archivo .gitignore:

touch .gitignore

Ejemplo de contenido de .gitignore:

*.log
*.tmp
node_modules/
.env

Añadir archivos al área de preparación:

git add .

Crear primer commit:

git commit -m "Commit inicial"

Consultar estado del repositorio:

git status

Consultar historial:

git log --oneline

---

## 4. Conexión con repositorio remoto en GitHub

Crear un repositorio en GitHub desde la plataforma web.

Añadir repositorio remoto (SSH):

git remote add origin git@github.com:usuario/mi-proyecto.git

Verificar remoto:

git remote -v

Renombrar rama principal:

git branch -M main

Subir contenido al repositorio remoto:

git push -u origin main

---

## 5. Configuración de clave SSH

Generar clave SSH:

ssh-keygen -t ed25519 -C "correo@ejemplo.com"

Iniciar agente SSH:

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

Mostrar clave pública:

cat ~/.ssh/id_ed25519.pub

Copiar la clave y añadirla en GitHub:
Settings → SSH and GPG keys → New SSH key

Probar conexión:

ssh -T git@github.com

---

## 6. Protección de la rama principal

Desde la configuración del repositorio en GitHub:

Settings → Rulesets → New branch ruleset

Configurar para la rama "main" y activar:

- Require a pull request before merging
- Required approvals (mínimo 1)
- Require status checks to pass
- Block force pushes
- Require conversation resolution before merging

Esto evita modificaciones directas en la rama principal y obliga a usar Pull Requests.

---

## 7. Flujo de trabajo recomendado

Crear nueva rama para cada funcionalidad:

git checkout -b feature/nueva-funcion

Realizar cambios y commits:

git add .
git commit -m "Descripción del cambio"

Subir rama:

git push origin feature/nueva-funcion

Abrir Pull Request en GitHub para fusionar en main.

Actualizar repositorio local:

git pull origin main

---

## 8. Integración continua con GitHub Actions

Crear directorio:

mkdir -p .github/workflows

Crear archivo:

nano .github/workflows/ci.yml

Ejemplo básico de configuración:

name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Mostrar versión de Git
        run: git --version

Subir cambios:

git add .
git commit -m "Configuración de integración continua"
git push

El pipeline se ejecutará automáticamente en cada push o pull request.

---

## 9. Comandos principales de Git

git init              → Inicializa repositorio
git add               → Añade archivos al staging
git commit            → Guarda cambios en historial
git status            → Muestra estado actual
git log               → Muestra historial
git branch            → Gestiona ramas
git checkout          → Cambia de rama
git merge             → Fusiona ramas
git pull              → Descarga y fusiona cambios remotos
git push              → Envía cambios al repositorio remoto

---

## Conclusión

La correcta instalación y configuración de Git, junto con la creación del repositorio local, la conexión segura mediante SSH, la protección de la rama principal y la implementación de integración continua, permiten establecer un entorno profesional de control de versiones.

Este procedimiento garantiza seguridad, trazabilidad, trabajo colaborativo eficiente y automatización de validaciones antes de integrar cambios en la rama principal.
