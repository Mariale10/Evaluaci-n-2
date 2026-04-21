import hashlib
import json
from time import time

class Blockchain:
    def __init__(self):
        self.chain = []
        # Bloque inicial obligatorio
        self.create_block(proof=1, previous_hash='0', data="Bloque Génesis")

    def create_block(self, proof, previous_hash, data):
        block = {
            'index': len(self.chain) + 1,
            'timestamp': time(),
            'data': data,
            'proof': proof,
            'previous_hash': previous_hash
        }
        # El sellado del bloque usa SHA-256
        block['hash'] = self.hash(block)
        self.chain.append(block)
        return block

    def hash(self, block):
        # Aseguramos que el JSON sea consistente para el hashing
        encoded_block = json.dumps(block, sort_keys=True).encode()
        return hashlib.sha256(encoded_block).hexdigest()

# Simulación de intercambio entre Servidor A y Servidor B
ucompensar_net = Blockchain()
ucompensar_net.create_block(proof=100, previous_hash=ucompensar_net.chain[-1]['hash'], data="Mensaje: Conexión Servidor A OK")
ucompensar_net.create_block(proof=200, previous_hash=ucompensar_net.chain[-1]['hash'], data="Mensaje: Respuesta Servidor B OK")

print(json.dumps(ucompensar_net.chain, indent=4))
