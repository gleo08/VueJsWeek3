<template>
  <div id="app">
    <header class="container">
      <h1>Shopping Cart</h1>
      <ul class="breadcrumb">
        <li>Home</li>
        <li>Shopping Cart</li>
      </ul>
      <span class="count">{{ itemCount }} items in the bag</span>
    </header>
    <div v-if="!products.length" class="empty-product">
      <h3>There are no products in your cart.</h3>
      <button @click="reloadProduct">Shopping now</button>
    </div>
    <Product :products="products" @updateQuantity="updateQuantity" @checkQuantity="checkQuantity" @removeItem="removeItem"/>
    <section class="container" v-if="products.length > 0">
      <div class="promotion">
        <label for="promo-code">Have A Promo Code?</label>
        <input type="text" id="promo-code" v-model="promoCode" />
        <button type="button" @click="checkPromoCode"></button>
      </div>

      <div class="summary">
        <ul>
          <li>
            Subtotal <span>{{ priceUsd(subTotal) }}</span>
          </li>
          <li v-if="discount > 0">
            Discount <span>{{ priceUsd(discountPrice) }}</span>
          </li>
          <li>
            Tax <span>{{ priceUsd(taxValue) }} </span>
          </li>
          <li class="total">
            Total <span>{{ priceUsd(totalPrice) }}</span>
          </li>
        </ul>
      </div>
      <div class="checkout">
        <button type="button">Check Out</button>
      </div>
    </section>
</div>
</template>

<script>
import rolex1 from "./assets/rolex1.jpg"
import rolex2 from "./assets/rolex2.jpg"
import Product from "./components/Product.vue"
export default {
    name: 'App',
    data() {
    return {
      products: [
        {
          id: 1,
          image: rolex1,
          name: 'Rolex Day-Date 228235',
          description: '40mm in Rose Gold',
          price: 61723,
          quantity: 2,
        },
        {
          id: 2,
          image: rolex2,
          name: 'Rolex Day-Date 228239',
          description: '40mm in White Gold',
          price: 55000,
          quantity: 1,
        },
      ],
      tax: 10,
      promotions: [
        {
          code: 'SPRING',
          discount: '10%',
        },
        {
          code: 'SUMMER',
          discount: '20%',
        },
        {
          code: 'AUTUMN',
          discount: '20%',
        },
        {
          code: 'WINTER',
          discount: '30%',
        },
      ],
      promoCode: '',
      discount: 0,
      productList: []
    }
  },

  created() {
    this.productList = this.products
  },

    computed: {
    itemCount: function() {
      var count = 0

      for (var i = 0; i < this.products.length; i++) {
        count += parseInt(this.products[i].quantity) || 0
      }

      return count
    },
    subTotal: function() {
      var subTotal = 0

      for (var i = 0; i < this.products.length; i++) {
        subTotal += this.products[i].quantity * this.products[i].price
      }

      return subTotal
    },
    discountPrice: function() {
      return (this.subTotal * this.discount) / 100
    },
    taxValue: function() {
      return (this.tax / 100)* this.subTotal
    },
    totalPrice: function() {
      return this.subTotal - this.discountPrice + this.taxValue
    }
    },
    methods: {
        updateQuantity: function(...data) {
            const [id, value] = data
            for (let i = 0; i < this.products.length; i++) {
              if (this.products[i].id == id) {
                this.products[i].quantity = value
              }
            }
        },
        checkQuantity: function(...data) {
            const [id, quantity] = data
            for (let i = 0; i < this.products.length; i++) {
              if (this.products[i].id == id) {
                this.products[i].quantity = quantity
              }
            }
        },
        removeItem: function(id) {
            this.products = this.products.filter(
              (product) => product.id !== id
            );
        },
        checkPromoCode: function() {
            for (var i = 0; i < this.promotions.length; i++) {
                if (this.promoCode === this.promotions[i].code) {
                    this.discount = parseFloat(this.promotions[i].discount.replace('%', ''))
                    return
                }
            }
            alert('Sorry, the Promotional code you entered is not valid!')
        },
        priceUsd: function(value) {
            return '$' + value.toFixed(2).replace(/(\d)(?=(\d\d\d)+(?!\d))/g, "$1,");
        },
        reloadProduct: function() {
          this.products = this.productList;
        }
    },
    components: {
        Product,
    }
}
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-size: 12px;
}

body {
  margin: 20px 0;
  padding: 0;
  font-family: arial, sans-serif;
  overflow: scroll;
}

img {
  max-width: 100%;
  vertical-align: middle;
  border-radius: 4px;
}

a {
  text-decoration: none;
  color: #333333;
}

a:hover {
  color: #181907;
}

button {
  background-color: #181907;
  border: 2px solid #181907;
  color: #ffffff;
  transition: all 0.25s linear;
  cursor: pointer;
}

button::after {
  position: relative;
  right: 0;
  content: ' \276f';
  transition: all 0.15s linear;
}

button:hover {
  background-color: #0B610B;
  border-color: #0B610B;
}

button:hover::after {
  right: -5px;
}

button:focus {
  outline: none;
}

ul {
  padding: 0;
  margin: 0;
  list-style-type: none;
}

input {
  transition: all 0.25s linear;
}

input[type='number']::-webkit-inner-spin-button,
input[type='number']::-webkit-outer-spin-button {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  margin: 0;
}

input {
  outline: none;
}

.container {
  width: 90%;
  margin: 0 auto;
  overflow: auto;
}

/* --- HEADER --- */
header.container {
  margin-bottom: 1.5rem;
}

header .breadcrumb {
  color: #7d7d7d;
}

header .breadcrumb li {
  float: left;
  padding: 0 6px;
  height: 20px;
  line-height: 20px;
}

header .breadcrumb li:first-child {
  padding-left: 2px;
}

header .breadcrumb li:not(:last-child)::after {
  content: ' \276f';
  padding-left: 8px;
}

header .count {
  float: right;
  color: #333333;
  height: 20px;
  line-height: 20px;
}

/* --- PRODUCT LIST --- */
.products {
  border-top: 1px solid #ddd;
}

.products > li {
  padding: 1rem 0;
  border-bottom: 1px solid #ddd;
}

.row {
  position: relative;
  overflow: auto;
  width: 100%;
}

.col,
.quantity,
.remove {
  float: left;
}

.col.left {
  width: 70%;
}

.col.right {
  width: 30%;
  position: absolute;
  right: 0;
  top: calc(50% - 30px);
}

.detail {
  padding: 0 0.5rem;
  line-height: 2.2rem;
}

.detail .name {
  font-size: 1.2rem;
}

.detail .description {
  color: #7d7d7d;
  font-size: 1rem;
}

.detail .price {
  font-size: 1.5rem;
}

.quantity,
.remove {
  width: 50%;
  text-align: center;
}

.remove svg {
  width: 60px;
  height: 60px;
}

.quantity > input {
  display: inline-block;
  width: 60px;
  height: 60px;
  position: relative;
  left: calc(50% - 30px);
  background: #fff;
  border: 2px solid #ddd;
  color: #181907;
  text-align: center;
  font: 600 1.5rem Helvetica, Arial, sans-serif;
}

.quantity > input:hover,
.quantity > input:focus {
  border-color: #181907;
}

.close {
  fill: #7d7d7d;
  transition: color 150ms linear, background-color 150ms linear,
    fill 150ms linear, 150ms opacity linear;
  cursor: pointer;
}

.close:hover {
  fill: #181907;
}

/* --- SUMMARY --- */
.promotion,
.summary,
.checkout {
  float: left;
  width: 100%;
  margin-top: 1.5rem;
}

.promotion > label {
  float: left;
  width: 100%;
  margin-bottom: 1rem;
}

.promotion > input {
  float: left;
  width: 80%;
  font-size: 1rem;
  padding: 0.5rem 0 0.5rem 1.8rem;
  border: 2px solid #181907;
  border-radius: 2rem 0 0 2rem;
}

.promotion:hover > input {
  border-color: #0B610B;
}

.promotion > button {
  float: left;
  width: 20%;
  height: 2.4rem;
  border-radius: 0 2rem 2rem 0;
}

.promotion:hover > button {
  border-color: #0B610B;
  background-color: #0B610B;
}

.promotion > button::after {
  content: '\276f';
  font-size: 1rem;
}

.summary {
  font-size: 1.2rem;
  text-align: right;
}

.summary ul li {
  padding: 0.5rem 0;
}

.summary ul li span {
  display: inline-block;
  width: 30%;
}

.summary ul li.total {
  font-weight: bold;
}

.checkout {
  text-align: right;
}

.checkout > button {
  font-size: 1.2rem;
  padding: 0.8rem 2.8rem;
  border-radius: 1.5rem;
}

.empty-product {
  text-align: center;
}

.empty-product > button {
  font-size: 1.3rem;
  padding: 10px 30px;
  border-radius: 5px;
}

/* --- SMALL SCREEN --- */
@media all and (max-width: 599px) {
  .thumbnail img {
    display: none;
  }

  .quantity > input {
    width: 40px;
    height: 40px;
    left: calc(50% - 20px);
  }

  .remove svg {
    width: 40px;
    height: 40px;
  }
}

/* --- MEDIUM & LARGE SCREEN --- */
@media all and (min-width: 600px) {
  html {
    font-size: 14px;
  }

  .container {
    width: 75%;
    max-width: 960px;
  }

  .thumbnail,
  .detail {
    float: left;
  }

  .thumbnail {
    width: 35%;
  }

  .detail {
    width: 65%;
  }

  .promotion,
  .summary {
    width: 50%;
  }

  .checkout {
    width: 100%;
  }

  .checkout,
  .summary {
    text-align: right;
  }
}

/* --- LARGE SCREEN --- */
@media all and (min-width: 992px) {
  html {
    font-size: 16px;
  }
}
</style>
