# TugasKriptogtafi

````
def create_playfair_matrix(key):
    # Menghilangkan duplikat huruf dan menggabungkan 'J' dengan 'I'
    key = ''.join(sorted(set(key), key=key.index)).replace('J', 'I')
    matrix = []
    alphabet = 'ABCDEFGHIKLMNOPQRSTUVWXYZ'
    for char in key:
        if char not in matrix:
            matrix.append(char)
    for char in alphabet:
        if char not in matrix:
            matrix.append(char)
    # Membentuk matrix 5x5
    return [matrix[i:i+5] for i in range(0, 25, 5)]

def preprocess_text(plaintext):
    plaintext = plaintext.replace('J', 'I').upper().replace(" ", "")
    pairs = []
    i = 0
    while i < len(plaintext):
        a = plaintext[i]
        b = plaintext[i + 1] if i + 1 < len(plaintext) else 'X'
        if a == b:
            pairs.append(a + 'X')
            i += 1
        else:
            pairs.append(a + b)
            i += 2
    if len(pairs[-1]) == 1:
        pairs[-1] += 'X'
    return pairs

def find_position(matrix, char):
    for row in range(5):
        for col in range(5):
            if matrix[row][col] == char:
                return row, col
    return None

def encrypt_pair(matrix, a, b):
    row_a, col_a = find_position(matrix, a)
    row_b, col_b = find_position(matrix, b)
    if row_a == row_b:
        return matrix[row_a][(col_a + 1) % 5] + matrix[row_b][(col_b + 1) % 5]
    elif col_a == col_b:
        return matrix[(row_a + 1) % 5][col_a] + matrix[(row_b + 1) % 5][col_b]
    else:
        return matrix[row_a][col_b] + matrix[row_b][col_a]

def playfair_encrypt(plaintext, key):
    matrix = create_playfair_matrix(key)
    pairs = preprocess_text(plaintext)
    ciphertext = ''
    for pair in pairs:
        ciphertext += encrypt_pair(matrix, pair[0], pair[1])
    return ciphertext

# Contoh penggunaan
plaintext1 = "GOOD BROOM SWEEP CLEAN"
plaintext2 = "REDWOOD NATIONAL STATE PARK"
plaintext3 = "JUNK FOOD AND HEALTH PROBLEMS"
key = "TEKNIK INFORMATIKA"

print("Enkripsi Playfair Cipher:")
print("Plaintext 1:", playfair_encrypt(plaintext1, key))
print("Plaintext 2:", playfair_encrypt(plaintext2, key))
print("Plaintext 3:", playfair_encrypt(plaintext3, key))
````

Hasil RUN

![Screenshot 2024-10-15 085941](https://github.com/user-attachments/assets/01773fc8-46ca-47f2-8048-45dec8fdf1d8)
