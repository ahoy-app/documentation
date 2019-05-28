# Costes operativos en distintos proveedores cloud

Estimar los costes operativos de un sistema así es, de entrada, imposible debido a que, al no ser una aplicación real con clientes reales, somos incapaces de estimar el volumen de tráfico o usuarios concurrentes. Además, los requerimientos del Product Owner son tan laxos (latencias de 60 segundos; ficheros de 1 MB) que prácticamente, cualquier sistema, por pequeño que sea, cubre las necesidades.

Asumiendo entonces:

- Un volumen de usuarios concurrentes bajo que no obliga a replicar nodos
- Un volumen de usuarios concurrentes bajo que no requiere máquina de altas capacidades
- Un volumen de datos saliente de menor a 5GB al mes (descargar 5000 veces ficheros de tamaño máximo)

Podemos asumir que el servicio entero puede estar distribuido en dos máquinas virtuales, una de ellas con persistencia en disco. De ese modo, reemplazaríamos en el diagrama de despliegue a Heroku por la primera máquina y a CloudAMQ y Mongo Atlas por la segunda.

Evidentemente, existiría siempre la opción de mantener los servicios en la distribución actual pagando un coste cero y ajustándose a las restricciones de los tres proveedores contratados.

## OVH

Esta primera solución es clave por su sencillez y por una cualidad específica del proveedor: No el tráfico de datos. Una empresa que conserva los modelos de hosting tradicionales donde pagas por la máquina que eliges, los discos adicionales y nada más.

### VPS

Contratando dos instancia de su [VPS SSD 3](https://www.ovh.es/vps/vps-ssd.xml) obtenemos:

- 2vCores (importante destacar que cada proceso de Node sólo consume un thread)
- 2GHz
- 8GB RAM
- 80GB SSD
- 100mbps de tráfico
- SLA del 99.95%
- Protección DDoS
- Una IP propia
- Monitoreo detallado del sistema desde el panel de cliente

Los costes serían de:
$$12eur/mes * 2vm = 24eur/mes$$

La desventaja es que la administración e instalación habrían de ser manuales como en cualquier máquina virtual.

### Dedicated Hosting + Kubernetes

Si la opción de los VPS no fuera suficientemente potente, o no nos gustara la administración manual, podríamos subir de nivel a un sistema de instancias cloud que, además de tener mejores prestaciones, permiten aprovechar el servicio de Kubernetes gratuito que ofrece OVH.

Contratando dos instancias [B2-7](https://www.ovh.es/public-cloud/instancias/precios/) obtenemos:

- 2vCores (importante destacar que cada proceso de Node sólo consume un thread)
- 2.3GHz
- 7GB RAM
- 50GB SSD
- 250mbps de tráfico
- Una IP propia + posibilidad de comprar una ip-failover con pago único de $2eur$
- Cluster Kubernetes privado con todo lo que K8S ofrece: Configuración centralizada, descubrimiento automático, enrutado interno, etc.
- Posibilidad de escalar horizontalmente añadiendo un balanceador de carga por $20eur/mes$

Los costes serían de:
$$22eur/mes * 2inst = 44eur/mes$$

## Azure Kubernetes

Si buscamos un despliegue de similares prestaciones en Azure, descubriremos que también ofrece servicio gratuito de Kubernetes donde sólo hay que pagar por la infraestructura subyacente.

Contratando dos maquinas [D2MS v3](https://azure.microsoft.com/es-es/pricing/details/virtual-machines/linux/#d-series) + un disco de sistema SSD E4 obtenemos:

- 2vCores (importante destacar que cada proceso de Node sólo consume un thread)
- 8GB RAM
- 50GB SSD temporal
- Ancho de banda no especificado. Tráfico gratuito hasta 5GB al mes (descargar 5000 veces ficheros de tamaño máximo)
- Cluster Kubernetes privado con todo lo que K8S ofrece: Configuración centralizada, descubrimiento automático, enrutado interno, etc.
- Balanceador de carga gratuito

Los costes serían de:
$$53eur/mes * 2inst + 3eur/mes = 109eur/mes$$
