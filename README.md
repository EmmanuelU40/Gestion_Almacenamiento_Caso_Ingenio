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
    git clone https://github.com/EmmanuelU40/Gestion_Almacenamiento_Caso_Ingenio.git
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
#### Caracterizacion de los datos Previo a extraccion

Para nuestro caso en especifico la empresa no nos dejo acceder directamente a la información, pero nos van a compartir archivos compilatorios cada 8 horas; la información se esta almacenando en un One drive.

Para lo anterior se manejan dos diferentes archivos:
El de Sigind es el archivo de las variables medidas desde el laboratorio de calidad y conformidad, los datos aquí representados están en frecuencia horaria en la que normalmente se realizan los análisis (intervalos de 2 horas); es de destacar que, para la empresa vista desde calidad, el día inicia desde las 6:00 am del día actual y va hasta las 5:59 am del día siguiente; también es importante mencionar que estos datos son medidos a una temperatura estándar de 20°C.
A continuación, se describe el archivo sigind/Biosalc:
•	Encabezado: en este punto describe el nombre del ingenio y la zafra que se está realizando (en este caso la zafra = año), también nos da información de la fecha en que inicia y finaliza el reporte, como también de las horas tomadas.

•	FECHA/HORA (Date): Como su nombre lo indica esta columna nos indica la fecha que se realizan los análisis es de tener presente lo mencionado anteriormente los días inician a las 6:00 am y terminan al siguiente día a las 5:59 am, en cuanto a al hora los análisis se comienzan a publicar bajo análisis desde las 6:30 am hasta las 4:30 am en un intervalo de tiempo para este caso de 2 horas.

•	JCLARBX(Float): Esta es la variable de Brix de jugo claro y los datos aquí ingresados están en °Brix, estos datos están en una escala de 0-85 y normalmente están alrededor 10-14.

•	MSCBXTA(Float): Esta es la variable de Brix de meladura y los datos aquí ingresados están en °Brix, estos datos estan en una escala de 0-85 y normalmente están alrededor de 60-68.

•	JALCPH(Float): Esta es la variable de PH de jugo enclado, estos datos están en una escala de entre 0-14 y normalmente están alrededor de 6.8 a 7.6.

•	JCLPHCL(Float): Esta es la variable de PH de jugo clarificado, estos datos están en una escala de entre 0-14 y normalmente están alrededor de 6.8 a 7.2.

•	MSCPHTA(Float): Esta es la variable de PH de la meladura saliendo de evaporación, estos datos están en una escala de entre 0-14 y normalmente están alrededor de 6.0 a 6.5.

•	JCLARPZ(Float): Esta es la variable de pureza del jugo claro, estos datos están en una escala de 0-99 %y normalmente están alrededor de 80-86 %.

•	MSCPZTA(Float): Esta es la variable de pureza de la meladura saliendo de evaporación, estos datos están en una escala de 0-99 %y normalmente están alrededor de 80-86 %.

El archivo de PCS7 es el archivo de las variables medidas desde los medidores o sensores del proceso productivo, los datos aquí representados tienen frecuencias bastante grades y dependiendo de la variable se almacenan cada 1 o 2 segundos, el sistema maneja el formato de hora militar para el seguimiento de las variables; es de tener en encueta que cada variable aquí cuenta con dos subitem, uno de tiempo y otro de valor.

A continuación, se describe el archivo data(fecha) turno (T1 o T2 o T3):

•	Flujo cond Klb/hr (Date/Float): Esta variable representa la salida de condensados del primer efecto del tren de evaporación y se expresa en Klb/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 400 y normalmente este alrededor de 200-340 Klb/hr.

•	Flujo cana Ton/hr (Date/Float): Esta variable representa la entrada de caña del tren de molinos y se expresa en Ton/hr; la frecuencia de medida de este dato esta alrededor de cada 1 segundo, estos datos están en una escale de 0 a 600 y normalmente este alrededor de 350-550 Ton/hr.

•	Jcla C1 m3/h (Date/float): Esta variable representa la entrada de jugo a la primer calandria del tren de evaporación y se expresa en M3/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 600 y normalmente este alrededor de 150-250 Ton/hr.

•	Jcla C2 m3/h (Date/float): Esta variable representa la entrada de jugo a la segunda calandria del tren de evaporación y se expresa en M3/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 600 y normalmente este alrededor de 150-250 Ton/hr.

•	Jcla C3 m3/h (Date/float): Esta variable representa la entrada de jugo a la tercer calandria del tren de evaporación y se expresa en M3/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 600 y normalmente este alrededor de 150-250 Ton/hr.

•	Jcla C4 m3/h (Date/float): Esta variable representa la entrada de jugo a la cuarta calandria del tren de evaporación y se expresa en M3/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 600 y normalmente este alrededor de 150-250 Ton/hr.

•	Cond C1 m3/hr (Date/float): Esta variable representa la salida de condensados de la primer calandria del tren de evaporación y se expresa en M3/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 400 y normalmente este alrededor de 100-200 Ton/hr.

•	Cond C2 m3/hr (Date/float):Esta variable representa la salida de condensados de la segunda calandria del tren de evaporación y se expresa en M3/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 400 y normalmente este alrededor de 100-200 Ton/hr.

•	Cond C3 m3/hr (Date/float): Esta variable representa la salida de condensados de latercer calandria del tren de evaporación y se expresa en M3/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 400 y normalmente este alrededor de 100-200 Ton/hr.

•	Cond C4 m3/hr (Date/float): Esta variable representa la salida de condensados de la cuarta calandria del tren de evaporación y se expresa en M3/hr; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 400 y normalmente este alrededor de 100-200 Ton/hr.

•	Brix mel (Date/float): Esta variable representa la concentración de salida del tren de evaporación y se expresa en °Bx; la frecuencia de medida de este dato esta alrededor de cada 2 segundos, estos datos están en una escale de 0 a 70 y normalmente este alrededor de 60-68 °Bx.
