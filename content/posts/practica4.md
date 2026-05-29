---
date: '2026-05-27T17:41:43-07:00'
draft: true
title: 'Practica4'
---

## Paradigmas de la Programación

### Practica 4: Programación con Prolog

__Saul Ignacio Meseguer Delgado__

---

### Introduccion
Se desarollaran ejericios de programacion clasicos utlizando Prolog: las Torres de Hanoi y el problema del Mono y la banana.

### Objetivos
- Comprender el paradigma de programación lógica en Prolog
- Aplicar técnicas de búsqueda y backtracking en Prolog

---

### Torres de Hanoi
En este problema existen tres torres y *n* cantidad de aros de diferentes tamaños. 

El problema establece tres reglas:
1. Solo se puede mover un aro a la vez.
2. Se debe tomar el aro superior de la torre y colocarlo en otra.
3. No se puede colocar un aro grande sobre uno pequeño.

```prolog
mover(1, Origen, Destino, _) :-
    format('Mover disco 1 de ~w a ~w~n', [Origen, Destino]).
```

```prolog
mover(N, Origen, Destino, Auxiliar) :-
    N > 1,
    N1 is N - 1,
    mover(N1, Origen, Auxiliar, Destino),
    mover(1, Origen, Destino, Auxiliar),
    mover(N1, Auxiliar, Destino, Origen).
```

```prolog
hanoi(N) :-
    format('~nResolviendo Torres de Hanoi con ~d discos:~n', [N]),
    format('Usando pilares: izquierda, centro, derecha~n~n', []),
    mover(N, izquierda, derecha, centro),
    format('~nTotal de movimientos: ~d~n', [2**N - 1]).
```

---

### Problema El Mono y la bnana
En este problema un mono debe alcanzar una banana que cuelga usando una caja.

- **Estados:** Posición del mono, posición de la caja, estado de la banana.
- **Acciones:** Caminar, empujar, subirse y agarrar.

```prolog
inicial(estado(suelo, esquina, no)).
final(estado(_, _, si)).

% Reglas de movimiento
mover(estado(PosMono, PosCaja, Tiene),
      estado(NuevaPos, PosCaja, Tiene)) :-
    PosMono \= NuevaPos,
    member(NuevaPos, [suelo, cerca_banana, esquina]).

mover(estado(PosMono, PosCaja, Tiene),
      estado(PosCaja, PosCaja, Tiene)) :-
    PosMono = PosCaja.

mover(estado(PosMono, PosCaja, Tiene),
      estado(NuevaPos, NuevaPos, Tiene)) :-
    PosMono = PosCaja,
    PosMono \= NuevaPos,
    member(NuevaPos, [cerca_banana, esquina, suelo]).

mover(estado(PosMono, PosCaja, no),
      estado(PosMono, PosCaja, si)) :-
    PosMono = PosCaja,
    PosCaja = cerca_banana.

% Búsqueda de solución
solucion(Estado, _, []) :-
    final(Estado).

solucion(EstadoActual, Visitados, [Accion|Resto]) :-
    mover(EstadoActual, EstadoSiguiente),
    not(member(EstadoSiguiente, Visitados)),
    accion(EstadoActual, EstadoSiguiente, Accion),
    solucion(EstadoSiguiente, [EstadoSiguiente|Visitados], Resto).
```
