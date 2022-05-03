# Guía para hacer deploy en Heroku de una aplicación Web hecha con Flask.

## Requisitos
- Tener instalado Python 3+
- En Windows, tener habilitada la ejecución de scripts, en caso de que no, seguir las instrucciones dadas en este [enlace](https://www.alexmedina.net/habilitar-la-ejecucion-de-scripts-para-powershell/)
- Tener instalado Heroku CLI, y registrarse. [Guia de instalación de Heroku](https://devcenter.heroku.com/articles/getting-started-with-python#set-up)
* Instalacion de Heroku en windows, ejecutar en la terminal el comando:
```
npm i -g heroku
```

## Instrucciones

### ***Para Windows***

 ***Creamos un entorno virtual con Python:***
> Ejecutamos estos comandos en la terminal
```
py -m venv venv
```

***Activamos el entorno virtual***
>En Windows copiamos este enlace y presionamos enter:
```
.\venv\Scripts\activate
```
> Desde la terminal, creamos nuestro archivo Procfile, que es necesaria para que Heroku reconozca nuestra app
```
New-Item Procfile
```
> Creamos el archivo wsgi que es necesario para correr el servidor
```
New-Item wsgi.py
```
> Creamos la carpeta app, que será en este caso el contenedor de nuestra aplicación, luego ingresamos a la carpeta creada
```
mkdir app
cd app
```
> Creamos nuestro archivo de python
```
New-Item app.py 
```

### ***Para Mac***
***Creamos un entorno virtual con Python:***
```
python3 -m venv venv
```
> Copiamos este enlace y le damos enter:
```
source ./venv/bin/activate
```
> Desde la terminal, creamos nuestro archivo Procfile, que es necesaria para que Heroku reconozca nuestra app
```
touch Procfile
```
> Creamos el archivo wsgi que es necesario para correr el servidor
```
touch wsgi.py
```
> Creamos la carpeta app, que será en este caso el contenedor de nuestra aplicación, luego ingresamos a la carpeta creada
```
mkdir app
cd app
```
> Creamos nuestro archivo de python
```
touch main.py 
```

## Para ambos sistemas operativos

>Una vez activado nuestro entorno instalamos nuestras dependencias:
```
pip install flask gunicorn 
```
> Guardamos nuestras dependencias en un archivo de requerimentos
```
pip freeze > requirements.txt
```

### Al final nos debería quedar una estructura de carpetas igual a esta:
    ├── CarpetaDelProyecto
    │   └── app
    │   │    └── app.py
    │   │   venv
    │   │   Procfile
    │   │   wsgi.py      
    │   │   requirements.txt

    Obs: Es importante respetar la estructura y que los archivos Procfile, wsgi.py y requirements.txt estén al mismo nivel de la carpeta app.

### Abrimos el proyecto desde nuestro editor de código y empezaremos a rellenar los archivos creados
- Ingresamos el código de nuestra aplicación en el archivo ***app.py***

```python
from flask import Flask
 
app = Flask(__name__)
 
@app.route("/")
def home_view():
        return "<h1>Mi primer servidor de Flask con Heroku</h1>"

if __name__ =='__main__':
    app.run()
```

- Copiamos el siguiente código para nuestro archivo ***wsgi.py***
```python
from app.app import app
 
if __name__ == "__main__":
        app.run()
```

- Copiamos el siguiente código para nuestro archivo Procfile y guardamos, asegurarse que el ***Procfile*** no tenga ninguna extensión como ***Procfile.txt***
```
web: gunicorn wsgi:app
```
### Debemos convertir nuestro proyecto en un repositorio de Git para enviar nuestro proyecto a los servidores de Heroku.
### Abra una terminal y cambie el directorio actual al directorio raíz del proyecto, luego ejecute:
```
git init

git branch -M main
```

### Hacemos Login al CLI de Heroku

```
heroku login
```
### Crear un nombre para nuestra aplicación
```
heroku create nombre-de-mi-aplicacion
```
### Luego, debemos agregar un nuevo control remoto de Heroku a nuestro repositorio.
```
heroku git:remote -a nombre-de-mi-aplicacion
```
### Luego utilizamos este comando para inicializar el servidor
```
heroku ps:scale web=1
```
### Agregar un buildpack de Python a Heroku
```
heroku buildpacks:set heroku/python
```

### Por último pero no menos importante, asegurarse de estar en la carpeta principal del proyecto para ejecutar los siguientes comandos
```
git add .

git commit -m "Primer commit"

git push heroku main
```
