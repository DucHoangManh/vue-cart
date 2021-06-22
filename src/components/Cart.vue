<template>
  <div id="app">
    <!-- Header -->
    <header class="container">
      <h1>Shopping Cart</h1>
      <ul class="breadcrumb">
        <li>Home</li>
        <li>Shopping Cart</li>
      </ul>
      <span class="count">{{ itemCount }} items in the bag</span>
    </header>
    <!-- End Header -->

    <!-- Product List -->
    <section class="container">
      <div v-if="products.length > 0">
        <ul class="products">
          <li class="row" v-for="product in products" :key="product.name">
            <CartItem :data="product" @onRemove="removeProduct" />
          </li>
        </ul>
      </div>
      <div v-else class="empty-product">
        <h3>There are no products in your cart.</h3>
        <button @click="revertProducts">Shopping now</button>
      </div>
    </section>
    <!-- End Product List -->

    <!-- Summary -->
    <section class="container" v-if="products.length > 0">
      <div class="promotion">
        <label for="promo-code">Have A Promo Code?</label>
        <input type="text" id="promo-code" v-model="promoCode" />
        <button type="button" @click="checkPromoCode"></button>
      </div>

      <div class="summary">
        <ul>
          <li>
            Subtotal <span>{{ displaySubTotal }}</span>
          </li>
          <li v-if="discount > 0">
            Discount <span>{{ displayDiscountPrice }}</span>
          </li>
          <li>
            Tax <span>{{ displayTax }}</span>
          </li>
          <li class="total">
            Total <span>{{ displayTotalPrice }}</span>
          </li>
        </ul>
      </div>

      <div class="checkout">
        <button type="button">Check Out</button>
      </div>
    </section>
    <!-- End Summary -->
  </div>
</template>
<script>
import CartItem from './CartItem.vue'
export default {
  name: 'cart',
  components: {
    CartItem,
  },
  data() {
    return {
      productsOrigin: [
        {
          id: 1,
          image:
            'https://magzentine.com/wp-content/uploads/2021/04/Ez0oHUqXsAELSQa-780x470.jpg',
          name: 'Shady moon approaching',
          description: 'Description for product a product with id 1',
          price: 5.99,
          quantity: 2,
        },
        {
          id: 2,
          image: 'https://plclagi.com/wp-content/uploads/2020/11/00-8.jpg',
          name: "L'Art de la peinture",
          description: "L'Art de la peinture by Johannes Vermeer",
          price: 9.99,
          quantity: 1,
        },
      ],
      promotions: [
        {
          code: 'SUMMER',
          discount: '50%',
        },
        {
          code: 'AUTUMN',
          discount: '40%',
        },
        {
          code: 'WINTER',
          discount: '30%',
        },
      ],
      products: [],
      promoCode: '',
      discount: 0,
    }
  },
  mounted() {
    this.products = this.productsOrigin
  },
  computed: {
    tax() {
      return this.subTotal / 10
    },
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
    totalPrice: function() {
      console.log(
        this.formatCurrency(this.subTotal - this.discountPrice + this.tax)
      )
      return this.subTotal - this.discountPrice + this.tax
    },
    displayTotalPrice() {
      return this.formatCurrency(this.totalPrice)
    },
    displayTax() {
      return this.formatCurrency(this.tax)
    },
    displayDiscountPrice() {
      return this.formatCurrency(this.discountPrice)
    },
    displaySubTotal() {
      return this.formatCurrency(this.subTotal)
    },
  },
  methods: {
    removeProduct: function(productId) {
      this.products = this.products.filter((product) => {
        return product.id !== productId
      })
    },
    checkPromoCode: function() {
      for (var i = 0; i < this.promotions.length; i++) {
        if (this.promoCode === this.promotions[i].code) {
          this.discount = parseFloat(
            this.promotions[i].discount.replace('%', '')
          )
          return
        }
      }

      alert('Sorry, the Promotional code you entered is not valid!')
    },
    formatCurrency(num) {
      const formatter = new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2,
      })
      return formatter.format(num)
    },
    revertProducts() {
      this.products = this.productsOrigin
    },
  },
}
</script>
<style scoped>
* {
  box-sizing: border-box;
}
#CartItem {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
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
  color: #b3e283;
}

button {
  background-color: #e99497;
  border: 2px solid #e99497;
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
  background-color: #b3e283;
  border-color: #b3e283;
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
  border: 2px solid #e99497;
  border-radius: 2rem 0 0 2rem;
}

.promotion:hover > input {
  border-color: #b3e283;
}

.promotion > button {
  float: left;
  width: 20%;
  height: 2.4rem;
  border-radius: 0 2rem 2rem 0;
}

.promotion:hover > button {
  border-color: #b3e283;
  background-color: #b3e283;
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
