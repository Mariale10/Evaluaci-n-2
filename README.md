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

# 3. Parte Empírica: Implementación de Blockchain para Mensajería

## Descripción del Proyecto
Se ha desarrollado una cadena de bloques (Blockchain) diseñada para permitir el intercambio de mensajes seguros e inmutables entre dos servidores. [cite_start]Esta solución garantiza que cualquier mensaje enviado no pueda ser alterado sin romper la integridad de toda la cadena[cite: 37].

## Paso a Paso de la Funcionalidad
La cadena opera bajo el siguiente flujo lógico:
1. **Instanciación:** Se crea el "Bloque Génesis", que es el primer bloque de la cadena con un hash previo de "0".
2. **Creación de Mensaje:** El Servidor A genera un mensaje (Data).
3. [cite_start]**Generación de Bloque:** El sistema toma el mensaje, el índice, la marca de tiempo y el Hash del bloque anterior para crear un nuevo bloque[cite: 37].
4. **Validación:** El Servidor B recibe el bloque y verifica que el `hash_previo` coincida con el hash del último bloque en su registro local.
5. **Sincronización:** Si es válido, el bloque se añade a la cadena compartida.

## Aspectos Técnicos

### [cite_start]¿Cómo se crea un bloque? [cite: 38]
Un bloque se crea mediante un proceso de empaquetado de datos que incluye:
* **Índice:** La posición del bloque en la cadena.
* **Timestamp:** Fecha y hora exacta de la creación.
* **Data:** El contenido del mensaje enviado entre los servidores.
* **Previous Hash:** El enlace criptográfico con el bloque anterior.
* **Nonce:** Un número aleatorio utilizado para el proceso de minería (Proof of Work).
* **Hash:** El identificador único generado mediante SHA-256 que sella el bloque.

### [cite_start]¿Qué tipo de encriptación se maneja en Blockchain? [cite: 39]
* **Hashing Criptográfico (SHA-256):** No es encriptación en el sentido estricto (ya que es unidireccional), pero se usa para asegurar la integridad. Si un solo bit del mensaje cambia, el Hash resultante será completamente diferente.
* **Criptografía de Clave Asimétrica (ECDSA):** Utilizada para firmar los mensajes, asegurando que solo los servidores autorizados puedan emitir nuevos bloques.

---

## Código de Simulación (Python)
Puedes ejecutar este código para demostrar la creación de la cadena:

```python
import hashlib
import json
from time import time

class Blockchain:
    def __init__(self):
        self.chain = []
        self.create_block(proof=1, previous_hash='0', data="Bloque Génesis")

    def create_block(self, proof, previous_hash, data):
        block = {
            'index': len(self.chain) + 1,
            'timestamp': time(),
            'data': data,
            'proof': proof,
            'previous_hash': previous_hash
        }
        block['hash'] = self.hash(block)
        self.chain.append(block)
        return block

    def hash(self, block):
        encoded_block = json.dumps(block, sort_keys=True).encode()
        return hashlib.sha256(encoded_block).hexdigest()

# Simulación de mensajes entre servidores
my_blockchain = Blockchain()
my_blockchain.create_block(proof=100, previous_hash=my_blockchain.chain[-1]['hash'], data="Mensaje de Servidor A a B")
print(json.dumps(my_blockchain.chain, indent=4))
**¿Cómo se crea un bloque?**
Se toma la información (Data), el Hash anterior y un "Nonce". Se aplica la función hash hasta que el resultado cumpla con la dificultad de la red.

---
*Este proyecto fue desarrollado con fines académicos para la asignatura de Seguridad de la Información.*
