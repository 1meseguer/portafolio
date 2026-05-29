---
date: '2026-05-27T17:41:25-07:00'
draft: false
title: 'Practica3'
---

##  Paradigmas de la Programación

###  Práctica 3: Haskell

__Saúl Ignacio Meseguer Delgado__

---

### Introducción

__Haskell__ es un lenguaje de programación funcional. 

Basa su modelo de ejecución en la evaluación de expresiones en lugar de la ejecución secuencial de instrucciones.

---

### Instalacion del entorno

Pasos para instalar Haskell en Windows:

1. Dirigete a [](haskell.org/ghcup/), la pagina del inistalador principal de Haskell.
2. Debes ingresar el siguiente comando en el PowerShell de tu computadora (no lo ejecutes en modo adiminstrador).
```
Set-ExecutionPolicy Bypass -Scope Process -Force;[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; try { & ([ScriptBlock]::Create((Invoke-WebRequest https://www.haskell.org/ghcup/sh/bootstrap-haskell.ps1 -UseBasicParsing))) -Interactive -DisableCurl } catch { Write-Error $_ }
```
3. Al ingresar el comando se indica que se instalará:
 - __GHCup__ - Instalador de Haskell
 - __GHC__ - Compilador de Haskell
 - __MSYS2__ - Gestor de paquetes
 - __Cabal__ - Herramienta de compilación de Haskell
 - __Stack__ - Herramienta de compilación de Haskell (una alternativa a Cabal)
 - __HLS__ - Servidor de lenguaje Haskell

La misma pantalla porporciona una ruta sugerida para la instalación. Si no quieres cambiar la ruta de instalacion presiona _Enter_ y el proceso iniciará.

4. Para verificar la instalación puedes ingresar los siguientes comandos:
```
# GHCup
ghcup --version
# The GHCup Haskell installer, version 0.1.50.2

# GHC
ghc --version
# The Glorious Glasgow Haskell Compilation System, version 9.6.7

# Cabal
cabal --version
# cabal-install version 3.14.2.0
# compiled using version 3.14.2.0 of the Cabal library
```

---

### Funcionamiento de la aplicación TODO

Para explicar el funcionamiento de una aplicacion de lista de tares utilizaré la aplicacion de _Steadylearner_ publicada en [](dev.to) desde el siguiente link: [](https://dev.to/steadylearner/how-to-use-stack-to-build-a-haskell-app-499j)

__Main.hs__ inicia la aplicacion:

```
module Main where
```

Declara que el archivo es el modulo principal _main_.

```
import Lib (prompt)
```

Importa la funcion _prompt_ del modulo _Lib_.

```
main :: IO ()
```

Declara que la funcion main realizará operacion de entrada y salida.

```
prompt []
```

Llama la funcion _prompt_ con una lista vacia. 

__Lib.hs__ que contiene la logica del programa:

```
module Lib
  ( prompt,
    editIndex,
  )
where

import Data.List
```

- ```module Lib``` - Define las funciones que podran usarse en _Main.hs_.
- ```prompt``` - Es el bucle principal de la aplicación.
- ```editIndex``` - Sirve para editar un elemento.
- ```import Data.List``` - Incluye la funcion ```isPrefixOf``` que regresa un ```true``` si una lista es prefijo de la segunda.

```
putTodo :: (Int, String) -> IO ()
putTodo (n, todo) = putStrLn (show n ++ ": " ++ todo)
```

Esta funcion recibe una tupla ```(Int, String)``` -> ```(Índice, Tarea)``` y la imprime con formato.

```
prompt :: [String] -> IO ()
prompt todos = do
  putStrLn ""
  putStrLn "Test todo with Haskell. You can use +(create), -(delete), s(show), e(dit), l(ist), r(everse), c(lear), q(uit) commands."
  command <- getLine
  
  if "e" `isPrefixOf` command
    then do
      print "What is the new todo for that?"
      newTodo <- getLine
      editTodo command todos newTodo
    else interpret command todos
```

- ```prompt :: [String] -> IO ()``` - Recibe la lista de tareas en String.
- ```command <- getLine``` - Guarda en command lo que escribe el usuario.

Si el comando empieza con _e_:
- Llama a ```editTodo``` y pide un nuevo texto para la tarea, sino llama a ```interpret```.

__Funcion interpret__

```
interpret ('+' : ' ' : todo) todos = prompt (todo : todos)
```

Cuando el comando empieza con ```+ ``` agrega la tarea al principio de la lista.

```
interpret ('-' : ' ' : num) todos =
  case deleteOne (read num) todos of
    Nothing -> do
      putStrLn "No TODO entry matches the given number"
      prompt todos
    Just todos' -> prompt todos'
```

Cuando el comando empieza con ```- ``` elimina la tarea en la posicion que se indica.

```
interpret "l" todos = do
  let numberOfTodos = length todos
  putStrLn ""
  print $ show numberOfTodos ++ " in total"
  mapM_ putTodo (zip [0 ..] todos)
  prompt todos
```

El comando ```l``` muestra todas las tareas, numerandolas desde el _0_.

```
interpret "r" todos = do
  let numberOfTodos = length todos
  putStrLn ""
  print $ show numberOfTodos ++ " in total"
  let reversedTodos = reverseTodos todos
  mapM_ putTodo (zip [0 ..] reversedTodos)
  prompt todos
```

El comando ```r``` invierte el orden de las tareas y las imprime.

```
interpret "c" todos = do
  print "Clear todo list."
  prompt []
```

El comando ```c``` borra las tareas de la lista.

```
interpret "q" todos = return ()
```

El comando ```q``` termina el programa.

```
interpret command todos = do
  putStrLn ("Invalid command: `" ++ command ++ "`")
  prompt todos
```

Muestra un error cuando el comando no es valido.

---

### Referencias

[](https://dev.to/steadylearner/how-to-use-stack-to-build-a-haskell-app-499j)