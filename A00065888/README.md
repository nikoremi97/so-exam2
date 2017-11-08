### Examen 2
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Estudiante:** Nicolas Recalde Miranda
**Tema:** Namespaces, CGroups, LXC
**Correo:** daniel.barragan at correo.icesi.edu.co

### Objetivos
* Comprender los fundamentos que dan origen a las tecnologías de contenedores virtuales
* Conocer y emplear funcionalidades del sistema operativo para asignar recursos a procesos
* Conocer y emplear capacidades de CentOS7 para la virtualización

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7

### Descripción
El segundo parcial del curso sistemas operativos trata sobre el manejo de namespaces, cgroups y virtualización por medio de LXC/LXD

### Solución:

## Punto 3
Para empezar a con el desarrollo del examen, verificamos que solo se utilice un nucleo del procesador.  
![][1]  
Creamos un directorio con los scripts que vamos a ejecutar posteriormente como servicio. En este caso, el directorio se llama "parcial2". Creamos dos archivos .sh que se ejecuten siempre, para esta solución, se crearon dos scripts que ejecutan lo siguiente  
```
while true; do echo HolaMundo; done.  
```
![][2]  
Ahora vamos a la carpeta /etc/systemd/system. Una vez aquí, creamos el servicio correspondiente al script creado previamente. Editamos los archivos como se muestra a continuación:  
![][3]  
Haciendo esto, le decimos al sistema operativo dónde se encuentra el script que queremos levantar como servicio y cuánta capacidad de procesamiento requiere cada uno. Queremos que cada servicio ocupe la mitad de la CPU, esto se logra con la linea 
```
CPUQuota=50%.  
```
Tenemos que verificar que no haya otro proceso corriendo:  
![][4]
El paso siguiente es levantar el servicio, lo hacemos de la siguiente manera:  
![][5]
Como vemos, los servicios que se ejecutan no usan más del 50% de capacidad de la CPU:  
![][6]  
Adicional a esto, verificamos que si un servicio se cae, el otro no ocupe el 100% de la CPU. Para esto, matamos el proceso con el comando 
```
sudo kill -9 IDPROCESS.  
```
![][7] y como vemos el proceso que sigue corriendo solo ocupa el 50% de la CPU ![][8]  

## Punto 4

Para este punto, limitaremos los servicios para que consuman 75% y 25% de la capacidad de la CPU. Esto lo haremos con  
```
CPUShares=750
CPUShares=250
```
Los servicios quedarían de la siguiente manera:  
![][9]  
Habilitamos e iniciamos los servicios tal y como lo hicimos anteriormente  
![][5]  
Verificamos que efectivamente los servicios estén consumiendo el 75% y el 25% únicamente  
![][10]  
Y en este caso, queremos que cuando un servicio se caiga, el otro pueda ocupar el 100% de la CPU, verificamos esto   
![][11]  

## Punto 5
### CPUQuota vs CPUShares
### CPUQuota
### CPUShares


## Referencias:
https://github.com/ICESI/so-processes/tree/master/03_systemd
https://docs.docker.com/engine/admin/resource_constraints/#configure-the-default-cfs-scheduler
https://www.freedesktop.org/software/systemd/man/systemd.resource-control.html

[1]: images/1core.JPG
[2]: images/procesos.JPG
[3]: images/editservice.JPG
[5]: images/startservice.JPG
[4]: images/sinprocesos.JPG
[6]: images/topCPUQuota.JPG
[7]: images/sudokill1.JPG
[8]: images/topprocesox.JPG
[9]: images/cpushares.JPG
[10]: images/topcpushares.JPG
[11]: images/100.JPG
