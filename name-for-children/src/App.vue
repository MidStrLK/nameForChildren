<template>
  <div id="app">
      <topMenu @gender="onChangeGender" @filter="onChangeFilter" @sort="onChangeSort"/>
      <centerTable ref="tableComponent"/>
  </div>
</template>

<script>
import topMenu from './components/menu.vue'
import centerTable from './components/table.vue'

export default {
  name: 'app',
  components: {
      topMenu,
      centerTable
  },
    data() {
        return {
            gender: '1',
            filter: {
                lastname: '',
                namebeg: '',
                nameend: '',
                patronym: '',
                rating: '0'
            }
        }
    },
    methods: {
        onChangeFilter(val){
            this.filter = val;
            this.$refs.tableComponent.setFilter(val);
        },

        onChangeGender(val){
            this.$refs.tableComponent.setGender(Boolean(parseInt(val)));
        },

        onChangeSort(val){
            this.$refs.tableComponent.setSort(val);
        }
    }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
}
</style>
