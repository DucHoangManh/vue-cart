<template>
  <div class="col left">
    <div class="thumbnail">
      <a href="#">
        <img class="image" :src="product.image" :alt="product.name" />
      </a>
    </div>
    <div class="detail">
      <div class="name">
        <a href="#">{{ product.name }}</a>
      </div>
      <div class="description">{{ product.description }}</div>
      <div class="price">{{ displayPrice }}</div>
    </div>
  </div>

  <div class="col right">
    <div class="quantity">
      <input
        type="number"
        class="quantity"
        step="1"
        :value="product.quantity"
        min="1"
        max="99"
        @keyup="updateQuantity"
        @blur="checkQuantity"
      />
    </div>
    <div class="remove">
      <svg
        @click="removeItem"
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
</template>
<script>
export default {
  name: 'ProductCart',
  props: {
    data: Object,
  },
  data() {
    return {
      product: this.data,
    }
  },
  computed: {
    displayPrice() {
      return this.formatCurrency(this.product.price)
    },
  },
  methods: {
    updateQuantity: function(event) {
      const value = event.target.value
      let valueInt = parseInt(value)
      if (valueInt < 1 || valueInt > 99) {
        valueInt = 1
      }
      this.product.quantity = valueInt
    },
    checkQuantity: function(event) {
      if (event.target.value === '') {
        this.product.quantity = 1
      }
    },
    removeItem: function() {
      this.$emit('onRemove', this.data.id)
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
  },
  emits: ['onRemove'],
}
</script>
<style scoped>
a {
  text-decoration: none;
  color: #333333;
}

a:hover {
  color: #b3e283;
}
nput {
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
  color: #7f7f7f;
  text-align: center;
  font: 600 1.5rem Helvetica, Arial, sans-serif;
}

.quantity > input:hover,
.quantity > input:focus {
  border-color: #b3e283;
}

.close {
  fill: #7d7d7d;
  transition: color 150ms linear, background-color 150ms linear,
    fill 150ms linear, 150ms opacity linear;
  cursor: pointer;
}

.close:hover {
  fill: #b3e283;
}
@media all and (max-width: 599px) {
  .thumbnail img {
    display: none;
  }
  .image {
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
@media all and (min-width: 600px) {
  html {
    font-size: 14px;
  }
  .image {
    height: 150px;
    width: 200px;
  }
  .container {
    width: 75%;
    max-width: 960px;
  }
  .col.left {
    display: flex;
    justify-content: space-around;
  }
}
</style>
