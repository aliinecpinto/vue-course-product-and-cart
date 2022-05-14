<template>
  <header class="top-bar spread">
    <nav class="top-bar-nav">
      <router-link to="/" class="top-bar-link">
        <i class="icofont-spoon-and-fork"></i>
        <span>Home</span>
      </router-link>
      <router-link to="/products" class="top-bar-link">
        <span>Products</span>
      </router-link>
      <router-link to="/past-orders" class="top-bar-link">
        <span>Past Orders</span>
      </router-link>
    </nav>
    <div @click="toggleSidebar" class="top-bar-cart-link">
      <i class="icofont-cart-alt icofont-1x"></i>
      <span>Cart ({{ totalQuantity }})</span>
    </div>
  </header>
  <router-view :inventory="inventory" :addToCart="addToCart" :pastOrders="pastOrders" :getIcon="getIcon" :recommended="recommended" />
  <SidebarComponent v-if="showSidebar" :toggle="toggleSidebar" :cart="cart" :inventory="inventory" :remove="removeItem" :checkout="checkout" :getIcon="getIcon" :getProduct="getProduct" />
</template>

<script>
import SidebarComponent from '@/components/Sidebar-component.vue'
import food from './food.json'

export default {
  mounted () {
    this.getRecommendedProducts()
  },
  components: {
    SidebarComponent
  },
  data () {
    return {
      showSidebar: false,
      inventory: food,
      recommended: [],
      cart: {},
      pastOrders: []
    }
  },
  computed: {
    totalQuantity () {
      return Object.values(this.cart).reduce((acc, curr) => {
        return acc + curr
      }, 0)
    }
  },
  methods: {
    getRecommendedProducts () {
      let attempts = 0
      while (this.recommended.length < 3 && attempts < this.inventory.length) {
        const chosenProduct = this.inventory[Math.floor(Math.random() * this.inventory.length)]
        const producAlreadyExist = this.recommended.find((p) => {
          return p.name === chosenProduct.name
        })

        if (!producAlreadyExist && chosenProduct.stockQuantity > 0) {
          this.recommended.push(chosenProduct)
        } else {
          attempts += 1
        }
      }
    },
    getProduct (name) {
      const product = this.inventory.find((p) => {
        return p.name === name
      })

      return product
    },
    getIcon (iconName, size) {
      return size + ' icofont-' + iconName
    },
    addToCart (name, quantity) {
      const product = this.getProduct(name)
      if (product.stockQuantity >= quantity) {
        if (!this.cart[name]) this.cart[name] = 0
        this.cart[name] += quantity
        product.stockQuantity -= quantity
        product.showStockMessage = false

        if (product.stockQuantity === 0) {
          this.updateRecommendeds(product)
        }
      } else {
        product.showStockMessage = true
      }
    },
    updateRecommendeds (product) {
      this.recommended = this.recommended.filter(function (elem) {
        return elem.name !== product.name
      })
      this.getRecommendedProducts()
    },
    toggleSidebar () {
      this.showSidebar = !this.showSidebar
    },
    removeItem (name) {
      this.getProduct(name).stockQuantity += this.cart[name]
      delete this.cart[name]
    },
    checkout () {
      const cartArray = Object.entries(this.cart)
      cartArray.forEach(product => {
        this.inventory.forEach(element => {
          if (element.name === product[0]) {
            const elementClone = Object.assign({}, element)
            elementClone.quantity = product[1]
            this.pastOrders.push(elementClone)
          }
        })
      })
      this.cart = {}
    }
  }
}
</script>
