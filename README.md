# IBM Cloud Code Engine Suscripcion :alarm_clock:

*Code Engine* permite que sus aplicaciones o trabajos reciban eventos de interés suscribiéndose a productores de eventos. La información de eventos se recibe como solicitudes POST HTTP para aplicaciones. El productor de eventos utilizado en esta guía es el productor de eventos cron, el cual genera un evento a intervalos regulares. 

## Tabla de contenido 📑

1. [Requisitos](#Requisitos-newspaper)
2. [Crear aplicación cronoapp](#Crear-aplicación-cronoapp-hourglass)
3. [Crear la suscripción](#Crear-la-suscripción)
4. [Probar la suscripción](#Probar-la-suscripción)
5. [Referencias](#Referencias-mag)
6. [Autores](#Autores-black_nib)

## Crear aplicación cronoapp :hourglass:
La aplicación cronoapp registrará cada evento a medida que llegue, mostrando el conjunto completo de encabezados HTTP y la carga útil del cuerpo HTTP. Esta aplicación extrae una imagen que se llama cron que está disponible en Docker Hub público y siempre tiene al menos una instancia en ejecución. Para crear la aplicación a traves de *IBM Cloud Shell*, siga los pasos mostrados a continuación:
1. Asegurese de estar en el grupo de recursos donde se encuentra la instancia de Code Engine:
```
ibmcloud target -g "<resource-group>"
```

2. Conectese a su proyecto de Code Engine:
```
ibmcloud ce project select -n <project>
```

3. Cree la aplicación:
```
ibmcloud ce app create --name cron-app --image ibmcom/cron --min-scale=1
```
4. Verifique que su aplicación esta en estado Ready:
```
ibmcloud ce application get --name cron-app
```
</br>

Por otro lado, si desea realizarlo desde la consola de IBM, siga los pasos mostrados a continuación:

1. Dirijase a [proyectos](https://cloud.ibm.com/codeengine/projects) de Code Engine y seleccione el proyecto donde quiere desplegar la aplicación.
2. Del menú de la izquierda, de click en la opción ```Applications``` y de click en ```Crear```. En la nueva ventana desplegada, complete lo siguiente:
* ```Name```: Elija un nombre único para la aplicación.
* ``` Choose the code to run```: Container image.
* ```Image Reference ```: ibmcom/cron
* ```Listening port override```: Dejar vacio
* ```Runtime settings```>```Number of instances```: Minimo de instancias en 1.

 Finalmente de click en ```Create```.
 
  <p align=center><img width="950" src=".github/appcrono.gif"></p>
 <br />

## Crear la suscripción
Una vez creada la aplicación puede pasar a crear la suscripción, esto se puede realizar mediante la consola de *IBM Cloud* o mediante el *IBM Cloud Shell*, estos dos procesos se explicarán a continuación.

### Opción 1 consola *IBM Cloud*
Para crear la suscripción mediante esta opción tenga en cuenta los siguientes pasos:

1. Desde el Dashboard de *IBM Cloud* seleccione el menú de navegación o menú de hamburguesa y de click sobre la opción de ```Code Engine```
2. Esto lo llevara a la ventana principal de Code Engine, aquí de click sobre proyectos y seleccione el proyecto en donde desplego la aplicación creada anteriormente.
3. En la ventana del proyecto de click sobre ```Suscripciones a eventos/Event subscriptions``` y de click sobre ```Crear```.
4. Esto lo llevara a la ventana de configuración para la suscripción a eventos, aquí complete la información necesaria de la siguiente manera:
 * ```Name```: Elija un nombre único para la suscripción
 * ```Event attributes (optional)```: En esta casilla puede adicionar pares clave-valor los cuales son asignados a cada evento y se convierten en metadatos que le permitirán comprender rápidamente los eventos realizados.
 * ```Day pattern```: Seleccione el patrón diario con el cual quiere que se repitan los eventos generados por la suscripción.
 * ```Hour pattern```: Seleccione el patrón horario con el cual quiere que se repitan los eventos generados por la suscripción.
 * ```Minute pattern```: Seleccione el patrón horario con el cual quiere que se repitan los eventos generados por la suscripción.
 * ```Custom event data (Optional)```: En este espacio puede proporcionar datos para que sean incluidos en el cuerpo del mensaje que se generar con su evento (se pueden seleccionar distintos formatos para los datos).
 * ```Component type```: Seleccione aplicación ya que la suscripción a eventos se realizará sobre la aplicación creada anteriormente.
 * ```Name```: Seleccione el nombre de la aplicación creada anteriormente.
 * ```Path```; Seleccione la versión de la aplicación (Si es necesario).
 * Finalmente de click en crear.

 <p align=center><img width="950" src=".github/consola.gif"></p>
 <br />


### Opción 2 *IBM Cloud Shell*
 
Para crear la suscripción mediante el *IBM CLoud Shell* primero debe entender y determinar un intervalo de tiempo para cron, el productor de eventos cron genera eventos a intervalos regulares los cuales se pueden programar por minutos, horas, días, meses o una combinación de todos estos como se mostro en la opción 1.

En el caso de *IBM Cloud Shell* se utiliza el formato ```* * * * *```que significa minuto, hora, día del mes y día de la semana. Por ejemplo, para programar un evento a medianoche se utilizaría ```0 0 * * *``` y para programar un evento para cada viernes a medianoche seria ```0 0 * * FRI```.

Una vez determinado el intervalo de tiempo y creada la aplicación se puede pasar a crear la suscripción, para esto se utiliza el comando ```ibmcloud ce sub cron create```a continuación se muestra un ejemplo de este comando.
```
 ibmcloud ce sub cron create --name cron-sub --destination cron-app --data '{"mydata":"hello world"}' --schedule '* * * * *'
```
En este caso se creo una suscripción de nombre ```cron-sub```en la aplicacion ```cron-app``` creada anteriormente.  También se añadieron datos en formato JSON los cuales serán desplegados como mensaje cuando ocurra un evento y se programo con el intervalo de tiempo ```* * * * *``` el cual permite mandar un evento cada minuto de cada día a la aplicación ```cron-app```.

Para obtener mas información sobre la suscripción utilice el comando
```
ibmcloud ce sub cron get -n cron-sub
```
 <p align=center><img width="950" src=".github/shell.png"></p>
 <br />
 
 
## Probar la suscripción
Luego de haber creado la suscripción ya sea mediante la opción 1 o la opción 2 se pueden mirar los registros para comprobar su adecuado funcionamiento, para esto utilice el siguiente comando.
```
ibmcloud ce app logs --name <NOMBRE_APP>
```
>Nota: `Reemplace el campo de <NOMBRE_APP> con el nombre de la aplicación, en este caso es cron-app`

Este comando le deberá otorgar la siguiente información




## Requisitos :newspaper:
- Contar con el servicio de Code Engine y tener un [proyecto](https://cloud.ibm.com/codeengine/create/project)
- Configure su entorno [CLI de Code Engine](https://cloud.ibm.com/docs/codeengine?topic=codeengine-install-cli) o utilice [IBM Cloud Shell](https://cloud.ibm.com/shell)

## Referencias :mag:

- [Subscribing to cron events](https://cloud.ibm.com/docs/codeengine?topic=codeengine-subscribe-cron-tutorial)
<br />

## Autores :black_nib:
Equipo *IBM Cloud Tech Sales Colombia*.
