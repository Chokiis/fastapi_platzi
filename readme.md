# Curso de Introducción a FastAPI

## 1.- FastAPI
Es un framework de alto rendimiento para la creación de API mediante Python, recopilando lo mejor del ecosistema de Python 

### Características
- Rápido
- Menos errores
- Fácil e intuitivo
- Robusto
- Basado en estándares

### Marco utilizado por FastAPI
- Starlette: Framework asíncrono para la construcción de servicios y es uno de los más rapidos de Python
- Pydantic: Encargado de las validaciones de datos en Python.
- Uvicorn: Encargado de ejecturar las aplicaciones con FastAPI.
------------
## 2.- Creación de un entorno virtual e instalación de FastAPI y Uvicorn
* Levantamiento de entorno con Venv.
	```shell
	py -m venv venv
	```
* Activación del entorno virtual con Windows.
    ```shell
    venv/Scripts/activate
    ```
* Activación del entorno virtual con Linux.
    ```shell
    source venv/bin/activate
    ```
* Instalación de FastAPI.
    ```shell
    pip install fastapi
    ```
* Instalación de Uvicorn.
    ```shell
    pip install uvicorn
    ```
* Creación de primera app con FastAPI.
    ```python
    from fastapi import FastAPI

    app = FastAPI()

    @app.get('/')
    def message():
        return "Hello World!"
    ```
* Ejecutar la app en el entorno de desarrollo para ver los cambios reflejados.
    ```shell
    uvicorn main:app --reload
    ```
    ```shell
    uvicorn main:app --reload --port 5000
    ```
------------
## 3.- Documentación automática con Swagger
Describe cada uno de los endpoints que tiene la aplicación basándose en los estándares abiertos de OpenAPI.

* Para acceder a la documentación, solamente agregar la dirección "/docs" mediante http://localhost:5000/docs

    ```python
    from fastapi import FastAPI

    app = FastAPI()
    app.title = "Mi aplicación con FastAPI" #Coloca el nombre de la app
    app.version = "0.0.1" # Coloca la versión de la app

    # Creación del Endpoint
    @app.get('/', tags=["home"])
    def message():
        return "Hello World!"
    ```
## 4.- Métodos HTTP
El protocolo HTTP es aquel que define un conjunto de métodos de petición que indican la acción que se desea realizar para un recurso determinado del servidor.

Los principales métodos soportados por HTTP y por ello usados por una API REST son:
- POST: crear un recurso nuevo.
- PUT: modificar un recurso existente.
- GET: consultar información de un recurso.
- DELETE: eliminar un recurso.

### ¿De qué tratará la app?
El proyecto a realizar será una API que brindará información relacionada con películas, por lo que se tiene lo siguiente:
- **Consulta de todas las películas:** Se utilizará el método GET para las consultar y solicitar los datos de todas las películas.
- **Filtrado de películas:** Se solicitará la información de las películas por su ID y por la categoría en la que pertenecen, por lo que se utilizará el método GET y se pasarán parámetros de ruta y parámetros de query.
- **Registro de películas:** Se utilizará el método POST para registros los datos de las películas junto con la ayuda de la librería Pydantic para el manejo de los datos.
- **Modificación y eliminación de películas:** Se utilizará los métodos PUT y DELETE paa la modificacíón y eliminación de datos en la aplicación.   

## 5.- Método GET

- Lista de películas
    ```python
    movies = [
        {
            "id": 1,
            "title": "The Galactic Adventure",
            "overview": "Una película galáctica.",
            "year": 2020,
            "rating": 8.0,
            "category": "Acción",
        },
        {
            "id": 2,
            "title": "La Gran Comedia",
            "overview": "Una película graciosa.",
            "year": 2019,
            "rating": 7.5,
            "category": "Comedia,
        },
        {
            "id": 3,
            "title": "Drama in the City",
            "overview": "Una película de drama.",
            "year": 2021,
            "rating": 8.5,
            "category": "Drama",
        },
        {
            "id": 4,
            "title": "Mystery Island",
            "overview": "Una película misteriosa.",
            "year": 2018,
            "rating": 9.0,
            "category": "Misterio",
        },
        {
            "id": 5,
            "title": "Aventura Extrema",
            "overview": "Una película extrema.",
            "year": 2022,
            "rating": 7.8,
            "category": "Acción",
        },
        # Agrega más películas ficticias basadas en películas reales según sea necesario
    ]

    # Creación de la función GET para obtener todas las películas.
    @app.get('/movies', tags=['movies']) #Etiqueta 'movies'.
    def get_movies():
        return movies
    ```
## Parámetros de ruta
Parámetros de ruta: Estos son valores que se extraen directamente de la ruta misma, como {id}. 

Para ingresar un parámetro dentro de la ruta, será necesario definir una lista de elementos para que funcione correctamente.

* Crear lista con varias películas.
    ```python
    movies = [
        {
            "id": 1,
            "title": "The Galactic Adventure",
            "overview": "Una película galáctica.",
            "year": 2020,
            "rating": 8.0,
            "category": "Acción",
        },
        {
            "id": 2,
            "title": "La Gran Comedia",
            "overview": "Una película graciosa.",
            "year": 2019,
            "rating": 7.5,
            "category": "Comedia",
        }
    ]
    
    # Crear ruta con parámetro
    @app.get('/movies/{id}', tags=['movies'])
    def get_movie(id: int): # Solamente recibirá un valor INT.
    for item in movies: # item: iterador,  movies: lista.
            if item['id'] == id: # Si el iterador entra un ID igual que el del parámetro,  regresa el item (la película).
                return item
        return [] # Si no encuentra el item, regresa una lista vacía.
    ```
### Request Query:
- http://127.0.0.1:5000/movies/1
----- 
## Parámetros Query
Parámetro Query: son un conjunto de parámetros opcionales que se añaden en la URL al finalizar la ruta, con la finalidad de definir acciones o contenido en la URL, se pueden visualizar estos elementos después de un '**?**', para agregar más parámetros query se utiliza '**&**'.

* Crear la ruta por categorías.
    ```python
    @app.get('/movies', tags=['movies'])
    def get_movies_by_category(category: str, year: int) # Parámetros categoria y año.
        return [item for item in movies if item['category'] == category] # Devuelve un item en un for que recorre toda la lista de 'movies' y si el item coincide con una categoría de la lista, trae las películas de esa categoría en una lista.
    ```