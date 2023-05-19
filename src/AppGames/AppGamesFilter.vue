<template >
  <div v-if="items">
  <div class='flex games-filter' :key="index" v-for="(item, index) in items">
    <div class="check-block flex">
      <input type="checkbox" :id='item["slug"]'
             :checked="item.slug in checked"
             :value="item.slug" @click="toggleFilterAction({list: filter, item: item.slug})"
             class="filters-check"><label class='filter-label' :for="item.slug">{{item.title}}
    </label>
    </div>
  </div>
  </div>
</template>

<script>
import {capitalize} from "eslint-plugin-vue/lib/utils/casing";
import {mapActions} from "vuex";

export default {
  name: "AppGamesFilter",
  data(){
    return {
      setter: 'set' + capitalize(this.filter),
      getter: 'get' + capitalize(this.filter),
    }
  },
  computed: {
    items(){
      if (this.filter === 'genres') return this.$store.getters.getGenres
      if (this.filter === 'developers') return this.$store.getters.getDevelopers
      if (this.filter === 'platforms') return this.$store.getters.getPlatforms
      if (this.filter === 'publishers') return this.$store.getters.getPublishers
      return []
    },
    params(){
      console.log(this.$store.getters.getQuery)
      return this.$store.getters.getQuery
    },
    checked(){
      let query = this.$store.getters.getQuery
      if (this.filter in query){
        return query[this.filter]
      }
      else{
        return []

      }
    }
  },
  methods:{
    ...mapActions(['fetchData', 'toggleFilterAction'])
  },
  created(){
    this.fetchData({filter: this.filter, setter: this.setter})

  },
  props: ['filter']
}
</script>

<style scoped>
.filter-label{
  z-index:2;
  text-align: start;
  font-size: 13px;
  color: #E7E7E7;
  cursor: pointer;
}

.filters-check{
  visibility: hidden;
  width: 0;
  height: 0;}

.filters-check:checked + label:after{
  background-color: rgba(255, 255, 255, 0.1);
}

.filters-check + label:after{
  z-index: 1;
  top: 0;
  left: 0;
  content: '';
  width: 100%;
  height: 100%;
  position: absolute;
  border: transparent 1px solid;
  transition: border-color 0.2s ease-in-out;
}

.filters-check:hover + label:after{
  border-color: white;
  transition: border-color 0.2s ease-in-out;
}

.check-block{

  width: 100%;
  padding: 8px 27px;
  position: relative;
  margin-bottom: 3px;
}

.games-filter{
  justify-content: flex-start;
}

</style>
