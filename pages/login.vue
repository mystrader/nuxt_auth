<template>
  <div class="container">
    <h1>Sign in para acessar página secreta</h1>
    <form @submit.stop.prevent="postLogin">
    <div>
      <label for="username">
        Nome : 
        <input id="username" type="text"  v-model="username">
      </label>
         <!-- <label for="email">
         E-mail : 
        <input id="email" type="email" v-model="email" >
      </label> -->
      <label for="password">
        Senha : 
        <input id="password" type="password" v-model="password" >
      </label>
      <button @click="postLogin">
        login
      </button>
      <!-- <p>The credentials are not verified for the example purpose.</p> -->
    </div>
    </form>

<br>
    <pre>
      email: "dhuo@dhuo.com"
password: "dhuo"
username: "dhuo"
    </pre>
  </div>
</template>

<script>
const Cookie = process.client ? require('js-cookie') : undefined

export default {
  middleware: 'notAuthenticated',
  data(){
    return{
     username: '' ,
     email: '' ,
     password: '',
     token: ''
    }
  },
  methods: {
    postLogin () {
    

    const payload = {
      username: this.username,
      // email: this.email,
      password: this.password
    }

    //FETCH DOS DADOS
      fetch('https://dogsapi.origamid.dev/json/jwt-auth/v1/token', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(payload),
    })
      .then((response) => {
        console.log(response);
        return response.json();
      })
      .then((json) => {
        console.log(json);
        this.token = json.token
      });


    // GUARDANDO A KEY

      setTimeout(() => { // we simulate the async request with timeout.
        const auth = {
          accessToken: this.token
        }
        this.$store.commit('setAuth', auth) // mutating to store for client rendering
        Cookie.set('auth', auth) // saving token in cookie for server rendering
        this.$router.push('/')
      }, 1000)
    }
  }
}
</script>

<style scoped>
label{
  display: block;
  margin-bottom: 1rem
}
</style>