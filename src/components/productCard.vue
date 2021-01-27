<template>
  <div class="product">
    <span class="product__code">Код: {{+product.code}}</span>
    <span class="product__status">{{product.isActive ? 'Наличие' : 'Отсутствует'}}</span>
    <div class="product__photo">
      <img class="product__photo-img" :src="srcTo_220x220_1(product.primaryImageUrl)">
    </div>
    
    <div class="product__main">
      <div class="product__main-description">
        <a>{{product.title}}</a>
      </div>
      <div class="product__main-assoc">
        <p>Могут понадобиться:</p>
        <a class="product__main-assoc-unit"
          href="#"
          v-for="(assocUnit, i) of assocArray" :key="+product.code + '-' + i"
        >
        {{assocUnit + (i != assocArray.length - 1 ? ',' : '.')}}
        </a>
      </div>
    </div>

    <div class="product__price">
      <div class="product__price-values">
        <div class="product__price-gold">
          <span class="product__price-gold-text">По карте клуба</span>
          <div class="product__price-gold-value">
            {{state ? product.priceGold.toFixed(2) : product.priceGoldAlt.toFixed(2)}} 
            <rouble-icon class="r-icon" color="#000"></rouble-icon>
          </div>
        </div>

        <div class="product__price-retail">
          <div class="product__price-value">
            {{state ? product.priceRetail.toFixed(2) : product.priceRetailAlt.toFixed(2)}} 
            <rouble-icon class="r-icon" color="#a7a7a7"></rouble-icon>
          </div>
        </div>
        
      </div>
      <p class="product__price-bonus-text">Можно купить за {{product.bonusAmount}} балла</p>
      
      <div class="product__price-toggle">
        <div class="product__price-toggle-wrap">
          <div class="product__price-toggle-b" :class="{'product__price-toggle-b_active': !state}"
            @click="toggleState(0)"
          >
            <p>За м.кв.</p>
          </div>
          <div class="product__price-toggle-b" :class="{'product__price-toggle-b_active': state}"
            @click="toggleState(1)"
          >
            <p>За упаковку</p>
          </div>
        </div>
      </div>

      <div>
        <div class="product__unit-info">
          <img class="w-icon" src="@/assets/unit--i.png">
          <div>
            <p>Продается упаковками:</p>
            <p>1 упак. = {{+product.unitRatioAlt.toFixed(2)}} м. кв.</p>
          </div>
        </div>

        <div class="product__buy">
          <div class="product__buy-count">
            <input class="product__buy-count-input" type="number" v-model="count">
            <div class="product__buy-count-arr">
              <div class="product__buy-count-arr-u" @click="count++"></div>
              <div class="product__buy-count-arr-d" @click="count--"></div>
            </div>
          </div>
          <div class="product__buy-btn" :data-product-id="product.productId">
            <cart-icon class="product__buy-btn-icon"></cart-icon> <div>В КОРЗИНУ</div>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {
    roubleIcon: () => import('@/components/icon/roubleIcon.vue'),
    cartIcon: () => import('@/components/icon/cartIcon.vue')
  },
  props: [
    'product'
  ],
  name: 'productCard',
  data: () => ({
    state: 0,
    count: 1
  }),
  computed: {
    assocArray() {
      return this.product.assocProducts.replace(/;{1,}/g, ';').split(';').map(str => str.trim()).filter(str => str.length != 0)
    }
  },
  methods: {
    srcTo_220x220_1(src) {
      return src.substr(0, src.lastIndexOf('.')) + '_220x220_1' + src.substr(src.lastIndexOf('.'))
    },
    toggleState(state) {
      this.state = state
    }
  },
  watch: {
    count(c) {
      if (c < 0) this.count = 0
    }
  }
}
</script>

<style lang="scss">
.product {
  min-height: 320px;
  position: relative;
  display: block;
  padding: 10px 10px 18px;
  border: 1px solid #e0e0e0;

  &__code {
    font-size: 13px;
    color: #666;
    position: absolute;
    left: 245px;
    top: 10px;
  }

  &__status {
    float: right;
    cursor: pointer;
    color: #093;
    font-size: 16px;
    border-bottom: 1px dotted #093;

    &:hover {
      border-bottom: none;
    }
  }

  &__photo {
    float: left;
    width: 200px;
    height: 200px;

    &-img {
      height: 100%;
      display: block;
      margin-left: auto;
      margin-right: auto
    }
  }

  &__main-assoc-unit {
    cursor: pointer;
  }

  &__main {
    float: left;
    margin: 50px 35px 0;
    max-width: 650px;

    &-assoc p {
      display: inline-block;
      font-size: 13px;
      line-height: 16px;
      color: #545454;
      font-weight: 700;
      margin: 0;
    }

    &-assoc-unit {
      text-decoration: none;
      font-size: 13px;
      line-height: 16px;
      color: #666;
      &:hover {
        text-decoration: underline;
      }
    }

    &-description {
      margin-bottom: 30px;

      a {
        font-weight: 700;
        font-size: 14px;
        cursor: pointer;

        &:hover {  
          text-decoration: underline;
        }
      }
    } 
  }

  &__unit-info {
    position: relative;
    display: flex;
    align-items: center;
    line-height: 16px;
    width: 222px;
    margin: 10px 0 12px;
    padding: 5px 10px;
    background: #fff;
    border: 1px solid #ccc;

    &::before, &::after {
      content: ''; 
      position: absolute;
      left: 15px; bottom: -21px;
      border: 10px solid transparent;
      border-top: 10px solid #ccc;
    }

    &::after {
      border-top: 10px solid white;
      bottom: -20px; 
    }

    img {
      margin-right: 15px;
    }
  }    

  &__buy {
    display: flex;
    justify-content: space-between;
        
    &-btn {
      display: flex;
      align-items: center;
      background: #f90;
      color: #fff;
      cursor: pointer;
      padding: 10px 15px;

      &:hover {
        background: #333;
      }

      &-icon {
        margin-right: 10px;
        fill: #fff;
      }
    }

    &-count {
      display: flex;
      border: 1px solid #ccc;

      &-input {
        text-align: center;
        width: 45px;
      }

      &-arr {
        width: 25px;

        &-u, &-d {
          position: relative;
          cursor: pointer;
          border-left: 1px solid #ccc;
          height: 21px;

          &:hover {
            background: #666;
          }
        }

        &-u {
          border-bottom: 1px solid #ccc;

          &::before, &::after {
            content: ''; 
            position: absolute;
            left: 4px; top: -3px;
            border: 8px solid transparent;
            border-bottom: 8px solid #666;
          }

          &::after {
            border-bottom: 8px solid white;
            top: 0px; 
          }

          &:hover::before{
            border-bottom: 8px solid #ccc;
          }

          &:hover::after{
            border-bottom: 8px solid #666;
          }
        }

        &-d {
          border-top: 1px solid #ccc;

          &::before, &::after {
            content: ''; 
            position: absolute;
            left: 4px; bottom: -3px;
            border: 8px solid transparent;
            border-top: 8px solid #666;
          }

          &::after {
            border-top: 8px solid white;
            bottom: 0px; 
          }

          &:hover::before{
            border-top: 8px solid #ccc;
          }

          &:hover::after{
            border-top: 8px solid #666;
          }
        }
      }
    }

    &-count input{
      border: none;

      &:focus {
        outline: none;
      }

      &::-webkit-inner-spin-button, 
      &::-webkit-outer-spin-button {  
        display: none;
      }
    }

  }

  &__price {
    top: 35px;
    right: 10px;
    position: absolute;

    &-gold, &-retail {
      display: flex;
      justify-content: flex-end;
      align-items: center;
    }

    &-gold-value, &-value {
      font-size: 28px;
      font-weight: 700;
      color: #000;
    }

    &-gold-text {
      margin-right: 15px;
      width: 70px;
      font-weight: 400;
      font-size: 16px;
      display: inline-block;
      text-align: right;
      line-height: 17px;
    }

    &-value {
      color: #a7a7a7;
    }

    &-bonus-text {
      text-align: right;
      font-size: 13px;
      line-height: 17px;
      color: #999;
    }

    &-toggle {
      height: 22px;
      margin: 5px 0 0;
      display: flex;
      justify-content: flex-end;
    }

    &-toggle-b + &-toggle-b {
      margin-left: 5px;
    }

    &-toggle-b {
      float: left;
      cursor: pointer;
      padding: 3px 5px;

      p {
        padding: 0;
        font-size: 13px;
        line-height: 13px;
        color: #707070;
        border-bottom: 1px dotted #707070;
        transition: .2s;
        -webkit-transition: .2s;
      }
    }

    &-toggle-b_active {
      background: #000;
      cursor: default;

      p {
        color: #fff;
      }
    }

    &-toggle-b p:hover, &-toggle-b_active p {
      border-bottom: unset;
    }
  }
}

@media (max-width: 1249px) and (min-width: 1000px) {

  .product {

    &__photo {
      float: left;
    }

    &__main {
    margin: 35px 5px;
    max-width: 400px;
    }

    &__code {
      left: 215px;
    }

    &__status {
      position: absolute;
      left: 310px;
      top: 8px;
    }

    &__price {
      top: 10px;
    }
    
  }
}


@media (max-width: 999px) and (min-width: 0px) {
  .product {
    width: 240px;

    &__main {
      float: none;
      margin: 35px 0;
      width: 100%;
      &-assoc {
        display: none;
      }
    }

    &__code {
      position: static;
    }

    &__status {
      float: right;
    }

    &__photo {
      margin: 10px 0 0;
      width: 100%;
      float: none;
    }

    &__price {
      position: static;
      &-bonus-text {
        display: none;
      }
    }

    &__unit-info {
      display: none;
    }
  }
}

.w-icon {
  width: 15px;
  height: 15px;
}

.r-icon {
  width: 19px;
  height: 19px;
}

</style>