<template>
  <!-- Modal -->
  <div class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
    ref="modal">
    <div class="modal-dialog modal-xl">
      <div class="modal-content">
        <div class="modal-header bg-dark text-white">
          <h5 class="modal-title" id="exampleModalLabel">{{ tempProduct.title }}</h5>
          <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="col-sm-6">
              <img class="img-fluid" :src="tempProduct.imageUrl" alt="">
            </div>
            <div class="col-sm-6">
              <p>商品描述: {{ tempProduct.description }}</p>
              <p>商品內容: {{ tempProduct.content }}</p>
              <h5 style="text-decoration:line-through;">原價: {{ tempProduct.origin_price }}</h5>
              <h5>售價: {{ tempProduct.price }}</h5>
              <div class="input-group">
                <select name="" id="" class="form-select" v-model.number="qty">
                  <option v-for="i in 20" :key="i+123" :value="i">{{ i }}</option>
                </select>
                <button class="btn btn-danger" @click="addCartList(tempProduct.id, qty)">加入購物車</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Modal from 'bootstrap/js/dist/modal'
const { VITE_URL, VITE_PATH } = import.meta.env

export default {
  props: ['id', 'addCartList'],
  data () {
    return {
      modal: [],
      tempProduct: {},
      qty: 1
    }
  },
  methods: {
    hide () {
      this.modal.hide()
    }
  },
  watch: {
    id () {
      this.$http.get(`${VITE_URL}/v2/api/${VITE_PATH}/product/${this.id}`)
        .then((res) => {
          this.tempProduct = res.data.product
          this.modal.show()
          console.log('單一產品', this.tempProduct)
        })
    }
  },
  mounted () {
    this.modal = new Modal(this.$refs.modal)
  }
}
</script>
