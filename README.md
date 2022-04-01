# password_manager
Un administrador de contraseñas hecho en python

## Requisitos
- Python 3.6 o superior
- Pipenv

## Instalación
```
git clone https://github.com/B5ala5/password_manager
cd password_manager
pipenv install --ignore-pipfile
```

## Guía de uso
1. Entrar en el entorno virtual
```
pipenv shell
```
2. Ejecutar el programa
```
python ./src/main.py
```
Output:
```
.::Password Vault::.

1. Generate new key
2. Store a password
3. List App/Site stored
4. Find credentials from App/Site
5. Encrypt file "passwords.csv"
6. Decrypt file "passwords.csv"
7. Exit
```
### Opciones

1. Generate new key

Genera una nueva clave con la cual se podra almacenar, encriptar y desencriptar las contraseñas.

<br>

2. Store a password

Guarda una contraseña indicando el nombre de la aplicación/sitio web, nombre de usuario y correo asociados a esta

<br>

3. List App/Site stored

Muestra una lista de todas las aplicaciones o sitios web registrados

<br>

4. Find crendentials from App/Site

Busca las credenciales asociadas a una aplicación o sitio web

<br>

5. Encrypt file "passwords.csv"

Encripta el archivo que contiene las contraseñas

<br>

6. Decrypt file "passwords.csv"

Desencripta el archivo que contiene las contraseñas

<br>

7. Exit

Cierra el administrador de contraseñas
