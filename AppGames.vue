<template>
<div class="catalogue" >
  <div class="catalogue-content flex">
    <div class="catalogue-wrapper flex">
      <div class="select-order-block flex"> <MySelect @changeSelect="changeOrder">
        <option value="new">Новинки</option>
        <option value="old">Старые игры</option>
        <option value="popular">Популярные</option>
        <option value="name">По названию</option>
        <option value="price_asc">Цена: по возрастанию</option>
        <option value="price_desc">Цена: по убыванию</option>
      </MySelect>
      </div>
      <template v-if="games">
    <ul class="catalogue-cards flex"><gameCard class='cat-card' :game="game" :key='index' v-for="(game, index) in games"></gameCard></ul>
      </template>
      <span v-else class="not-found" >По вашему запросу ничего не нашлось :(</span>
      <app-games-pagination></app-games-pagination>
    </div>
    <app-games-filters></app-games-filters>

  </div>

  <!-- <ul class="flex pagination" :key="page"><li  :key="index"  v-for="index in currentData['len']"><a :class="{'pagination-number--active': page == index}" class="pagination-number" :key='page' @click="changePage(index)">{{index}}</a></li></ul> -->

</div>
</template>

<script>
import {mapGetters, mapActions} from "vuex";
import gameCard from "@/components/gameCard";
import MySelect from "@/components/UI/MySelect";
import AppGamesPagination from "@/../../../../../../WebstormProjects/wishbook-vue/src/AppGames/appGamesPagination";
import AppGamesFilters from "@/../../../../../../WebstormProjects/wishbook-vue/src/AppGames/appGamesFilters";
export default {
  name: "AppGames",
  components: {AppGamesPagination, gameCard, AppGamesFilters, MySelect},
  data() {
    return {
      page: null,
      params: {},
      search: "",
      genres: [],
      platforms: [],
      publishers: [],
      developers: [],
      loaded: false

    }
  },
  computed: {
    ...mapGetters(['minPrice', 'maxPrice', 'games', 'totalGames', 'getParams', 'minPriceQuery', 'maxPriceQuery'])}
  ,
  methods: {
    ...mapActions([ "fetchGames", 'toggleItemAction']),
    togglePreOrder(){if (this.params['preorder'] === 1){
      this.params['preorder'] = 0
    }
    else{
      this.params['preorder'] = 1
    }
      this.refresh(true)},
    resetPrice(){
      this.$store.dispatch('games/resetPrice')
    },
    sendPriceParams(){
      this.params['min_price'] = this.minPrice
      this.params['max_price'] = this.maxPrice
    },
    delayedRefresh(timeout){
      clearInterval(this.timer)
      this.timer = setTimeout(() => {this.refresh(true)}, timeout)
    },
    updatePrice(price) {
      this.minPrice = price.minValue
      this.maxPrice = price.maxValue
      this.sendPriceParams()
      this.delayedRefresh(500)

    },
    refresh(drop) {
      window.scrollTo(0, 100)
      if (drop === true) {
        this.params['page'] = 1
        this.page = 1
      }
      this.$router.replace({query: 'hello'})
      console.log(this.getParams)
      console.log(this.params)
    },

    geCurrentParams() {
      let genres = []
      let platforms = []
      let publishers = []
      let developers = []
      let order = this.$route.query.order_by
      let text = this.$route.query.text
      let min_price = this.$route.query.min_price
      let discount = parseInt(this.$route.query.discount)
      let preorder = parseInt(this.$route.query.preorder)
      if (!preorder){
        preorder = 0
      }
      if (!discount){
        discount = 0
      }

      platforms = platforms.concat(this.$route.query.platform)
      genres = genres.concat(this.$route.query.genre)
      developers = developers.concat(this.$route.query.developer)
      publishers = publishers.concat(this.$route.query.publisher)

      const max_price = this.$route.query.max_price
      let page = this.$route.query.page
      if (!page) {
        page = 1
      }
      const out = {
        'platform': platforms,
        'order_by': order,
        'genre': genres,
        'discount': discount,
        'developer': developers,
        'publisher': publishers,
        'min_price': min_price,
        'max_price': max_price,
        'page': page,
        'text': text,
        'preorder': preorder
      }
      console.log('imhere')
      console.log(out)
      return out
    },
    changeMinPrice(event){
      const min = parseInt(event.target.value)
      clearInterval(this.timer)
      this.timer = setInterval(() => {
        if (min > this.maxPrice){
          this.minPrice = this.maxPrice
        }
        else if(min < this.currentData['price']['min_price']){
          this.minPrice = this.currentData['price']['min_price']
        }
        else{
          this.minPrice = min}}, 500)}
    ,
    changeMaxPrice(event) {
      const max = parseInt(event.target.value)
      clearInterval(this.timer)
      this.timer = setInterval( () =>
      {if (max < this.minPrice) {
        this.maxPrice = this.minPrice
      }
      else if (max > this.currentData['price']['max_price']) {
        this.maxPrice = this.currentData['price']['max_price']
      }
      else{
        this.maxPrice = max
      }}, 500)
    },
    changeOrder(value) {
      this.params['order_by'] = value
      this.refresh()
    },
    searchInput(text) {
      this.params['text'] = text
      this.refresh(true)
    },
    initData() {
     return

    }

  },
  created() {
    this.fetchGames()
    console.log(this.games)
    this.loaded = true
    this.$route.query = 'hello'
    this.page = parseInt(this.params['page'])
    this.refresh()
  },
  mounted() {
    console.log(this.$route)
    if (this.$route.query.order_by) {
      document.querySelector(`.order-option[value=${this.$route.query.order_by}]`).setAttribute('selected', 'selected')
    }

  },
  watch: {
    $route() {
      this.setParams(this.geCurrentParams())
      console.log(this.params)
    },
    getParams(){
      this.refresh()
    },
    search() {
      clearTimeout(this.timer)
      this.timer = setTimeout(() => {
        this.searchInput(this.search)
      }, 500)
    },
    page() {
      this.params['page'] = this.page
      this.refresh()
    },

}}
</script>
<style>


.select-order-block{
  align-self: flex-start;
}


</style>
<style scoped>

.not-found{
  text-align: left;
  width: 958px;
  margin-right: 75px;
}








.price-input{
  width: 100px;
  background-color: #201F1F;
  color: #E7E7E7;

  border: #6C6767 2px solid;
  border-radius: 25px;
  padding: 5px 15px ;
}

.price-input::-webkit-outer-spin-button,
.price-input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.cat-form svg{
  margin-right: 8px;
}






.multi-range-slider .--webkitthumb::before{
  content: "";
  background-color: white;
  position: absolute;
  width: 12px;
  height: 12px;
  border: none;
  box-shadow: none;
  border-radius: 50%;
  z-index: 1;
  margin: -8px;
  cursor: pointer;
  top: 6px;
}

.multi-range-slider .thumb::before{
  content: "";
  background-color: white;
  position: absolute;
  width: 12px;
  height: 12px;
  border: none;
  box-shadow: none;
  border-radius: 50%;
  z-index: 1;
  margin: -8px;
  cursor: pointer;
  top: 6px;
}





.filters-check:checked + label:after{
}



.catalogue{
  margin-top: 275px;
  padding: 0 120px;
}

.catalogue-wrapper{
  flex-direction: column;
}



.catalogue-content{
  width: inherit;
  flex-direction: row;
  justify-content: center;
  margin-bottom: 91px;

}

.catalogue-cards{
  justify-content: flex-start;
  flex-wrap: wrap;
  column-gap: 35px;
  row-gap: 35px;
  margin-right: 75px;
  width: 958px;

}



.cat-card{
}


.cat-card:nth-child(4n){
  margin-right: 0;
}

</style>

<style>
.pagination-container{
  justify-content: center;
  margin-bottom: 154px;

}
.paginate-buttons{
  border: none;
  background-color: transparent;
  margin-right: 13px;
  text-decoration: none;
  cursor: pointer;
  color: #7D7D7D;
}

.active-page{
  color: white;
  border-bottom: white 1px solid;
}
</style>
