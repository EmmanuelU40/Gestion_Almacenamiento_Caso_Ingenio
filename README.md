# Gestion_Almacenamiento_Caso_Ingenio

Este proyecto implementa se realiza con el fin de implementar una pipeline ETL (Extract, Transform, Load) dentro del context de un ingenio (extenderse mas aqui con la justificacion)


## Requisitos
Antes de proceder con la ejecucion del proyecto, asegurate de cumplir con los siguientes requisitos
-Python 3.13
-Venv (Para la gestion de la dependencia)
-MySQL Server
-JupyterLab

## Instalacion y configuracion
###Clonar el repositorio
```bash
    git clone https://github.com/juancho191327/customer-churn-etl.git](https://github.com/EmmanuelU40/Gestion_Almacenamiento_Caso_Ingenio.git
    cd Ambiente_Gestion
```
### Configurar el entorno virtual con Venv
```bash
    python -m venv nombre_del_entorno
    venv -scripts -activate
    venv\Scripts\activate.bat
    venv\Scripts\activate 
```
### Crea un archivo `config.yaml` en la raíz del proyecto con la siguiente estructura:
```ini
    db_user = db_config["user"]
    db_password = db_config["password"]
    db_host = db_config["host"]
    db_port = 3306 
    db_name = db_config["name"]
```
### Para ejecutar el pipeline, simplemente ejecute el jupyter y proceda a correr el codigo
```bash
    jupyter lab --no-browser
```
