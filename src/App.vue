<template>
  <div class="fundo">
    <img class="perfil-img" src="/pessoas.png">
    
    <div class="login-container">
      <input type="text" v-model="email" placeholder="E-mail">
      <input type="password" v-model="password" placeholder="Senha">
      <button @click="fazerLogin">Entrar</button>
    </div>

    <p>{{ mensagem }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      email: "", 
      password: "",
      mensagem: "Bem-vindo!"
    }
  },
  methods: {
    async fazerLogin() {
      console.log("Tentativa de login com:", this.email);

      try {
        const resposta = await axios.post('http://localhost:3000/api/users/login', {
          email: this.email,
          password: this.password
        });

        const { accessToken, user } = resposta.data;
        
        localStorage.setItem('token', accessToken);
        alert("Login realizado com sucesso! Bem-vindo.");
        this.mensagem = `Logado como: ${user.email}`;

      } catch (error) {
        this.mensagem = error.response?.data?.error || "Erro de conexão com o servidor";
        console.error("Erro detalhado:", error);
      }
    }
  } 
} // <--- Faltava fechar o export default aqui
</script> <style scoped>
.fundo {
  min-height: 100vh;
  width: 100%;
  background: linear-gradient(#222831, #393E46, #00ADB5, #EEEEEE);
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.perfil-img {
  width: 100px;
  height: auto;
  margin-bottom: 20px;
  border-radius: 10px;
}

.login-container {
  display: flex;
  flex-direction: column;
  gap: 15px;
  width: 100%;
  max-width: 300px;
  padding: 20px;
}

input {
  padding: 12px;
  border-radius: 8px;
  border: none;
  outline: none;
  font-size: 16px;
}

button {
  padding: 12px;
  border-radius: 8px;
  border: none;
  background-color: #00ADB5;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s;
}

button:hover {
  background-color: #393E46;
}
</style>