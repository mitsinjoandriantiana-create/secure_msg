Secure Messaging CLI
A secure command-line messaging application that implements end-to-end encryption using AES-256-GCM for data confidentiality, RSA-2048 for key exchange, and RSA-PSS for digital signatures. Includes protection against common attacks including replay attacks.
Features
·	Hybrid Encryption: AES-256-GCM for fast symmetric encryption + RSA-2048 for secure key exchange
·	Digital Signatures: RSA-PSS signatures ensure message authenticity and integrity
·	Replay Protection: Timestamps prevent message replay attacks
·	JSON Format: Human-readable message packets
·	CLI Interface: Simple command-line operations
Security Features
·	Authenticated Encryption: AES-GCM provides both confidentiality and integrity
·	Forward Secrecy: Each message uses a unique AES key
·	Signature Verification: All messages are cryptographically signed
·	Timestamp Validation: Messages expire after 5 minutes and reject future timestamps
·	Replay Detection: Prevents duplicate message acceptance
Installation
1.	Install Python dependencies:
pip install cryptography

2.	Clone or download the project files
Usage
1. Generate Keys
python main.py --action keygen

This creates private.pem and public.pem files.
2. Send a Message
python main.py --action send --msg "Your secret message" --outfile message.json

3. Receive a Message
python main.py --action receive --packet '{"timestamp": 1234567890, "enc_aes_key": "...", "nonce": "...", "ciphertext": "...", "signature": "..."}'

Or from a file:
python main.py --action receive --packet "$(cat message.json)"

Message Format
Messages are JSON objects with the following structure:
{
  "timestamp": 1776946861,
  "enc_aes_key": "base64-encoded-rsa-encrypted-aes-key",
  "nonce": "base64-encoded-12-byte-nonce",
  "ciphertext": "base64-encoded-aes-gcm-encrypted-data",
  "signature": "base64-encoded-rsa-pss-signature"
}

Security Testing
Run the automated security tests:
python test_attacks.py

Tests include:
·	Ciphertext tampering detection
·	Signature forgery detection
·	Replay attack prevention
Cryptographic Details
·	AES Mode: GCM (Galois/Counter Mode) with 256-bit keys
·	RSA Key Size: 2048 bits
·	RSA Padding: OAEP for encryption, PSS for signatures
·	Hash Function: SHA-256
·	Timestamp Window: ±5 minutes for validity
Attack Protections
1.	Ciphertext Modification: AES-GCM authentication tag detects tampering
2.	Signature Forgery: RSA-PSS signatures prevent impersonation
3.	Key Compromise: Hybrid encryption limits damage from RSA key exposure
4.	Replay Attacks: Timestamp validation + in-memory cache
5.	Network Eavesdropping: End-to-end encryption protects data in transit
Architecture
Sender: Message → AES-GCM Encrypt → RSA Encrypt Key → Sign → JSON Packet
Receiver: JSON Packet → Verify Signature → RSA Decrypt Key → AES-GCM Decrypt → Message

Limitations
·	In-memory replay cache (resets on application restart)
·	No message fragmentation for large messages
·	Single recipient per message
·	No key rotation or forward secrecy beyond per-message keys
License
This project is provided as-is for educational and demonstration purposes.
# Générer clés
python main.py --action keygen

# Envoyer
python main.py --action send --msg "Hello World" --outfile message.json

# Recevoir
python main.py --action receive --packet message.json

Sous PowerShell, la redirection > peut créer un fichier UTF-16. Pour un fichier JSON UTF-8 fiable, utilisez plutôt --outfile message.json ou python main.py --action send --msg "Hello World" | Out-File -Encoding utf8 message.json.
Sécurité
·	Vérification de signature AVANT déchiffrement
·	Nonce unique pour chaque message
·	Protection contre les attaques par rejeu (ajouter timestamp recommandé)
·	Authentification des messages via signature
Technologies
·	AES-256-GCM : Chiffrement symétrique authentifié
·	RSA-2048 : Chiffrement asymétrique
·	RSA-PSS : Signature probabiliste
·	JSON : Format d'échange
·	Base64 : Encodage des données binaires
