+++
date = '2026-02-20T10:17:33-08:00'
draft = false
title = 'Practica0'
+++

##  Paradigmas de la Programación

###  Práctica 0: Uso de repositorios

__Saúl Ignacio Meseguer Delgado__

---

### ¿Qué es Markdown?

Markdown es un lenguaje de marcado ligero que permite añadir elementos de formato a documentos de texto sin formato.

### ¿Cómo se utiliza?

Se utilizan editores de texto y de preferencia un renderizador.

### Sintaxis

Ejemplos: 

| Encabezado | Énfasis |
|---|:---:|
|`# Esto es un encabezado`|`*Texto en italica*`|
|`## Esto es un encabezado 2`|`_Texto en italica_`|
|`### Esto es un encabezado 3`|`**Texto en negrita**`|
|`#### Esto es un encabezado 4`|`__Texto en negrita__`|
||`~~Tachado~~`|

| Lista | Lista de tareas |
|---|:---:|
|`* Lista desordenada`|`- [x] completado`|
|`- Lista desordenada`|`- [] incompleto`|
|`+ Lista desordenada`|
|`1. Lista ordenada`|

| Imagen | Link |
|---|---|
|`![Imagen](/imagen/aqui.png)`|`[Pagina](https://www.aqui.com)`|
|`![Imagen](/imagen/aqui.png "Titulo")`|`[Pagina](https://www.aqui.com "Titulo")`|

| Division | Cita |
|:---:|---|
|`***`|`> Esta es una cita larga,`|
|`___`|`> aqui continua y despues`|
|`---`|`> llega por aqui.`|

---

### ¿Qué es Git?

Es un sistema de control de versiones local. Permite comentar y guardar los cambios a los archivos de un proyecto.

### ¿Qué es GitHub?

Github es una plataforma que aloja proyectos de Git. Puedes crear, subir y clonar repositorios.

### Comandos de Git

| Comando | Utilidad |
|:---:|:---:|
|`git init`|Inicializa un repositorio local|
|`git clone [url]`|Clona un repositorio desde una url al directorio actual|
|`git status`|Muestra los arhivos modificados en el directorio acual|
|`git commit -m "[descripcion]"`|Confirma los cambios realizados. Introduce la descripción del cambio.|
|`git push [alias] [branch]`|Envia los cambios del repositorio local al repositorio remoto.|

---

### ¿Qué es Hugo?

Es un framework que genera sitios web estaticos utilizando archivos Markdown.

### ¿Qué es GitHub Actions?

Es una plataforma de automatizacion de flujos de trabajos. Compila código automaticamente desde un push o pull y lo publica en _GitHub Pages_.

### ¿Cómo crear un sitio estático con Hugo?

1. Verificar Hugo está instalado. `hugo version`

2. Crear el sitio. `hugo new site nombre`, `cd nombre`

3. Inicializar git. `git init`

4. Agregar un tema y configurar el sitio. `echo "theme = 'ananke'" >> hugo.toml`

5. Agregar a GitHub Actions. `mkdir .github/workflows`, `touch hugo.yaml`

6. Configurar GitHub Pages desde la configuracion del repositorio.

---

### Enlaces

__Portafolio__ https://github.com/1meseguer/portafolio

__GitHub Pages__ https://1meseguer.github.io/portafolio/