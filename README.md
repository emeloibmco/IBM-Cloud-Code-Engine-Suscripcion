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
Una vez creada la aplicacion puede pasar a crear la suscripcion, esto se puede realizar mediante la consola de *IBM Cloud* o mediante el *IBM Cloud Shell*, estos dos procesos se explicaran a continuacion.

### Opcion 1 consola *IBM Cloud*
Para crear la suscripcion mediante esta opcion tenga en cuenta los siguientes pasos:

1. Desde el Dashboard de *IBM Cloud* seleccione el menu de navegacion o menu de hamburguesa y de click sobre la opcion de '''Code Engine'''
2. Esto lo llevara a la ventana principal de Code Engine, aqui de click sobre projectos y seleccione el proyecto en donde desplego la aplicacion creada anteriormente.
3. En la ventana del proyecto de click sobre '''Suscripciones a eventos/Event subscriptions''' y de click sobre '''Crear'''.
4. Esto lo llevara a la ventana de configuracion para la suscripcion a eventos, aqui complete la informacion necesaria de la siguiente manera:
 * ```Name```: Elija un nombre unico para la suscripcion
 * ```Event attributes (optional)```:En esta casilla puede adicionar pares clave-valor los cuales son asignados a cada evento y se convierten en metadatos que le permitiran comprender rapidamente los eventos realizados.
 * ```Day pattern```: Seleccione el patron diario con el cual quiere que se repitan los eventos generados por la suscripcion.
 * 
 
   




## Requisitos :newspaper:
- Contar con el servicio de Code Engine y tener un [proyecto](https://cloud.ibm.com/codeengine/create/project)
- Configure su entorno [CLI de Code Engine](https://cloud.ibm.com/docs/codeengine?topic=codeengine-install-cli) o utilice [IBM Cloud Shell](https://cloud.ibm.com/shell)

## Referencias :mag:

- [Subscribing to cron events](https://cloud.ibm.com/docs/codeengine?topic=codeengine-subscribe-cron-tutorial)
<br />

## Autores :black_nib:
Equipo *IBM Cloud Tech Sales Colombia*.
