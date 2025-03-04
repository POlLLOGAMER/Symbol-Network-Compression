# Symbol-Network-Compression

```
import sys

def get_common_chars(char1, char2):
    # Dado que cada letra es un string de un carácter,
    # la intersección solo tendrá valor si ambas son iguales.
    return set(char1) & set(char2)

def compress_letters(letters):
    nodes = []
    order = []  # Guardará el orden de compresión
    added = [False] * len(letters)  # Marca cada letra individualmente
    # Cada nodo será una tupla: (lista_de_letras, lista_de_posiciones)
    
    for i in range(len(letters)):
        if not added[i]:
            current_node = [letters[i]]
            current_positions = [i]
            added[i] = True
            for j in range(i + 1, len(letters)):
                if not added[j] and get_common_chars(letters[i], letters[j]):
                    current_node.append(letters[j])
                    current_positions.append(j)
                    added[j] = True
            nodes.append((current_node, current_positions))
            order.append(len(nodes) - 1)
    return nodes, order

def decompress_letters(nodes, order):
    decompressed_letters = []
    recovered_positions = []
    
    for index in order:
        node_letters, node_positions = nodes[index]
        decompressed_letters.extend(node_letters)
        recovered_positions.extend(node_positions)
    
    # Ordenamos las letras recuperadas según sus posiciones originales
    sorted_pairs = sorted(zip(recovered_positions, decompressed_letters))
    return [letter for _, letter in sorted_pairs]

def get_size_in_bytes(obj):
    return sys.getsizeof(obj)

# Ejemplo de uso con letras (¡con humor y peligro! XD)
text = "Hola jajaja"
letters = list(text)  # Convertimos el texto en lista de caracteres

compressed_nodes, order = compress_letters(letters)
compressed_size = get_size_in_bytes(compressed_nodes)

decompressed_letters = decompress_letters(compressed_nodes, order)
decompressed_size = get_size_in_bytes(decompressed_letters)

decompressed_text = ''.join(decompressed_letters)

# Resultados
print("Texto original:", text)
print("Texto convertido a letras:", letters)
print("Letras comprimidas en nodos:")
for idx, (node_letters, positions) in enumerate(compressed_nodes):
    print(f"Nodo {idx}: {node_letters} (posiciones: {positions})")

print("\nOrden de compresión:", order)
print("\nLetras descomprimidas:")
print(decompressed_letters)
print("\nTexto descomprimido:", decompressed_text)

print(f"\nTamaño comprimido (nodos): {compressed_size} bytes")
print(f"Tamaño descomprimido: {decompressed_size} bytes")
```
