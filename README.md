Localizador de Host por IP en Red Cisco

Este script en Python permite localizar un host dentro de una red de switches Cisco, partiendo de una dirección IP.
Utiliza conexiones SSH automatizadas con Netmiko para recorrer la topología de red en capa 2, analizando tablas ARP, MAC address y CDP neighbors, hasta identificar el puerto físico donde se conecta el dispositivo buscado.

⚙️ Funcionalidad principal

Conexión automática a dispositivos Cisco mediante SSH.

Obtención de la dirección MAC asociada a una IP (usando show ip arp).

Identificación del puerto y VLAN donde se encuentra esa MAC (show mac address-table address).

Exploración recursiva de vecinos CDP (show cdp neighbors detail) para recorrer toda la topología.

Detección automática del switch y puerto final donde está conectado el host.

🧠 Flujo general del proceso
flowchart TD
    A[Inicio] --> B[Conexión al switch raíz]
    B --> C[Buscar MAC asociada a la IP]
    C --> D[Obtener VLAN y puerto por MAC]
    D --> E{¿Puerto tiene vecino CDP?}
    E -- Sí --> F[Conectar al vecino y continuar búsqueda]
    E -- No --> G[Host encontrado en ese puerto]
    F --> C
    G --> H[Mostrar resultados]

🧩 Requisitos

Python 3.8 o superior

Librerías necesarias:

pip install netmiko


Dispositivos Cisco con acceso SSH habilitado.

Credenciales válidas para iniciar sesión (usuario y contraseña).

🚀 Ejecución

Clona o descarga el repositorio:

git clone https://github.com/tuusuario/Localizador-Host-Cisco.git
cd Localizador-Host-Cisco


Ejecuta el script:

python rastreo_host.py


Introduce los datos solicitados:

👉 Ingresa la IP del switch/router: 10.10.10.1
👉 Ingresa la IP que deseas localizar: 10.10.20.55


El script recorrerá la red y mostrará el resultado:

✅ HOST FINAL DETECTADO EN SW_ACC01:
   🔌 Puerto: Gi1/0/24
   🆔 VLAN: 20
   💻 MAC: 00a1.b2c3.d4e5
   🌐 IP: 10.10.20.55

📄 Descripción técnica

El script utiliza Netmiko para conectarse a dispositivos Cisco IOS y ejecutar comandos de diagnóstico de red.
Su comportamiento se basa en un rastreo recursivo, de modo que si un puerto conectado a la MAC buscada pertenece a otro switch, el script salta automáticamente al siguiente equipo usando la información de CDP neighbors, repitiendo el proceso hasta encontrar el host final.

Principales funciones:
Función	Descripción
conectar()	Establece conexión SSH con el dispositivo.
obtener_mac_por_ip()	Busca la MAC asociada a la IP objetivo en la tabla ARP.
obtener_puerto_por_mac()	Identifica el puerto y VLAN donde se encuentra la MAC.
obtener_vecinos_cdp()	Obtiene la lista de vecinos conectados mediante CDP.
rastrear_host()	Recorre la red de manera recursiva hasta encontrar el host final.
🛠️ Configuración personalizable

Puedes editar las variables al inicio del script según tus credenciales o tiempos de espera:

usuario = "cisco"
password = "cisco99"

TIMEOUT_CONN = 2     # Tiempo máximo al conectar (segundos)
READ_TIMEOUT = 4     # Tiempo máximo para leer salida

⚠️ Notas importantes

El usuario debe tener privilegios de modo enable en los dispositivos.

Se recomienda que CDP esté habilitado en los switches para un rastreo correcto.

Si el host no se encuentra, el script mostrará:

❌ No se encontró la IP en la topología accesible desde el switch/router raíz proporcionado.

👨‍💻 Autor

Ever Contreras
📧 Contacto: (agrega tu correo o GitHub si deseas)
🛠️ Versión: 1.0
📅 Última actualización: Noviembre 2025# conectividad
