<template>
  <h2>
    購物車
  </h2>
  <div class="d-flex justify-content-end">
    <button type="button" class="btn btn-outline-danger me-4 mb-3"
      @click="deleteCart()">
      清空購物車
    </button>
  </div>
  <table class="table align-middle">
    <thead>
      <tr>
        <th></th>
        <th style="padding-left: 65px">圖片</th>
        <th class="text-center">品名</th>
        <th style="width: 150px">數量/單位</th>
        <th class="text-center">價格</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in cart.carts" :key="item.id">
        <td>
          <button type="button" class="btn-close border border-danger" aria-label="Close"
            @click="deleteCartItem(item.id)"></button>
        </td>
        <td>
          <img :src="item.product.imageUrl" width="150" alt="">
        </td>
        <td class="text-center">{{ item.product.title }}</td>
        <td>
          <div class="input-group input-group-sm">
            <input type="number" class="form-control" min="1"
              v-model.number="item.qty"
              @change="changeCartNum(item.id, item.qty)">
            <span class="input-group-text">{{ item.product.unit }}</span>
          </div>
        </td>
        <td class="text-center">{{ item.final_total }}</td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td colspan="4" class="text-end">總計 :</td>
        <td class="text-center">{{ cart.final_total }}</td>
      </tr>
    </tfoot>
  </table>
</template>

<script>
const { VITE_URL, VITE_PATH } = import.meta.env

export default {
  data () {
    return {
      cart: {}
    }
  },
  methods: {
    getCartList () {
      this.$http.get(`${VITE_URL}/v2/api/${VITE_PATH}/cart`)
        .then((res) => {
          this.cart = res.data.data
          console.log('購物車列表:', this.cart)
        })
    },
    changeCartNum (id, qty) {
      const data = {
        product_id: id,
        qty
      }

      this.$http.put(`${VITE_URL}/v2/api/${VITE_PATH}/cart/${id}`, { data })
        .then(() => {
          this.getCartList()
        })
    },
    deleteCart () {
      this.$http.delete(`${VITE_URL}/v2/api/${VITE_PATH}/carts`)
        .then((res) => {
          this.getCartList()
          alert(res.data.message)
        })
    },
    deleteCartItem (id) {
      this.$http.delete(`${VITE_URL}/v2/api/${VITE_PATH}/cart/${id}`)
        .then((res) => {
          this.getCartList()
          alert(res.data.message)
        })
    }
  },
  mounted () {
    this.getCartList()
  }
}
</script>
