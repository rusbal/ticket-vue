<template>
  <div class="container">
    <div>
      <h2>Quantity: {{ quantity("stocks") }}</h2>
      <h2>Price total : {{ total("stocks") }}</h2>
      <ul>
        <li v-for="item in stocks" :key="item.message">
          <div>
            Name: {{ item.name }}

            <button @click="addToBasket(item)">Add to basket</button>
            <button @click="deleteToy(item)">Delete</button>
            <br />

            Available: {{ item.quantity }}
            <br />

            Price: {{ item.price }}
          </div>
        </li>
      </ul>
    </div>
    <div>
      <h2>Quantity: {{ quantity("basket") }}</h2>
      <h2>Basket total: {{ total("basket") }}</h2>
      <ul>
        <li v-for="item in basket" :key="item.message">
          <div>
            Name: {{ item.name }}

            <button @click="removeFromBasket(item.id)">Remove from basket</button>
            <br />

            Available: {{ item.quantity }}
            <br />

            Price: {{ item.price }}
          </div>
        </li>
      </ul>
    </div>
  </div>
  <button @click="addStock">Add Toy</button>
</template>

<script>
export default {
  data () {
    return {
      stocks: [],
      basket: [],
    }
  },
  mounted () {
    for (let x = 0; x < 3; x++) {
      this.addStock()
    }
  },
  methods: {
    randomId () {
      return Math.random() * 1000000000000000000
    },
    addStock () {
      this.stocks.push({
        name: (Math.random() + 1).toString(36).substring(7),
        price: Math.floor(Math.random()*(100) + 10),
        quantity: Math.floor(Math.random()*(10) + 2)
      })
    },
    addToBasket (toy) {
      const selectedToy = { ...toy, id: this.randomId() }
      this.basket.push(selectedToy)
    },
    deleteToy (item) {
      this.stocks.splice(this.stocks.indexOf(item), 1)
      this.deleteToyFromBasket(item.name)
    },
    removeFromBasket (id) {
      this.basket = this.basket.filter(item => {
        return item.id !== id
      })
    },
    deleteToyFromBasket (name) {
      this.basket = this.basket.filter(item => {
        return item.name !== name
      })
    },
    total (thing) {
      if (this[thing].length === 0) return 0
      return this[thing].map(toy => toy.price * toy.quantity).reduce((prev, next) => prev + next)
    },
    quantity (thing) {
      if (this[thing].length === 0) return 0
      return this[thing].map(toy => toy.quantity).reduce((prev, next) => prev + next)
    }
  }
}
</script>

<style>
.container {
  display: flex;
}
</style>
