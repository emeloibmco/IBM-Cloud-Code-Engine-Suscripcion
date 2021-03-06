# IBM Cloud Code Engine Suscripcion :alarm_clock:

*Code Engine* permite que sus aplicaciones o trabajos reciban eventos de inter茅s suscribi茅ndose a productores de eventos. La informaci贸n de eventos se recibe como solicitudes POST HTTP para aplicaciones. El productor de eventos utilizado en esta gu铆a es el productor de eventos cron, el cual genera un evento a intervalos regulares. 

## Tabla de contenido 馃搼

1. [Requisitos](#Requisitos-newspaper)
2. [Crear aplicaci贸n cronoapp](#Crear-aplicaci贸n-cronoapp-hourglass)
3. [Crear la suscripci贸n](#Crear-la-suscripci贸n-paperclip)
4. [Probar la suscripci贸n](#Probar-la-suscripci贸n-arrow_forward)
5. [Referencias](#Referencias-mag)
6. [Autores](#Autores-black_nib)

## Requisitos :newspaper:
- Contar con el servicio de Code Engine y tener un [proyecto](https://cloud.ibm.com/codeengine/create/project)
- Configure su entorno [CLI de Code Engine](https://cloud.ibm.com/docs/codeengine?topic=codeengine-install-cli) o utilice [IBM Cloud Shell](https://cloud.ibm.com/shell)


## Crear aplicaci贸n cronoapp :hourglass:
La aplicaci贸n cronoapp registrar谩 cada evento a medida que llegue, mostrando el conjunto completo de encabezados HTTP y la carga 煤til del cuerpo HTTP. Esta aplicaci贸n extrae una imagen que se llama cron que est谩 disponible en Docker Hub p煤blico y siempre tiene al menos una instancia en ejecuci贸n. Para crear la aplicaci贸n a traves de *IBM Cloud Shell*, siga los pasos mostrados a continuaci贸n:
1. Asegurese de estar en el grupo de recursos donde se encuentra la instancia de Code Engine:
```
ibmcloud target -g "<resource-group>"
```

2. Conectese a su proyecto de Code Engine:
```
ibmcloud ce project select -n <project>
```

3. Cree la aplicaci贸n:
```
ibmcloud ce app create --name cron-app --image ibmcom/cron --min-scale=1
```
4. Verifique que su aplicaci贸n esta en estado Ready:
```
ibmcloud ce application get --name cron-app
```
</br>

Por otro lado, si desea realizarlo desde la consola de IBM, siga los pasos mostrados a continuaci贸n:

1. Dirijase a [proyectos](https://cloud.ibm.com/codeengine/projects) de Code Engine y seleccione el proyecto donde quiere desplegar la aplicaci贸n.
2. Del men煤 de la izquierda, de click en la opci贸n ```Applications``` y de click en ```Crear```. En la nueva ventana desplegada, complete lo siguiente:
* ```Name```: Elija un nombre 煤nico para la aplicaci贸n.
* ``` Choose the code to run```: Container image.
* ```Image Reference ```: ibmcom/cron
* ```Listening port override```: Dejar vacio
* ```Runtime settings```>```Number of instances```: Minimo de instancias en 1.

 Finalmente de click en ```Create```.
 
  <p align=center><img width="950" src=".github/appcrono.gif"></p>
 <br />

## Crear la suscripci贸n :paperclip:
Una vez creada la aplicaci贸n puede pasar a crear la suscripci贸n, esto se puede realizar mediante la consola de *IBM Cloud* o mediante el *IBM Cloud Shell*, estos dos procesos se explicar谩n a continuaci贸n.

### Opci贸n 1 consola *IBM Cloud*
Para crear la suscripci贸n mediante esta opci贸n tenga en cuenta los siguientes pasos:

1. Desde el Dashboard de *IBM Cloud* seleccione el men煤 de navegaci贸n o men煤 de hamburguesa y de click sobre la opci贸n de ```Code Engine```
2. Esto lo llevara a la ventana principal de Code Engine, aqu铆 de click sobre proyectos y seleccione el proyecto en donde desplego la aplicaci贸n creada anteriormente.
3. En la ventana del proyecto de click sobre ```Suscripciones a eventos/Event subscriptions``` y de click sobre ```Crear```.
4. Esto lo llevara a la ventana de configuraci贸n para la suscripci贸n a eventos, aqu铆 complete la informaci贸n necesaria de la siguiente manera:
 * ```Name```: Elija un nombre 煤nico para la suscripci贸n
 * ```Event attributes (optional)```: En esta casilla puede adicionar pares clave-valor los cuales son asignados a cada evento y se convierten en metadatos que le permitir谩n comprender r谩pidamente los eventos realizados.
 * ```Day pattern```: Seleccione el patr贸n diario con el cual quiere que se repitan los eventos generados por la suscripci贸n.
 * ```Hour pattern```: Seleccione el patr贸n horario con el cual quiere que se repitan los eventos generados por la suscripci贸n.
 * ```Minute pattern```: Seleccione el patr贸n horario con el cual quiere que se repitan los eventos generados por la suscripci贸n.
 * ```Custom event data (Optional)```: En este espacio puede proporcionar datos para que sean incluidos en el cuerpo del mensaje que se generar con su evento (se pueden seleccionar distintos formatos para los datos).
 * ```Component type```: Seleccione aplicaci贸n ya que la suscripci贸n a eventos se realizar谩 sobre la aplicaci贸n creada anteriormente.
 * ```Name```: Seleccione el nombre de la aplicaci贸n creada anteriormente.
 * ```Path```; Seleccione la versi贸n de la aplicaci贸n (Si es necesario).
 * Finalmente de click en crear.

 <p align=center><img width="950" src=".github/consola.gif"></p>
 <br />


### Opci贸n 2 *IBM Cloud Shell*
 
Para crear la suscripci贸n mediante el *IBM CLoud Shell* primero debe entender y determinar un intervalo de tiempo para cron, el productor de eventos cron genera eventos a intervalos regulares los cuales se pueden programar por minutos, horas, d铆as, meses o una combinaci贸n de todos estos como se mostro en la opci贸n 1.

En el caso de *IBM Cloud Shell* se utiliza el formato ```* * * * *```que significa minuto, hora, d铆a del mes y d铆a de la semana. Por ejemplo, para programar un evento a medianoche se utilizar铆a ```0 0 * * *``` y para programar un evento para cada viernes a medianoche seria ```0 0 * * FRI```.

Una vez determinado el intervalo de tiempo y creada la aplicaci贸n se puede pasar a crear la suscripci贸n, para esto se utiliza el comando ```ibmcloud ce sub cron create```a continuaci贸n se muestra un ejemplo de este comando.
```
 ibmcloud ce sub cron create --name cron-sub --destination cron-app --data '{"mydata":"hello world"}' --schedule '* * * * *'
```
En este caso se creo una suscripci贸n de nombre ```cron-sub```en la aplicacion ```cron-app``` creada anteriormente.  Tambi茅n se a帽adieron datos en formato JSON los cuales ser谩n desplegados como mensaje cuando ocurra un evento y se programo con el intervalo de tiempo ```* * * * *``` el cual permite mandar un evento cada minuto de cada d铆a a la aplicaci贸n ```cron-app```.

Para obtener mas informaci贸n sobre la suscripci贸n utilice el comando
```
ibmcloud ce sub cron get -n cron-sub
```
 <p align=center><img width="950" src=".github/shell.png"></p>
 <br />
 
 
## Probar la suscripci贸n :arrow_forward:
Luego de haber creado la suscripci贸n ya sea mediante la opci贸n 1 o la opci贸n 2 se pueden mirar los registros para comprobar su adecuado funcionamiento, para esto utilice el siguiente comando.
```
ibmcloud ce app logs --name <NOMBRE_APP>
```
>Nota: `Reemplace el campo de <NOMBRE_APP> con el nombre de la aplicaci贸n, en este caso es cron-app`

Este comando le deber谩 otorgar la siguiente informaci贸n

 <p align=center><img width="950" src=".github/prueba.png"></p>
 <br />


## Referencias :mag:

- [Subscribing to cron events](https://cloud.ibm.com/docs/codeengine?topic=codeengine-subscribe-cron-tutorial)
<br />

## Autores :black_nib:
Equipo *IBM Cloud Tech Sales Colombia*.
