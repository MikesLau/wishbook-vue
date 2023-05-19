<template>
  <!--  TODO Починить синхронизацию данных с фильтра-->
  <div>
    <MultiRangeSlider min-caption=""
                      max-caption=""
                      v-if="true"
                      :label="false"
                      :min-value='min'
                      :max-value='max'
                      @input="updatePriceQuery"
                      base-class-name="multi-range-slider"
                      :min='minPrice'
                      :max='maxPrice' :ruler="false">

    </MultiRangeSlider>
    <div v-if="true" class="price-block flex">
      <div class="flex price-input-block"><span>от</span><input class='price-input cat-form-input' :key="min" :max="max"  :min='minPrice' @input='changeMinPrice' :value='min' type="number"><span>₽</span> </div>
      <div class="flex price-input-block"><span>до</span><input class='search-form-input price-input' type="number" :min='min' @input='changeMaxPrice' :value="max" :max='maxPrice'><span>₽</span></div>
    </div>
  </div>

</template>

<script>
import MultiRangeSlider from "multi-range-slider-vue";
import {mapActions, mapGetters} from "vuex";
export default {
  name: "AppGamesPriceFilter",
  data(){
    return {
      min: 0,
      max: 0,
      minPriceQuery: 0,
      maxPriceQuery: 0,
    }
  },
  computed: {
    ...mapGetters(['minPrice', 'maxPrice', 'games', 'totalGames'])
  },
  methods:{
    ...mapActions(['setMinPriceQuery', 'setMaxPriceQuery', 'updatePriceQuery']),
    updatePriceQuery(e){
      this.min = e.minValue
      this.max = e.maxValue
      this.$store.dispatch('updatePriceQuery', [this.min, this.max])
    },
    changeMinPrice(event){
      const current = parseInt(event.target.value)
      clearInterval(this.timer)
      this.timer = setTimeout(() => {
        if (current > this.max){
          this.min = this.max
          this.setMinPriceQuery(this.min)
        }
        else if(current < this.minPrice){
          this.min = this.minPrice
          this.setMinPriceQuery(this.min)

        }
        else{
          this.min = current
          this.setMinPriceQuery(this.min)}}, 500)}
    ,
    changeMaxPrice(event){
      const current = parseInt(event.target.value)
      clearInterval(this.timer)
      this.timer = setTimeout(() => {
        if (current < this.min){
          this.max = this.min
          this.setMaxPriceQuery(this.max)
        }
        else if(current > this.maxPrice){
          this.max = this.maxPrice
          this.setMaxPriceQuery(this.max)
        }
        else{
          this.max = current
          this.setMaxPriceQuery(this.max)}}, 500)
}
  }
  , components: {MultiRangeSlider},
  created(){
    this.min = this.$store.getters['minPriceQuery'] ? this.$store.getters['minPriceQuery'] : this.minPrice
    this.max = this.$store.getters['maxPriceQuery'] ? this.$store.getters['maxPriceQuery'] : this.maxPrice
  }

}
</script>
<style>
.multi-range-slider .bar .bar-left{
  background-color: #585858;
  border-radius: 17px;
  box-shadow: none;
  height: 5px;
  padding: 0;
}

.multi-range-slider .bar .bar-right {
  background-color: #585858;
  border-radius: 17px;
  box-shadow: none;
  height: 5px;
}

.multi-range-slider .bar-inner{
  background-color: white;
  height: 5px;
  box-shadow: none;
  border: none
}

.multi-range-slider .thumb::before{
  border: none;
  box-shadow: 0px 0px 5px rgba(255, 255, 255, 0.56);
  height: 7px;
  width: 7px;
  top: 7px;
  left: 7px
}

.multi-range-slider .thumb .caption{
  visibility: hidden;
}
.price-input-block{
  padding: 5px 15px;
  align-items: center;
  justify-content: space-between;
  row-gap: 10px;
}

.price-block{
  flex-direction: column;
}
.multi-range-slider{
  background-color: transparent;
  border:none;
  box-shadow: none;
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



.bar-right{
  background-color: #f0f0f0
}
</style>
<style scoped>



</style>
