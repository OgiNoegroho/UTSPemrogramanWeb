<template>
  <div class="container">
    <div class="logo-container">
      <img src="@/assets/images/logouin.png" alt="SIPTATIF Logo" class="logo-img">
      <h1>SIPTATIF</h1>
      <h2>Sistem Informasi Pendaftaran Tugas Akhir Mahasiswa Teknik Informatika</h2>
    </div>
    <div class="form-container">
      <h1>Log In</h1>
      <form @submit.prevent="login">
        <label for="email">Masukkan Email Anda</label>
        <input type="email" v-model="email" id="email" required>
        <br>
        <label for="password">Password</label>
        <input type="password" v-model="password" id="password" required>
        <a href="/forgot-password" class="forgot-password">Forgot Password?</a>
        <div class="button-container">
          <button :disabled="isLoading" type="submit" class="register-button">
            <span v-if="isLoading">Loading...</span>
            <span v-else>Login</span>
          </button>
          <a href="/registration" class="register-button">Buat Akun</a>
        </div>
      </form>
      <p v-if="successMessage" class="success-message">{{ successMessage }}</p>
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      isLoading: false,
      successMessage: '',
      errorMessage: ''
    };
  },
  computed: {
    registeredUsers() {
      return this.$store.getters.getRegisteredUsers;
    }
  },
  methods: {
    async login() {
      this.isLoading = true;
      const user = this.registeredUsers.find(user => 
        user.email === this.email && user.password === this.password
      );

      if (user) {
        const userData = {
          email: user.email,
        };
        try {
          await this.$store.dispatch('loginUser', userData);
          this.successMessage = 'Login berhasil!';
          this.$router.push({ name: 'Home' }); 
        } catch (error) {
          this.errorMessage = 'Terjadi kesalahan saat login.';
        } finally {
          this.isLoading = false;
        }
      } else {
        this.errorMessage = 'Email atau password salah';
        this.isLoading = false;
      }
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Poppins:400,500,600,700&display=swap');

body {
  margin: 0;
  padding: 0;
  font-family: 'Poppins', sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #234D20 !important;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh; /* Change height to viewport height */
}

.logo-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-left: 50px;
  margin-right: 50px;
}

.logo-img {
  width: 150px;
  margin-bottom: 20px;
}

h1, h2 {
  color: black;
}

h1 {
  font-size: 50px;
  text-align: center;
  margin-top: 5px;
  margin-bottom: 5px;
}

h2 {
  font-size: 14px;
  text-align: center;
  margin-bottom: 50px;
}

.form-container {
  background-color: #77AB59;
  padding: 30px;
  width: 40%; /* Adjust width */
  max-width: 500px; /* Add max-width */
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgba(0,0,0,0.1);
  margin-right: 50px; /* Add some margin to the left */
}

.form-container h1 {
  color: white;
  font-size: 50px;
  text-align: center;
  margin-top: 5px;
  margin-bottom: 10px;
}

label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}

input[type="email"], input[type="password"] {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 10px;
  outline: none;
  box-sizing: border-box;
  font-size: 16px;
}

button {
  background-color: #36802D;
  color: #000000;
  border: none;
  padding: 10px 20px;
  border-radius: 10px;
  cursor: pointer;
  margin-top: 15px;
  width: 100%;
  box-sizing: border-box;
  font-size: 16px;
}

button:hover {
  background-color: #234D20;
}

a {
  color: #007bff;
  text-decoration: none;
  margin-left: 10px;
}

a:hover {
  text-decoration: underline;
}

.forgot-password {
  display: block;
  text-align: right;
  font-size: small;
  margin-top: 5px;
}

.button-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-weight: bold;
  margin-top: 15px;
}

.register-button {
  background-color: #36802D;
  color: #000;
  padding: 10px 20px;
  border-radius: 10px;
  margin-top: 15px;
  text-decoration: none;
  font-weight: bold;
  width: 100%;
  box-sizing: border-box;
  font-size: 16px;
}

.register-button:hover {
  background-color: #234D20;
}

.success-message {
  color: white;
  text-align: center;
  margin-top: 15px;
}

.error-message {
  color: red;
  text-align: center;
  margin-top: 15px;
}

</style>


