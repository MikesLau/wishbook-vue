<template>
  <div class="catalogue-filters">
    <div class="filters-top flex">
      <p class="">Фильтры</p><router-link class="filters-reset" @click="resetQueryAction" to="/games/">сбросить</router-link></div>
    <appGamesSearch />
    <appAccordion title="Жанры">
      <app-games-filter filter="genres"></app-games-filter>
    </appAccordion>
    <appAccordion title="Платформы">
      <app-games-filter filter="platforms"></app-games-filter>
    </appAccordion>
    <appAccordion title="Разработчики"><app-games-filter filter="developers"></app-games-filter>
    </appAccordion>
    <appAccordion title="Цена">
      <AppGamesPriceFilter />
    </appAccordion>
    <appAccordion v-if="false" title="Предложения">
<!--      TODO предложения-->
      <MyCheckBox @checked='() => {setDiscount(!discount)}' id="discount">Со скидкой</MyCheckBox>
      <MyCheckBox @checked="() => {setPreOrder(!preorder)}" id="preorder">Предзаказ</MyCheckBox>
    </appAccordion>
  </div>
</template>

<script>
import appGamesFilter from "@//AppGamesFilter";
import appAccordion from "@/components/UI/AppAccordion";
import MyCheckBox from "@/components/UI/MyCheckBox";
import AppGamesSearch from "@//AppGamesSearch";
import AppGamesPriceFilter from "@//AppGamesPriceFilter";
import {mapActions} from "vuex";

export default {
  name: "appGamesFilters",
  components: {AppGamesPriceFilter, appGamesFilter, appAccordion,  MyCheckBox, AppGamesSearch},
  data(){
    return {
      search: '',
      minPrice: 0,
      maxPrice: 0,
      minPriceQuery: 0,
      maxPriceQuery: 0,
      discount: false,
      preorder: false,
      games: null
    }
  },
  methods: {
    ...mapActions(['resetQueryAction'])
  }
}
</script>

<style scoped>
.catalogue-filters{
  padding-top: 40px;
  width: 185px;
}

.filters-top{
  justify-content: space-between;
  margin-bottom: 9px;
}

.filters-reset{
  text-decoration: none;
  color: #6C6767;
  transition: color 0.1s ease-in-out;
}

.filters-reset:hover{
  color: darkred;
  transition: color 0.1s ease-in-out;
}

</style>
