
**TODO**:Cantidades de regiones actuales, con zonas de disponibilidad?

# Definiciones
**Region**: Area geografica, consta de dos o mas AZ.
**AZ (Zona disponibilidad)**: es un centro de datos.
**Ubicacion de Borde**: Puntos finales de red de entrega de contenidos (CDN) para cloudFront

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
- Mobile Hub: Para probar aplicaciones mobiles en varios dispositivos
- Cognito: Guardar datos en la nube de las aplicaciones
- Device Farm: pruebas calidad para diferentes dispositivos
- Mobile Analytics: estadísticas de los aplicaciones
- SNS: Servicio de notificacion simple

## Aplicaciones de Empresas
- WorkSpaces: escritorios virtuales en la nuve (windows)
- WorkDocs: es como google docs
- WorkMail: Servicio de schange

##Internet de las Cosas
- Internet of Things


# Almacenamiento
## S3
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
	- S3- ARR (almacenamiento de redundancia redudica): Datos que son facilmente reproducibles pero que se pueden perder.
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
 	-- SSL/TLS 
 
- En Repaso
	--Servidor
		- Claves aministradas por S3 (desde la interfaz): SSE-S3
		- Servicios de Administracion de claves (servicio estra): SSE-KMS
		- Encriptacion en Origen con claves Proporcionadas por el cliente: SSE-C
	--Encriptacion en Destino
		- Encriptados por el usuario


# Puerta de Enlace de Almacenamiento: 
Sistema de espejo mediante un sistema virtualizado, instalado en el centro de datos propio que guarda la copia en AWS, hay dos tipos:
- Volumenes almacenados: respalda los datos asincronicamente guardados localmente.
- Volumenes en cache: los que son requeridos mas comunes son guardado localmente y el resto en S3.
- Libreria de Cinta Virtual: coleccion de cintas virtuales para guardar datos de sistemas de datos como NetBackup, Backup Exec, Veam, etc. 

# Importar y exportar
## Importar/exportar Disco
Copiar datos en un disco y se envía a amazon físicamente, ellos lo copian en algún sistema de almacenamiento como S3, EBS, Gracier.
## Importar/exportar Bola de nieve
Hardware  que se alquila para la transferencia de datos a S3 
		 









