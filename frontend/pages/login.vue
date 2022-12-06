<template>
  <form @submit.prevent="login">
    <div>
      <CommonHeader></CommonHeader>
    </div>
    <div class="form">
      <h1>Login</h1>
      <input type="email" v-model="email" placeholder="email" required />
      <input type="password" v-model="password" placeholder="password" required />
      <div>
        <button type="submit">送信</button>
      </div>

    </div>

  </form>
</template>

<script>
import CommonHeader from '../components/CommonHeader'
export default {

  data() {
    return {
      email: null,
      password: null,
    };
  },
  components: {
    CommonHeader,
  },
  methods: {
    async login() {
      try {
        await this.$auth.loginWith("laravelJWT", {
          data: {
            email: this.email,
            password: this.password,
          },
        });
        this.$router.push("/");
      } catch {
        alert("メールアドレスまたはパスワードが間違っております");
      }
    },
  },
};
</script>

<style scoped>
h1 {
  color: black;
  padding: 10px;
}

.right p {
  margin-right: 20px;
  cursor: pointer;
}

.header {
  margin: 20px;
}

.logo {
  width: 150px;
  cursor: pointer;
}

.flex {
  display: flex;
  justify-content: space-between;
}

button {
  width: 100px;
  text-align: center;
  padding: 8px 0 10px;
  color: #fff;
  background-color: #5419da;
  border-radius: 25px;
  cursor: pointer;
}

.card {
  margin: 100px auto;
  width: 350px;
  background: #fff;
  border-radius: 5px;
  padding: 20px;
}

.card p {
  color: black;
  font-weight: bold;
  text-align: center;
}

input {
  margin-top: 15px;
  width: 50%;
  border-radius: 10px;
  padding: 10px;
  border: 1px solid black;
  color: black;
}

.form {
  text-align: center;
  background-color: white;
  width: 50%;
  margin: 0 auto;
  border-radius: 10px;
  margin-top: 100px;
}

.form button {
  margin-top: 15px;
  margin-bottom: 15px;
}
</style>