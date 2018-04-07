
**TODO**:Cantidades de regiones actuales, con zonas de disponibilidad?

# Definiciones
- **Region**: Area geografica, consta de dos o mas AZ.
- **AZ (Zona disponibilidad)**: es un centro de datos.
- **Ubicacion de Borde**: Puntos finales de red de entrega de contenidos (CDN) para cloudFront.
- **AMI (Amazon Machine Image)**: Plantillas de las instancias 

# Lista de servicios

## Red
- VPC (Red Privado Virtual): Centro de datos virtual
- Direct Connet (Conexion virtual): coneccion fibra optica
- Route53: Servicio de DNS

## Computacion
- EC2: Instancias de computo
- EC2 Container Service: Contenedores docker
- Elastic Beanstalk: Paas
- Lambda: ???

## Almacenamiento
- S3: Almacen de archivos
- Cloud Front: CDN
- Glacier: resguardo backup
- EFS: almacenamiento conectado en red en la nuve
- Snowball: Mandar discos para subirlos
- Store Gateway: Maquina virtual que se replica en la nuve

## Bases de datos:
- RDS: Motores de bases de datos en la nube
- DynamoDB: base de datos propia de amazon
- Elasticache: Cache de la bases de datos
- Redshift: base datos olap
- DMS: migrar bases de datos entre motores

## Estadistica
- EMR: Procesar multiples datos
- Data Pipeline: Mover datos de un servicio a otros
- Elastic Search: busqueda de datos en los servicios de amazon
- Kinesis: tranmicion de grandes volumenes datos
- Machine Learning: api machine learning
- Quick Sight: Servicios de intelegencia de negocios

## Seguridad e Identidad
- IAM: seguridad y edentidad para los servicios
- Directory Service: 
- Inspector: Agentes en las instancias para auditorias de seguridad
- WAF: 
- Cloud HSM: seguridad de hardware
- KMS: servicio de manejo de llaves

## Herramientas de Administracion
- Cloud Watch:
- Coud Formation: 
- Cloud Trail: auditoria de acceso a los servicios de amazon
- Opsworks: Manejo de configuraciones con chef
- Config: Menejo de confiraciones de todos los servicios de amazon
- Service Catalog: Crear catalogo de servicios
- Trusted Advisor: Servicio que da sugerencias de seguridad y ahorro

## Servicios de Aplicacion
- Api Gateway: seguridad de api
- AppStream: transmitir aplicaciones de windows
- CloudSearch: Realizar búsquedas en una web
- Elastic Transcoder: Traducir contenido multimedia
- SES: servicio de email
- SQS: desacoplar infraestructura
- SWF: correr tareas en backgroud

## Herramientas de Desarrollo
- CodeCommit: Servicio de control de versiones
- CodeDeploy: Servicio que automatiza deploy
- CodePipeline: Servicio de entrega continua

## Servicios Moviles
- Mobile Hub: Para probar aplicaciones mobiles en varios dispositivos.
- Cognito: Guardar datos en la nube de las aplicaciones.
- Device Farm: pruebas calidad para diferentes dispositivos.
- Mobile Analytics: estadísticas de los aplicaciones.
- SNS: Servicio de notificacion simple.

## Aplicaciones de Empresas
- WorkSpaces: escritorios virtuales en la nuve (windows)
- WorkDocs: es como google docs
- WorkMail: Servicio de schange

## Internet de las Cosas
- Internet of Things


# Almacenamiento
## S3
- Puedo tener 100 contenedores.
- Es basado en objetos par clave valor donde la clave es el nombre y el valor es el binario.
- deben pesar entre 1byte hasta 5 TB.
- No tiene limite de almacenamiento.
- Los nombres de los contenedores deben ser únicos
- 2 modos de consistencia
	- Consistencia de lectura después de escritura para puts de nuevos objetos (La propagación puede demorar, pero la disponibilidad es inmediata).
	- Consistencia eventual para la sobreescritura de puts y deletes (los cambios pueden demorar ya que la propagacion no es inmediata).
- Tipos de S3
	- S3 (Durable, disponible inmediatamente, acceso frecuente).
	- S3-IA (durable, disponible inmediatamente, acceso esporadico).
	- S3-RRS (almacenamiento de redundancia redudica): Datos que son facilmente reproducibles pero que se pueden perder.
	- **Glacier** Datos archivados, en los cuales es aceptable una demora de 3 a 5 horas para el acceso (no puede ser publico).
- Bases fundamentales de S3:
	- Clave (Nombre).
	- Valor (datos).
	- ID de la versión.
	- Metadata.
- **Versionado**:
	- se crea un nuevo objeto por version.
	- No se borra, solo se etiqueta como borrado, para borrarlo tendria que borrar todas las versiones.
	- Cuando se habilita no se puede desestabilizar.
	- Se integra con el ciclo de vida.
	- Se puede integra con AFM.
	- Para la replicacion de region multiple, el versionado debe estar habilitado.
- **Aministracion de ciclo de vida**:
	- Puede ser usado con las versiones actuales o anteriores.
	- Puede cambiar el tipo de almacenamiento de estandar a exporadico:
		- Debe ser minimo 124kb.
		- minimo 30 dias de la fecha de creacion.
	- Se puede pasar a Glaciar.
		- 30 dias minimo despues de haber pasado a esporadico.
		- Si no se paso a glaciar, puede enviarlo al dia.


## CloudFront
- Ubicacion Limite: Es la ubicacion donde sera almacenado en cache.
- Origen: Desde donde se distribuida, puede ser S3, EC2, Balanceador de carga elastico o Ruta53.
- Distribucion: nombre dado a una coleccion de Ubicaciones Limite.
- Distribucion Web: A sitios web.
-RTMP: Utilizado para streamind de archivos multimedia.
- La ubicaciones limite no son solo lectura, tambien se pueden modificar.
- Los objetos son almacenado de acuerdo al tiempo de vida (Time to Live - TTL).
- Se puede borrar el cache pero tiene un costo.



## Asegurando Contenedores
Por defecto todos bienen Privados, se puede configurar para que sea publicos con:
- Politicas de Contenedores
- Lista de Control de Acceso (ACL)

Se puede habilitar un log del registro de acceso.

Hay dos tipos de Encriptacion

- En transito (desde o hasta el contenedor):
 	- SSL/TLS 
 
- En Repaso
	- Servidor
		- Claves aministradas por S3 (desde la interfaz): **SSE-S3**
		- Servicios de Administracion de claves (servicio estra): **SSE-KMS**
		- Encriptacion en Origen con claves Proporcionadas por el cliente: **SSE-C**
	- Encriptacion en Destino
		- Encriptados por el usuario


# Puerta de Enlace de Almacenamiento: 
Sistema de espejo mediante un sistema virtualizado, instalado en el centro de datos propio que guarda la copia en AWS, hay dos tipos: 

- Volumenes almacenados: respalda los datos asincronicamente guardados localmente(tipicamente cuando hay Problemas de conectividad y total de datos).
- Volumenes en cache: los que son requeridos mas comunes son guardado localmente y el resto en S3 (tipicamente cuando la Conectividad no es problema, se quiere ahorar en gastos).
- Libreria de Cinta Virtual: coleccion de cintas virtuales para guardar datos de sistemas de datos como NetBackup, Backup Exec, Veam, etc(cuando se quiere respaldar las cintas). 

# Importar y exportar
## Importar/exportar Disco
Copiar datos en un disco y se envía a amazon físicamente, ellos lo copian en algún sistema de almacenamiento como S3, EBS, Gracier.

- Importa hacia EBS.
- Importa hacia S3.
- Importa hacia Glaciar.
- Exporta desde S3

## Importar/exportar Bola de nieve
Hardware  que se alquila para la transferencia de datos a S3. 

#EC2 (Elastic Compute Cloud)
- Lanzar rapido.
- Escalar recursos hacia arriba o hacia abajo.
- Modelos de Precios:
 - A demandada: solo por el tiempo que se use, sin compromiso, se puede eliminar despues (Tipicamente usado mantener costos bajos, no tener un contrato, aplicaciones que no son continuas, que tienen picos de trabajo y no deben ser interrumpidas, aplicaciones solo para probar o desarrollo, picos esperados en aplicaciones continuas).
 - Reservada: por uno o por 3 años (Aplicaciones estables, con recursos esperados).
 - Subasta: se compra por una hora o mas, sin compromiso y por lo general es mucho mas barato, pero no es fijo, pueden ser terminadas por amazon (Tipicamente para mejores precios, computo corto).
- Tipos de instancia:

 - T2: Bajo Costo, Proposito General, por ejemplo para servidores web y pequeñas bases de datos.
 - M4,M3: Proposito general usadas para servidores de aplicaciones.
 - C4,C3: Computo optimizado para aplicaciones con mayor uso de cpu o bases de datos.
 - R3: Memoria optimizada para aplicaciones o bases de datos de mas uso de momoria
 - G2: Uso de tarjetas graficas o que necesiten mucho un gpu, por ejemplo aplicaciones de codificacion de video, transmision de estreming, machine learning.
 - I2: Almacenamiento de alta velocidad usadas por ejemplo para analisis de grandes bases de datos. 
 - D2: Gran almacenamiento por ejemplo data warehousing, hadoop.

# EBS (Stored Blocl Elastic)
Discos duros virtuales que permite agregarlos a las instancias, pero solo una a la ves.

Tipos de Volumenes EBS:

- Proposito General SSD (GP2):
  - 99.999% de disponibilad.
  - 3 IOPS (entrada/salida) por GB y hasta 10.000 IOPS, puede llear hasta los 3000 IOPS por periodos cortos en volumenes por debajo de 1 GB.
- IOPS aprovisionado SSD (IO1):
  - Para aplicaciones de uso intesivo de E/S, como bases de datos, usadas generalmente mas de 10.000 IOPS.
- Magnetico (Standard):
  - Menor costo.
  - Ideales para cuando no son accedidos frecuentemente, ejemplo almacenes de archivo.
  - Ideales para el costo de almecenamiento es importante.

		 









