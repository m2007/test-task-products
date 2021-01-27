<template>
  <div id="app">
    <div v-if="dataReady">
      <product-card class="ma-20" v-for="pr of products" :product="pr" :key="pr.code"></product-card>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
    productCard: () => import('@/components/productCard.vue')
  },
  data: () => ({
    dataReady: false
  }),
  async created() {
    const products = await require('./assets/products.json')

    products.filter(function(item, pos) {
        return products[products.indexOf(item)].code != products[pos].code;
    })

    this.products = products
    this.dataReady = true
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  font-family: Arial,Helvetica,sans-serif;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  color: #222;
  font-size: 14px;
  line-height: 1.4;
}

#app {
  padding: 10px 20px;
}

.ma-20 {
  margin: 20px;
}

</style>
