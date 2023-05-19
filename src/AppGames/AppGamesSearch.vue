<template>
  <form class="form cat-form">
    <app-games-search-icon></app-games-search-icon>
    <input class='form-input cat-form-input' placeholder="Ключевые слова" :value='getGamesSearch' @input="setSearchText" type="search"></form>
</template>

<script>
import appGamesSearchIcon from "@/components/UI/icons/appGamesSearchIcon";
import {mapActions, mapGetters} from "vuex";

export default {
  name: "AppGamesSearchInput",
  components: {appGamesSearchIcon},
  computed: {
    ...mapGetters(['getGamesSearch'])
  },
  methods: {
    ...mapActions(['setGamesSearchAction']),
    setSearchText(e){
      this.$store.dispatch('setGamesSearchAction', e.target.value)
    },
  }

}
</script>

<style scoped>
.cat-form{
  width: 100%;
  padding-left: 10px;
  padding-right: 12px;
  margin-bottom: 39px;
}

.cat-form-input{
  font-size: 14px;
  width: 139px;
}

</style>
