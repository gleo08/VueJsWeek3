<template>
        <section class="container">
            <div class="col left">
              <div class="thumbnail">
                <a href="#">
                  <img :src="product.image" :alt="product.name" />
                </a>
              </div>
              <div class="detail">
                <div class="name">
                  <a href="#">{{ product.name }}</a>
                </div>
                <div class="description">{{ product.description }}</div>
                <div class="price">{{ priceUsd(product.price) }}</div>
              </div>
            </div>
            <div class="col right">
              <div class="quantity">
                <input
                  type="number"
                  class="quantity"
                  step="1"
                  :value="product.quantity"
                  @input="updateQuantity(product.id, $event)"
                  @blur="checkQuantity(product.id, $event)"
                />
              </div>
              <div class="remove">
                <svg
                  @click="showModal"
                  version="1.1"
                  class="close"
                  x="0px"
                  y="0px"
                  viewBox="0 0 60 60"
                  enable-background="new 0 0 60 60"
                  xml:space="preserve"
                >
                  <polygon
                    points="38.936,23.561 36.814,21.439 30.562,27.691 24.311,21.439 22.189,23.561 28.441,29.812 22.189,36.064 24.311,38.186 30.562,31.934 36.814,38.186 38.936,36.064 32.684,29.812"
                  ></polygon>
                </svg>
              </div>
            </div>
            <Modal v-if="isShow" @confirmModal="removeItem(product.id)" @closeModal="closeModal"/>
    </section>
</template>

<script>
import Modal from "./Modal.vue"
export default {
    name: "Product",
    data() {
      return {
        isShow: false,
      }
    },
    props: ['product'],
    emits: ['updateQuantity', 'checkQuantity', 'removeItem'],
    methods: {
    updateQuantity: function(id, event) {
      var value = event.target.value
      var valueInt = parseInt(value)
      // Minimum quantity is 1, maximum quantity is 100, can left blank to input easily
      if (value === '') {
        this.$emit('updateQuantity', id, value)
      } else if (valueInt > 0 && valueInt < 100) {
        this.$emit('updateQuantity', id, valueInt)
      }
    },
    checkQuantity: function(id, event) {
      // Update quantity to 1 if it is empty
      if (event.target.value === '') {
        this.$emit('checkQuantity', id, 1)
      }
    },
    removeItem: function(id) {
      this.isShow = false;
      this.$emit('removeItem', id)
    },
    priceUsd: function(value) {
            return '$' + value.toFixed(2).replace(/(\d)(?=(\d\d\d)+(?!\d))/g, "$1,");
    },
    showModal: function() {
      this.isShow = true;
    },
    closeModal: function() {
      this.isShow = false;
    }
    },
    components: {
      Modal,
    }
}
</script>

