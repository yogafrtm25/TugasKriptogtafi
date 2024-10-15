# TugasKriptogtafi

````
import string

class PlayfairCipher:
    def __init__(self, key):
        self.key = self.prepare_key(key)
        self.matrix = self.create_matrix()

    def prepare_key(self, key):
        key = key.replace(" ", "").upper()
        key = ''.join(sorted(set(key), key=key.index))  # Remove duplicates while maintaining order
        return key

    def create_matrix(self):
        alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"  # I/J are merged
        key_matrix = self.key + ''.join([char for char in alphabet if char not in self.key])
        return [key_matrix[i:i + 5] for i in range(0, 25, 5)]

    def format_text(self, text):
        text = text.replace(" ", "").upper()
        text = text.replace("J", "I")  # Replace J with I
        formatted_text = ""
        i = 0

        while i < len(text):
            char1 = text[i]
            formatted_text += char1
            if i + 1 < len(text):
                char2 = text[i + 1]
                if char1 == char2:  # If the same, insert 'X'
                    formatted_text += 'X'
                    i += 1
                else:
                    formatted_text += char2
                    i += 2
            else:
                formatted_text += 'X'  # If odd length, append 'X'
                i += 1

        return formatted_text

    def find_position(self, char):
        for i, row in enumerate(self.matrix):
            if char in row:
                return i, row.index(char)
        return None

    def encrypt(self, plaintext):
        formatted_text = self.format_text(plaintext)
        ciphertext = ""

        for i in range(0, len(formatted_text), 2):
            r1, c1 = self.find_position(formatted_text[i])
            r2, c2 = self.find_position(formatted_text[i + 1])

            if r1 == r2:  # Same row
                ciphertext += self.matrix[r1][(c1 + 1) % 5]
                ciphertext += self.matrix[r2][(c2 + 1) % 5]
            elif c1 == c2:  # Same column
                ciphertext += self.matrix[(r1 + 1) % 5][c1]
                ciphertext += self.matrix[(r2 + 1) % 5][c2]
            else:  # Rectangle
                ciphertext += self.matrix[r1][c2]
                ciphertext += self.matrix[r2][c1]

        return ciphertext

    def decrypt(self, ciphertext):
        decrypted_text = ""

        for i in range(0, len(ciphertext), 2):
            r1, c1 = self.find_position(ciphertext[i])
            r2, c2 = self.find_position(ciphertext[i + 1])

            if r1 == r2:  # Same row
                decrypted_text += self.matrix[r1][(c1 - 1) % 5]
                decrypted_text += self.matrix[r2][(c2 - 1) % 5]
            elif c1 == c2:  # Same column
                decrypted_text += self.matrix[(r1 - 1) % 5][c1]
                decrypted_text += self.matrix[(r2 - 1) % 5][c2]
            else:  # Rectangle
                decrypted_text += self.matrix[r1][c2]
                decrypted_text += self.matrix[r2][c1]

        return decrypted_text

# Example usage
if __name__ == "__main__":
    key = "YOURKEY"  # Replace with your key

    playfair = PlayfairCipher(key)

    plaintexts = [
        "GOOD BROOM SWEEP CLEAN",
        "REDWOOD NATIONAL STATE PARK",
        "JUNK FOOD AND HEALTH PROBLEMS"
    ]

    for plaintext in plaintexts:
        encrypted = playfair.encrypt(plaintext)
        decrypted = playfair.decrypt(encrypted)
        print(f"Plaintext: {plaintext}")
        print(f"Encrypted: {encrypted}")
        print(f"Decrypted: {decrypted}")
        print()


````
Penjelasan
Persiapan Kunci: Kunci yang diberikan diolah untuk membuang karakter duplikat dan menyiapkan matriks 5x5.
Format Teks: Teks diformat untuk memastikan setiap pasangan karakter tidak sama, dan jika ada karakter yang sama, 'X' disisipkan.
Enkripsi: Menggunakan aturan Playfair untuk mengenkripsi teks.
Dekripsi: Menggunakan aturan Playfair yang sama untuk mendekripsi teks yang terenkripsi.

Hasil RUN

![Screenshot 2024-10-15 091939](https://github.com/user-attachments/assets/1e0bb66a-7835-4b63-ae7a-60eb2228ffb9)

