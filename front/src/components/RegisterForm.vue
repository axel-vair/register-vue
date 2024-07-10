<template>
  <div class="register-form">
    <h2>Inscription</h2>
    <form @submit.prevent="register">
      <div class="form-group">
        <label for="email">Email:</label>
        <input
            type="email"
            id="email"
            v-model="user.email"
            required
        >
      </div>
      <div class="form-group">
        <label for="password">Mot de passe:</label>
        <input
            type="password"
            id="password"
            v-model="user.password"
            required
        >
      </div>
      <div class="form-group">
        <label for="confirmPassword">Confirmer le mot de passe:</label>
        <input
            type="password"
            id="confirmPassword"
            v-model="confirmPassword"
            required
        >
      </div>
      <button type="submit">S'inscrire</button>
    </form>
    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'RegisterForm',
  data() {
    return {
      user: {
        email: '',
        password: '',
      },
      confirmPassword: '',
      error: null,
    };
  },
  methods: {
    async register() {
      this.error = null;
      if (this.user.password !== this.confirmPassword) {
        this.error = "Les mots de passe ne correspondent pas.";
        return;
      }
      try {
        const response = await axios.post('http://localhost:8000/api/register', this.user);
        console.log('Inscription réussie', response.data);
        // Rediriger l'utilisateur ou afficher un message de succès
        this.$router.push('/login'); // Assurez-vous d'avoir configuré vue-router
      } catch (error) {
        console.error("Erreur lors de l'inscription", error);
        this.error = error.response?.data?.message || "Une erreur est survenue lors de l'inscription.";
      }
    },
  },
};
</script>

<style scoped>
.register-form {
  max-width: 300px;
  margin: 0 auto;
}
.form-group {
  margin-bottom: 15px;
}
label {
  display: block;
  margin-bottom: 5px;
}
input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
button {
  width: 100%;
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
.error {
  color: red;
  margin-top: 10px;
}
</style>