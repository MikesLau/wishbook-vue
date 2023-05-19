<template>
  <vue-awesome-paginate   class="flex" :total-items="totalGames"
                          :items-per-page="24"
                          :max-pages-shown="3"
                          v-model="currentPage"></vue-awesome-paginate>
</template>

<script>
import {mapActions, mapGetters} from "vuex";
import { VueAwesomePaginate } from 'vue-awesome-paginate'

export default {
  name: "AppGamesPagination",
  components: { VueAwesomePaginate },
  methods:{...mapActions(['setGamesPage'])},
  data(){
    return {
      currentPage: 1
    }
  },
  computed: {
    ...mapGetters(['totalGames', 'getGamesPage'])
  },
  created(){
    this.currentPage = this.getGamesPage
  }
  ,
}
</script>

<style scoped>

.pagination{
  justify-content: center;
  margin-bottom: 154px;

}

.pagination-number{
  margin-right: 13px;
  text-decoration: none;
  cursor: pointer;
  color: #7D7D7D;
}

.pagination-number--active{
  color: white;
  border-bottom: white 1px solid;
}
</style>
