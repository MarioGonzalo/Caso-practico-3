# Caso-practico-3

Link al repositorio: https://github.com/MarioGonzalo/Caso-practico-3.git

## Paso 1: Diseño Integral de la Red y Modelado de Comunicación

Para transformar el campus universitario en una red inteligente, se debe diseñar una infraestructura que contemple las diferentes funciones y usuarios. A continuación se definen las áreas clave de la red:

**1. Área IoT (Internet de las Cosas)
Función: Recolectar y transmitir datos desde sensores y dispositivos inteligentes.**

Componentes:

- Sensores ambientales (temperatura, humedad, calidad del aire, luminosidad, etc.)

- Cámaras IP para videovigilancia

- Sensores de presencia o movimiento

- Controladores de iluminación y climatización inteligente

- Dispositivos de control de acceso (por ejemplo, lectores RFID)

Requisitos técnicos:

- Baja latencia

- Alta disponibilidad

- Conectividad segura (VPN, VLANs específicas, protocolos como MQTT sobre TLS)

- Administración centralizada de dispositivos

**2. Área de Usuarios (Estudiantes y Profesores)
Función: Proveer acceso a internet, recursos académicos, aplicaciones institucionales y colaboración en línea.**

Componentes:

- PCs en laboratorios, bibliotecas y aulas

- Acceso a redes WiFi y cableadas

- Plataformas LMS (Learning Management Systems), correo institucional, herramientas de videoconferencia

Requisitos técnicos:

- Conexiones escalables (WiFi de alta densidad: WiFi 6 o superior)

- Control de acceso basado en identidad

- QoS para tráfico académico y videoconferencias

- Segmentación por perfiles (estudiantes, docentes, invitados)

**3. Área de Servicios Multimedia
Función: Distribuir contenidos digitales en tiempo real y bajo demanda.**

Componentes:

- Streaming en vivo de clases o eventos (aulas híbridas)

- Servidores de video y contenido multimedia

- Plataformas de transmisión (YouTube privado, streaming interno)

Requisitos técnicos:

- Alta capacidad de ancho de banda

- Multicast y QoS para flujos de video

- Servidores de medios con almacenamiento redundante

- Redes troncales de alto rendimiento

**4. Área de Administración y Gestión
Función: Controlar, monitorear y asegurar la red y los servicios de TI del campus.**

Componentes:

- Sistemas de monitoreo de red (NMS, SIEM)

- Consolas de gestión de dispositivos y seguridad

- Infraestructura de respaldo y virtualización

Requisitos técnicos:

- Segmentación rígida de red (VLANs aisladas)

- Altos estándares de seguridad (firewalls, IDS/IPS, control de acceso)

- Acceso restringido al personal autorizado

Alta disponibilidad y tolerancia a fallos

## Paso 2: Capa Física – Capacidad, Modulación y Eficiencia

Para calcular la capacidad necesaria en los enlaces cableados e inalámbricos del campus emplearemos la fórmula de Shannon:

$$ C = B \times log_2(1+SNR)$$

Donde:


- C: Capacidad máxima del canal (bps)

- B: Ancho de banda del canal (Hz)

- SNR: Relación señal a ruido (unitaria, no en decibelios)

Para convertir SNR en dB a forma unitaria emplearemos la fórmula:

$$𝑆𝑁𝑅_{lineal} = 10 𝑙𝑜𝑔_{10}(𝑆𝑁𝑅) = 10^{\frac{SNR}{10}} [dB]$$



Dado que es necesario el streaming y la tranferencia de archivos grandes se recomienda un SNR de al menos 30.  
$$SNR_{lineal}= 10^{\frac{30}{10}} = 10^3 = 1000$$

- **Ahora para los distintos cables**

1. *Cobre*

    Cable de cobre categoria 6 con ancho de banda de 250 Mhz. Entre switches y access points.  

    $$C = 2,5 \times 10^8 \times log_2(1+1000) = 2,5 \times 10^8 \times log_2(1001) = 2,5 \times 10^8 \times 9.967 = 2.49 \times 10^9bps = 2.49 Gbps$$


    Dado que los access points limitan la transmision a 100 Mbps al switch debido a la conexion del puerto todo esos calculos antes son teoricos y la velocidad verdadera es 100Mbps

2. *Resto de red cableada*  

    Cable  de un Ghz de ancho de banda para mayor trafico.

    $$C = 1\times 10^9 \times log_2(1001) =  1 \times 10^9 \times 9.967 = 9,96710^9 bps = 9.97 Gbps$$

3. *Inalambricos*

    Para los enlaces inalámbricos emplearemos una SNR diferente, de 20 db.

    $$SNR_{lineal}= 10^{\frac{20}{10}} = 10^2 = 100$$

    También usaremos un ancho de banda de 80MHz y aplicamos la fórmula de Shannon

    $$C = 8 \times 10^7 \times log_2(1+100) =  8 \times 10^7 \times log_2(101) = 8 \times 10^7 \times 6.66 = 3,33 \times 10^8bps = 533 Mbps$$

4. *USB*

    USB 3.0 con un ancho de banda de 500 MHz y una SNR aproximada de 20 dB, equivalente a 100 en escala lineal:

    $$SNR_{lineal}= 10^{\frac{20}{10}} = 10^2 = 100$$

    $$C = 5 \times 10^8 \times log_2(1+100) =  5 \times 10^8 \times log_2(101) = 5 \times 10^ \times 6.66 = 5,328 \times 10^9bps = 33 Gbps$$

## Paso 3: Capa de Red

### Esquema del direccionamiento

- La red interna es la 192.168.1.0/vlan1:
    - *Rango de Hosts:* 192.168.1.2 – 192.168.1.254
    - *Default gateway:* 192.168.1.1
    - *Broadcast:* 192.168.1.256
    - *Total de Hosts Útiles:* 254

- La red interna es la 192.168.2.0/vlan1:
    - *Rango de Hosts:* 192.168.2.2 – 192.168.2.254
    - *Default gateway:* 192.168.2.1
    - *Broadcast:* 192.168.2.256
    - *Total de Hosts Útiles:* 254  

    Toda la red interna y las vlans reciben sus ips a base de portocolo DHCP

- La red externa es la 200.1.1.0/vlan1:
    - *Rango de Hosts:* 200.1.1.2 – 200.1.1.254
    - *Default gateway:* 200.1.1.1
    - *Broadcast:* 200.1.1.256
    - *Total de Hosts Útiles:* 254

Aparte la red se divide en multiple vlans cada una con su propia IP, gateway y broadcast. 

| VLAN  | Descripción             | Red IP             | Gateway           | Broadcast           |
|-----|----------|----------|--------------------|----------------------|
| 100   | vlan 100     | 192.168.100.0/24   | 192.168.100.1      | 192.168.100.255      |
| 200      |  vlan 200      | 192.168.20.0/24   | 192.168.20.1      | 192.168.20.255      |
| 300      | vlan 300    | 192.168.30.0/24    | 192.168.30.1       | 192.168.30.255       |
| 600      | vlan 400    | 192.168.40.0/24    | 192.168.40.1       | 192.168.30.255       |
| 500      | vlan 500    | 192.168.50.0/24    | 192.168.50.1       | 192.168.30.255       |
| 500      | vlan 600    | 192.168.60.0/24    | 192.168.60.1       | 192.168.30.255       |


Para cosas como el servidor de DHCP:
#### VLAN 10 – 192.168.200.0/24

- *Rango de Hosts:* 192.168.200.2 – 192.168.200.254
- *DDefault gateway:* 192.168.200.1
- *Broadcast:* 192.168.200.256
- *Total de Hosts Útiles:* 254


Cada VLAN tiene su propia subred /24 (máscara de subred 255.255.255.0), lo cual permite hasta 254 hosts por segmento, ideal para entornos académicos de tamaño medio.

#### VLAN 100 – 192.168.10.0/24

- *Rango de Hosts:* 192.168.10.2 – 192.168.10.254
- *DDefault gateway:* 192.168.10.1
- *Broadcast:* 192.168.10.256
- *Total de Hosts Útiles:* 254

#### VLAN 200 – 192.168.20.0/24

- *Rango de Hosts:* 192.168.20.2 – 192.168.200.254
- *Default gateway:* 192.168.20.1
- *Broadcast:* 192.168.20.256
- *Total de Hosts Útiles:* 254

#### VLAN 300 – 192.168.30.0/24

- *Rango de Hosts:* 192.168.30.2 – 192.168.30.254
- *DDefault gateway:* 192.168.30.1
- *Broadcast:* 192.168.30.256
- *Total de Hosts Útiles:* 254

#### VLAN 400 – 192.168.40.0/24

- *Rango de Hosts:* 192.168.40.2 – 192.168.40.254
- *DDefault gateway:* 192.168.40.1
- *Broadcast:* 192.168.40.256
- *Total de Hosts Útiles:* 254

#### VLAN 500 – 192.168.50.0/24

- *Rango de Hosts:* 192.168.50.2 – 192.168.50.254
- *DDefault gateway:* 192.168.50.1
- *Broadcast:* 192.168.50.256
- *Total de Hosts Útiles:* 254

#### VLAN 600 – 192.168.60.0/24

- *Rango de Hosts:* 192.168.60.2 – 192.168.60.254
- *DDefault gateway:* 192.168.60.1
- *Broadcast:* 192.168.60.256
- *Total de Hosts Útiles:* 254

  La vlan 600 tambien se ocupa del streaming

---
## Paso 4: Capa de Transporte – Selección de Protocolos

### 1. Criterios de Selección

En el diseño de una red académica, la elección del protocolo de transporte adecuado es crucial para asegurar un rendimiento eficiente y una experiencia fluida para los usuarios. Se define el uso de protocolos basados en la *naturaleza del servicio*:

- *TCP (Transmission Control Protocol):*  
  Se usará para *servicios críticos* que requieran *fiabilidad, control de errores y orden* en la entrega de datos.

- *UDP (User Datagram Protocol):*  
  Se usará para *servicios de transmisión en tiempo real* donde *la velocidad es más importante que la fiabilidad*, tolerando cierta pérdida de paquetes.

---

#### Protocolos basados en tipo de tráfico

| Servicio / Aplicación | Protocolo usado | Justificación breve |
|:----------------------|:----------------|:--------------------|
| Acceso a sistemas administrativos (ERP, Intranet, CRM) | *TCP* | Requiere fiabilidad total en el transporte de datos. |
| Transferencia de archivos (FTP, SFTP, SCP) | *TCP* | Necesita transmisión íntegra y segura de archivos. |
| Correo electrónico (SMTP, IMAP, POP3) | *TCP* | Garantiza entrega completa y ordenada de mensajes. |
| Navegación web (HTTP/HTTPS) | *TCP* | Necesita fiabilidad en la entrega de páginas y datos. |
| Cámaras IP de seguridad (streaming) | *UDP* | Prioriza la inmediatez de video frente a errores menores. |
| Telefonía IP (VoIP) | *UDP* | Baja latencia es más crítica que la corrección de errores. |
| Sensores IoT (telemetría continua) | *UDP* | El envío constante de datos en tiempo real es prioritario. |
| Videoconferencia en vivo (WebRTC, Zoom, etc.) | *UDP* | Necesita fluidez en tiempo real, aceptando posible pérdida mínima. |


Para aplicaciones que implican transferencia de archivos, como documentos, proyectos o recursos digitales, se recomienda el uso del protocolo TCP.
TCP ofrece características esenciales como el control de flujo, confirmación de entrega, corrección de errores y retransmisión de paquetes perdidos, garantizando así la integridad de los archivos transferidos. Aunque introduce una ligera latencia, esta es aceptable en aplicaciones donde la precisión y la fiabilidad son prioritarias.

Por otro lado, para servicios de streaming y transmisión en tiempo real, como video en vivo o telefonía IP, se optará por el protocolo UDP.
A diferencia de TCP, UDP no requiere confirmaciones ni mantiene el estado de la conexión, lo que reduce significativamente la latencia. Aunque puede haber pequeñas pérdidas de información, estas no afectan de forma significativa la calidad general del servicio, priorizando así la continuidad y la velocidad del flujo de datos.


### Cálculo de la Ventana:

Para optimizar la transferencia de archivos en la red mediante el protocolo *TCP, es importante calcular el **tamaño óptimo de la ventana de transmisión*. Esta ventana determina la cantidad de datos que pueden enviarse antes de esperar una confirmación del receptor (ACK), permitiendo aprovechar el canal al máximo.


#### Parámetros medidos:
Utilizando un ping de un ordenador a otro de ka red se obtienen estos valores

- *RTT promedio observado en la simulación:* 1 ms
- *RTT convertido a segundos:* RTT = 1 ms = 0.001 segundos
- *Ancho de banda simulado:* 100 Mbps

####  Cálculo:

Se aplica la fórmula:


$\text{Ventana (bytes)} = \frac{Ancho de Banda (bps) \times RTT (seg)}{8}$



$$\text{Ventana (bytes)} = \frac{100,000,000 \times 0.001}{8} = \frac{100,000}{8} = 12,500 \text{ bytes}$$

Luego, calculamos cuántos segmentos TCP de tamaño estándar (MSS) se pueden enviar:

$\text{Cálculo de segmentos TCP (MSS = 1460 bytes) }$

$$
\text{Segmentos} = \frac{12,500}{1460} \approx 8 \text{ segmentos}
$$

#### Resultado:

El tamaño óptimo de la ventana de transmisión TCP en los enlaces simulados es de aproximadamente *12,500 bytes, lo que equivale a unos **8 segmentos TCP* de 1460 bytes. Este tamaño garantiza una transmisión eficiente, minimizando esperas por confirmaciones y aprovechando al máximo la capacidad del canal.

## Paso 5: Capa de Aplicación y Multimedia – Servicios y Resolución de Nombres

Diseño de Servicios basados en HTTP/HTTPS

**Portal del Campus Universitario**

Proporciona acceso unificado a:

- Información institucional y académica

- Servicios de inscripción, consulta de horarios y notas

- Correo institucional

- Plataforma LMS (ej. Moodle)

- Noticias y eventos

**Distribución de Recursos Multimedia**

Servicios proporcionados:

- Streaming en vivo de clases o conferencias

- Video bajo demanda (VoD)

- Señalización digital (paneles informativos)

**Proceso de Resolución de Nombres (DNS)**

La resolución de nombres traduce dominios (ej. campus.edu) en direcciones IP. Se seguirán estos pasos:

- Usuario accede a: https://aulas.campus.edu

- Cliente (navegador) consulta el servidor DNS local del campus.

- Si el nombre no está en caché, el servidor local consulta a un servidor raíz, luego al TLD (.edu), y finalmente al servidor autoritativo de campus.edu.

- Se devuelve la dirección IP del servidor web correspondiente.

- El navegador se conecta a esa IP a través de HTTPS.

**Autenticación de Usuarios**

Requisitos:

- Identificar a estudiantes, docentes, personal administrativo

- Evitar múltiples inicios de sesión para cada servicio

- Soportar acceso remoto y federado

Para ello emplearemos SAML SSO que seguirá estos pasos:

- Usuario accede al portal del campus (ej. https://portal.campus.edu).

- El portal redirige al Identity Provider (IdP), por ejemplo, un servidor SAML configurado con Active Directory o LDAP.

- El usuario ingresa sus credenciales (una sola vez).

- El IdP autentica al usuario y devuelve un token SAML firmado al portal.

- El portal verifica el token y permite el acceso.

- Cualquier otro servicio (LMS, streaming, biblioteca) que soporte SAML confía en esa sesión y lo deja pasar sin pedir credenciales.


## Técnicas de Streaming y su Adaptabilidad

### 1. UDP Streaming (Transmisión en Tiempo Real)

**Características:**
- Basado en protocolo UDP, sin control de errores ni garantía de entrega.
- Baja latencia, ideal para transmisiones en vivo, como clases o conferencias.
- Utiliza protocolos como RTP.

**Adaptación al ancho de banda:**
- No se adapta automáticamente al ancho de banda del cliente.
- Si hay congestión o baja velocidad, se pierden paquetes, lo que afecta la calidad.
- Requiere una red controlada  para funcionar bien.

**Uso sugerido:**
- En aulas híbridas o auditorios con buena red interna.

---

### 2. HTTP Streaming

**Características:**
- Basado en HTTP/HTTPS: el cliente descarga un archivo de video mientras lo reproduce.
- Común en sitios pequeños o con video estático.

**Adaptación al ancho de banda:**
- No es adaptable: se transmite el archivo tal como está.
- Si el ancho de banda es bajo, puede haber pausas por buffering.

**Uso sugerido:**
- Videos grabados accesibles desde el portal del campus o bibliotecas multimedia.
- Ideal para contenido educativo bajo demanda con calidad fija.

---

### 3. DASH

**Características:**
- Divide el video en fragmentos pequeños y varias versiones del mismo video a diferentes calidades.
- El cliente selecciona dinámicamente la calidad de cada fragmento según su ancho de banda actual.
- Se basa en HTTP/HTTPS, funciona bien con CDN y firewalls.
- HLS funciona de forma similar y también es ampliamente utilizado.

**Adaptación al ancho de banda:**
- Altamente adaptable: si el ancho de banda disminuye, el cliente baja la calidad sin interrumpir el video.
- Puede subir a calidad HD si la conexión mejora.

**Uso sugerido:**
- Streaming de clases en vivo o bajo demanda accesible desde dentro y fuera del campus.
- Compatible con móviles, WiFi variable y acceso remoto.

---

## Comparación de Técnicas

| Técnica            | Latencia  | Adaptabilidad | Ideal para                        | Confiabilidad en red variable |
|--------------------|-----------|----------------|------------------------------------|-------------------------------|
| UDP Streaming      | Muy baja  | No             | Clases en vivo (LAN)               | Baja                          |
| HTTP Streaming     | Media     | No             | Video educativo bajo demanda       | Media                         |
| DASH / HLS         | Alta (inicial) | Sí      | Acceso remoto, dispositivos móviles| Alta                          |

---

## Conclusión

Para un campus universitario inteligente, se recomienda:

- Utilizar **DASH o HLS** para todo el contenido multimedia accesible desde WiFi, red externa o dispositivos móviles.
- Emplear **UDP/RTP** en entornos de red local controlada como aulas híbridas o auditorios.
- Complementar con un sistema de CDN interno o caché local para optimizar la entrega del contenido multimedia.


## Paso 6: Seguridad – Estrategias y Configuraciones

### Medidas de Seguridad Implementadas

La seguridad de la red del campus se ha estructurado en múltiples capas, integrando mecanismos preventivos, defensivos y de segmentación lógica. Las principales medidas son:

#### 1. Proteccion de red interna con dmz de doble firewall

Se utiliza una arquitectura de *dos firewalls ASA (Cisco Adaptive Security Appliance)*:

- *Firewall externo (ASA1):*  
  Conectado a Internet. Su función principal es proteger la DMZ del tráfico no autorizado proveniente del exterior.

- *Firewall interno (ASA2):*  
  Separa la red interna de la DMZ. Controla el acceso de usuarios internos a servidores públicos y restringe que servicios expuestos comprometan el interior.

- *DMZ:*  
  Segmento de red intermedio donde se ubican servicios como:
  - Servidor DNS
  - Servidor FTP
  - Servidor de correo electrónico
  - Servicios web


Esta segmentación reduce significativamente el riesgo de intrusiones, ya que incluso si un servidor en la DMZ se ve comprometido, el acceso a la red interna sigue protegido por un segundo nivel de filtrado. Para una correcta implementación, es fundamental configurar rutas estáticas, NAT y ACLs específicas en ambos firewalls, asegurando además que los servidores tengan como puerta de enlace el ASA interno. Esta estrategia proporciona una sólida defensa, control granular del tráfico y una estructura alineada con las mejores prácticas de seguridad en redes académicas o corporativas.

#### 2. Firewall Perimetral con ACLs

El firewall se posiciona estratégicamente en la entrada a la red interna, actuando como la primera línea de defensa. Se configura con *ACLs (Access Control Lists)* que filtran el tráfico según:

- ACL_In_DMZ: Permite que cualquier ip de red interna cruce el  primer firwall(ASA 2).
- ACL_DMZ_In: Permite que los servidores dentro de la dmz lleguen a red interna
- ACL_DMZ_Out: Permite que todas las ip de la DMZ crucen el segundo firewall(ASA 1)

Esto permite bloquear accesos no autorizados desde el exterior, limitar conexiones innecesarias entre VLANs, y prevenir ataques como escaneo de puertos o denegación de servicio (DoS).

#### 3. Separación por VLANs

La red está segmentada en vlans que añaden medidas de seguridad y encapsulación:

- Aísla el tráfico entre grupos funcionales
- Reduce la superficie de ataque
- Mejora la contención ante incidentes

Aparte gracias a la presencia del switch de capa 3 no es necesario que las vlans lleguen al router para poder comunicarse entre ellas.
