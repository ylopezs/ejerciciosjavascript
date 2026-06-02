# Taller: Consumo de la API de Rick and Morty con JavaScript

## Objetivo

Aprender a consumir una API REST utilizando JavaScript y la función `fetch()`, obteniendo información de personajes de la serie Rick and Morty.

API:
https://rickandmortyapi.com/api/character

## Parte 1: Obtener un Personaje por ID

### Código base

```javascript
async function obtenerPersonaje(id) {
  // Escribe tu código aquí
}

obtenerPersonaje(1);
```

### Requisitos

- Construir la URL usando el ID recibido.
- Realizar una petición GET usando `fetch`.
- Convertir la respuesta a JSON.
- Mostrar el resultado usando `console.log`.

## Parte 2: Obtener Todos los Personajes

### Código base

```javascript
async function obtenerPersonajes() {
  // Escribe tu código aquí
}

obtenerPersonajes();
```

### Requisitos

- Consumir la API.
- Convertir la respuesta a JSON.
- Mostrar el resultado completo usando `console.log`.

## Ejercicios Adicionales

1. Mostrar únicamente los nombres de los personajes.
2. Mostrar nombre, estado y especie.
3. Implementar manejo de errores con `try/catch`.
4. Investigar cómo obtener otras páginas usando el parámetro `page`.

### Bonus

```javascript
async function obtenerPagina(numeroPagina) {
  // Implementar
}

obtenerPagina(2);
```
