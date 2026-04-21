import hashlib
import json
from time import time

class Blockchain:
    def __init__(self):
        self.chain = []
        # Crear el primer bloque (Bloque Génesis)
        self.create_block(proof=1, previous_hash='0', data="Bloque Génesis")

    def create_block(self, proof, previous_hash, data):
        block = {
            'index': len(self.chain) + 1,
            'timestamp': time(),
            'data': data,
            'proof': proof,
            'previous_hash': previous_hash
        }
        # Se genera el sello de seguridad usando SHA-256
        block['hash'] = self.hash(block)
        self.chain.append(block)
        return block

    def hash(self, block):
        # El bloque se convierte en texto para poder generar el hash
        encoded_block = json.dumps(block, sort_keys=True).encode()
        return hashlib.sha256(encoded_block).hexdigest()

# Simulación de mensajes entre servidores
my_blockchain = Blockchain()
my_blockchain.create_block(proof=100, previous_hash=my_blockchain.chain[-1]['hash'], data="Mensaje de Servidor A a B")

# Mostrar la cadena de bloques resultante
print(json.dumps(my_blockchain.chain, indent=4))
