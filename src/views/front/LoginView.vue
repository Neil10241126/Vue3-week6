<template>
    <div class="row justify-content-center mt-4">
      <h1 class="h3 mb-3 font-weight-normal text-center text-success">
        請先登入
      </h1>
      <div class="col-4">
        <form id="form" class="form-signin">
          <div class="form-floating mb-3">
            <input type="email" class="form-control" id="username"
              v-model="user.username"
              placeholder="name@example.com" required autofocus>
            <label for="username">Email address</label>
          </div>
          <div class="form-floating">
            <input type="password" class="form-control" id="password"
              v-model="user.password"
              placeholder="Password" required>
            <label for="password">Password</label>
          </div>
          <button class="btn btn-lg btn-success w-100 mt-3" type="button"
            @click="login()">
            登入
          </button>
        </form>
      </div>
    </div>
    <p class="mt-5 mb-3 text-center">
      &copy; 2021~∞ - 六角學院
    </p>
</template>

<script>
const { VITE_URL } = import.meta.env

export default {
  data () {
    return {
      user: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    login () {
      this.$http.post(`${VITE_URL}/v2/admin/signin`, this.user)
        .then((res) => {
          alert(res.data.message)
          const { expired, token } = res.data

          document.cookie = `hexToken=${token}; expires=${new Date(expired)};`
          this.$router.push('/admin/products')
        })
    }
  }
}
</script>
