<template>
  <div class="container mt-5">
    <div class="form-group">
      <input v-model="plaintext" class="form-control" placeholder="Texto a cifrar" />
    </div>
    <div class="form-group">
      <button @click="encrypt" class="btn btn-primary my-2">Cifrar</button>
    </div>
    <div class="form-group">
      <p>Cifrado: {{ ciphertext }}</p>
    </div>
    <div class="form-group">
      <button @click="decrypt" class="btn btn-primary my-2">Descifrar</button>
    </div>
    <div class="form-group">
      <p>Descifrado: {{ decryptedText }}</p>
    </div>
  </div>
</template>

<script>
import CryptoJS from 'crypto-js';

export default {
  data() {
    return {
      plaintext: '',
      ciphertext: '',
      decryptedText: '',
      secretKey: '1234567890123456', // 16 bytes (128 bits)
      iv: CryptoJS.enc.Hex.parse('0123456789ABCDEF'), // Vector de inicialización
    };
  },
  methods: {
    encrypt() {
      const key = CryptoJS.enc.Utf8.parse(this.secretKey);
      const encrypted = CryptoJS.TripleDES.encrypt(this.plaintext, key, {
        iv: this.iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7,
      });
      this.ciphertext = encrypted.toString();
    },
    decrypt() {
      const key = CryptoJS.enc.Utf8.parse(this.secretKey);
      const decrypted = CryptoJS.TripleDES.decrypt(this.ciphertext, key, {
        iv: this.iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7,
      });
      this.decryptedText = decrypted.toString(CryptoJS.enc.Utf8);
    },
  },
};
</script>
