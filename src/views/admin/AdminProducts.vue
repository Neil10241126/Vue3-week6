<template>
  後台產品列表
  <table class="table">
    <thead>
      <tr>
        <th>分類</th>
        <th>產品名稱</th>
        <th>原價</th>
        <th>售價</th>
        <th>是否啟用</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in adminProducts" :key="item.id">
        <td>{{ item.category }}</td>
        <td>{{ item.title }}</td>
        <td>{{ item.origin_price }}</td>
        <td>{{ item.price }}</td>
        <td>
          <span v-if="item.is_enabled" :class="{'text-success': item.is_enabled}">啟用</span>
          <span v-else class="{'text-danger': !item.is_enabled}">未啟用</span>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
const { VITE_URL, VITE_PATH } = import.meta.env

export default {
  data () {
    return {
      adminProducts: []
    }
  },
  methods: {
    checkLogin () {
      // 取出 token
      const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1')
      this.$http.defaults.headers.common.Authorization = token

      this.$http.post(`${VITE_URL}/v2/api/user/check`)
        .then(() => {
          this.getAdminProducts()
        })
        .catch(() => {
          this.$router.push('/login')
        })
    },
    getAdminProducts () {
      this.$http.get(`${VITE_URL}/v2/api/${VITE_PATH}/products/all`)
        .then((res) => {
          this.adminProducts = res.data.products
          console.log('後台產品:', this.adminProducts)
        })
    }
  },
  mounted () {
    this.checkLogin()
  }
}
</script>
