<template>
  <h2>
    產品列表頁
  </h2>
  <productModal
    ref="modal"
    :id="productId"
    :add-cart-list="addCartList"
    ></productModal>
  <table class="table">
    <thead>
      <tr>
        <th class="col-3">圖片</th>
        <th class="col-3">商品名稱</th>
        <th class="col-3">價格</th>
        <th class="col-3"></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in products" :key="item.id" class="align-middle">
        <td>
          <img :src="item.imageUrl" width="200" alt="">
        </td>
        <td>{{ item.title }}</td>
        <td>{{ item.price }}</td>
        <td>
          <button type="button" class="btn btn-primary me-1" @click="openModal(item.id)">查看更多</button>
          <button type="button" class="btn btn-danger" @click="addCartList(item.id)">加入購物車</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import productModal from '../../components/ProductModal.vue'
const { VITE_URL, VITE_PATH } = import.meta.env

export default {
  data () {
    return {
      products: [],
      productId: ''
    }
  },
  components: {
    productModal
  },
  methods: {
    getProducts () {
      this.$http.get(`${VITE_URL}/v2/api/${VITE_PATH}/products/all`)
        .then((res) => {
          this.products = res.data.products
          console.log('產品列表', this.products)
        })
    },
    openModal (id) {
      this.productId = id
    },
    addCartList (id, qty = 1) {
      const data = {
        product_id: id,
        qty
      }

      this.$http.post(`${VITE_URL}/v2/api/${VITE_PATH}/cart`, { data })
        .then((res) => {
          alert(res.data.message)
          this.$refs.modal.hide()
        })
    }
  },
  mounted () {
    this.getProducts()
    console.log(this.$refs)
  }
}
</script>
