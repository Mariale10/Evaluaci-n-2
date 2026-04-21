# Evaluación 2: Seguridad de la Información
**Institución:** Fundación Universitaria Compensar  
**Programa:** Ingeniería en Telecomunicaciones  
**Integrantes:** * Maria Alejandra Nuñez
* Efrain Yair Diaz Anzola

---

## 1. Parte Conceptual

### A. Footprinting: Reconocimiento Activo vs. Pasivo
* **Reconocimiento Pasivo:** Se obtiene información sin interactuar directamente con el objetivo (ej. búsquedas en Google, Whois, redes sociales). No deja rastros en los logs de la víctima.
* **Reconocimiento Activo:** Implica interactuar directamente con el sistema (ej. escaneo de puertos con Nmap). Es más preciso pero detectable.

### B. Exploits, Payloads y Post-Exploits
* **Exploit:** Fragmento de software o comando que aprovecha una vulnerabilidad. *Ejemplo: EternalBlue (MS17-010).*
* **Payload:** El código que se ejecuta tras una explotación exitosa (la "carga útil"). *Ejemplo: Meterpreter o una Shell reversa.*
* **Post-Exploit:** Acciones realizadas tras ganar acceso para mantener persistencia o elevar privilegios. *Ejemplo: Mimikatz para extraer credenciales.*

### C. Metasploit Framework
Es una herramienta para desarrollar y ejecutar exploits contra una máquina remota. En entornos reales, se usa para **Pruebas de Penetración**, permitiendo visualizar eventos anormales al simular ataques controlados para verificar si los sistemas de detección (IDS/IPS) generan las alertas correctas.

### D. Criptografía: Clásica, Cuántica y Post-Cuántica
* **Clásica:** Basada en algoritmos matemáticos (AES, RSA) vulnerables ante el poder de cómputo masivo futuro.
* **Cuántica:** Utiliza principios de la física cuántica (entrelazamiento) para el intercambio de llaves (QKD).
* **Post-Cuántica:** Algoritmos matemáticos diseñados para ser resistentes incluso ante ataques de computadoras cuánticas.

### E. Blockchain y Criptomonedas
Es un libro de contabilidad digital, descentralizado e inmutable. En criptomonedas, se usa para registrar transacciones de forma segura mediante consenso, evitando el doble gasto sin necesidad de un banco central.

### F. Sistemas Biométricos
Utilizan protocolos como **FIDO2** o **BioAPI** y mecanismos de seguridad como el cifrado de plantillas, "Liveness Detection" (para evitar fotos/máscaras) y almacenamiento en enclaves seguros (TEE).

---

## 2. Parte de Diseño: Seguridad en Nodos Bitcoin y Repositorio Core

### Matriz de Amenazas (Vectores de Ataque)
| Vector | Impacto | Probabilidad | Contramedida |
| :--- | :--- | :--- | :--- |
| Ataque de Doble Gasto | Muy Alto | Baja | Esperar al menos 6 confirmaciones de red. |
| Vulnerabilidad en Código (GitHub) | Crítico | Media | Firma PGP de commits y auditorías de código. |
| Inyección de Malware en Nodo | Alto | Media | Uso de Firewalls, segmentación de red y actualización constante de Bitcoin Core. |

### Metodología de Hacking Ético Aplicada
1. **Reconocimiento:** Identificación de versiones de nodos y servicios RPC expuestos.
2. **Escaneo:** Uso de Nmap para detectar puertos abiertos (ej. 8333).
3. **Explotación:** Intento de acceso mediante credenciales débiles en la interfaz RPC.
4. **Post-Explotación:** Análisis de la billetera (wallet.dat) y persistencia en el servidor.

---

## 3. Parte Empírica: Desarrollo de Blockchain Mensajería

### Funcionalidad y Paso a Paso
Esta blockchain permite enviar mensajes cifrados entre dos servidores (Nodo A y Nodo B).
1. **Creación del bloque:** Cada mensaje se agrupa con un timestamp, un índice y el hash del bloque anterior.
2. **Encriptación:** Se utiliza el algoritmo **SHA-256** para garantizar la integridad de cada bloque.
3. **Consenso:** Ambos servidores validan que el hash previo coincida antes de añadir el nuevo mensaje a la cadena.

**¿Cómo se crea un bloque?**
Se toma la información (Data), el Hash anterior y un "Nonce". Se aplica la función hash hasta que el resultado cumpla con la dificultad de la red.

---
*Este proyecto fue desarrollado con fines académicos para la asignatura de Seguridad de la Información.*
