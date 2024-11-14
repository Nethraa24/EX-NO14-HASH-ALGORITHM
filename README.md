# EX-NO14-HASH-ALGORITHM

## AIM:
To implement HASH ALGORITHM

## ALGORITHM:

1. Hash Algorithm is used to convert input data (message) into a fixed-size string, typically a hash value, which uniquely represents the original data.

2. Initialization:
   - Choose a hash function \( H \) (e.g., SHA-256, MD5, etc.).
   - The message \( M \) to be hashed is input.

3. Message Preprocessing:
   - Break the message \( M \) into fixed-size blocks. If necessary, pad the message to make it compatible with the block size required by the hash function.
   - For example, in SHA-256, the message is padded to ensure that its length is a multiple of 512 bits.

4. Hash Calculation:
   - Process the message block by block, applying the hash function \( H \) iteratively to produce an intermediate hash value.
   - For SHA-256, each block is processed through a series of logical operations, bitwise manipulations, and modular additions.

5. Output:
   - After all blocks are processed, the final hash value (digest) is produced, which is a fixed-size output (e.g., 256-bit for SHA-256).
   - The resulting hash is unique to the input message, meaning even a small change in the message will result in a completely different hash.

6. Security: The strength of the hash algorithm lies in its collision resistance, ensuring that it is computationally infeasible to find two different messages that produce the same hash value.


## Program:
```
#include <stdio.h>
#include <string.h>
#define MAX_LEN 256 // Maximum length of the message
#define BLOCK_SIZE 64 // Block size for the HMAC
// XOR pad function
void xor_pad(char *key, char pad, char *output, int key_len) {
for (int i = 0; i < key_len; i++) {
output[i] = key[i] ^ pad;
}
for (int i = key_len; i < BLOCK_SIZE; i++) {
output[i] = pad;
}
}
// Simple hashing function (XOR sum)
void simple_hash(const char *input, char *output) {
int len = strlen(input);
char hash_value = 0;

for (int i = 0; i < len; i++) {
hash_value ^= input[i];
}
snprintf(output, 3, "%02x", hash_value); // Store the hash as a hex string
}
// Perform HMAC-like operation
void hmac(const char *message, const char *key, char *output_mac) {
char o_key_pad[BLOCK_SIZE]; // Outer padded key
char i_key_pad[BLOCK_SIZE]; // Inner padded key
char temp[MAX_LEN + BLOCK_SIZE]; // Buffer for inner hash calculation
char inner_hash[3]; // To store the result of the inner hash
int key_len = strlen(key);
// XOR the key with inner and outer pads
xor_pad((char *)key, 0x36, i_key_pad, key_len);
xor_pad((char *)key, 0x5c, o_key_pad, key_len);
// Perform the inner hash: hash(i_key_pad || message)
strcpy(temp, i_key_pad);
strcat(temp, message);
simple_hash(temp, inner_hash); // Calculate inner hash
// Perform the outer hash: hash(o_key_pad || inner_hash)
strcpy(temp, o_key_pad);
strcat(temp, inner_hash);
simple_hash(temp, output_mac); // Calculate outer hash (final MAC)
}
// XOR-based encryption
void encrypt(const char *input, const char *key, char *output) {
int len = strlen(input);
int key_len = strlen(key);
for (int i = 0; i < len; i++) {
output[i] = input[i] ^ key[i % key_len]; // XOR encryption
}
output[len] = '\0'; // Null-terminate the encrypted string
}
// XOR-based decryption
void decrypt(const char *input, const char *key, char *output) {
encrypt(input, key, output); // XOR encryption is symmetric
}
int main() {
char message[MAX_LEN]; // Plaintext message
char key[MAX_LEN]; // Symmetric key
char mac[3]; // HMAC output
char encrypted[MAX_LEN]; // Encrypted message
char decrypted[MAX_LEN]; // Decrypted message
printf("\n Simulation of HMAC Algorithm with Encryption and Decryption\n\n");

// Get plaintext message from the user
printf("Enter the plaintext message: ");
fgets(message, MAX_LEN, stdin);
message[strcspn(message, "\n")] = 0; // Remove newline character
// Get symmetric key from the user
printf("Enter the symmetric key: ");
fgets(key, MAX_LEN, stdin);
key[strcspn(key, "\n")] = 0; // Remove newline character
// Perform HMAC-like operation
hmac(message, key, mac);
printf("Generated HMAC: %s\n", mac);
// Perform encryption
encrypt(message, key, encrypted);
printf("Encrypted message (raw bytes): ");
for (int i = 0; i < strlen(message); i++) {
printf("%02x ", (unsigned char)encrypted[i]);
}
printf("\n");
// Perform decryption
decrypt(encrypted, key, decrypted);
printf("Decrypted message: %s\n", decrypted);
return 0;
}
```

## Output:

![image](https://github.com/user-attachments/assets/dd7c9893-8759-47d4-9df4-5a6b9c17be9a)

## Result:
The program is executed successfully.
