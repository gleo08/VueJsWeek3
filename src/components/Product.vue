<template>
        <section class="container">
      <div v-if="products.length > 0">
        <ul class="products">
          <li
            class="row"
            v-for="(product, index) in products"
            :key="product.name"
          >
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
                  @input="updateQuantity(index, $event)"
                  @blur="checkQuantity(index, $event)"
                />
              </div>

              <div class="remove">
                <svg
                  @click="removeItem(index)"
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
          </li>
        </ul>
      </div>
      <div v-else class="empty-product">
        <h3>There are no products in your cart.</h3>
        <button>Shopping now</button>
      </div>
    </section>
</template>

<script>
export default {
    name: "Product",
    props: ['products'],
    emits: ['updateQuantity', 'checkQuantity', 'removeItem'],
    methods: {
    updateQuantity: function(index, event) {
      var value = event.target.value
      console.log(value)
      console.log(index)
      var valueInt = parseInt(value)
      console.log(valueInt)

      // Minimum quantity is 1, maximum quantity is 100, can left blank to input easily
      if (value === '') {
        this.$emit('updateQuantity', index, value)
      } else if (valueInt > 0 && valueInt < 100) {
        this.$emit('updateQuantity', index, valueInt)
      }
    },
    checkQuantity: function(index, event) {
      // Update quantity to 1 if it is empty
      if (event.target.value === '') {
        this.$emit('checkQuantity', index, 1)
      }
    },
    removeItem: function(index) {
      this.$emit('removeItem', index)
    },
    priceUsd: function(value) {
            return '$' + value.toFixed(2).replace(/(\d)(?=(\d\d\d)+(?!\d))/g, "$1,");
    },
    }
}
</script>

