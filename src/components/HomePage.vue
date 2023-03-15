<template>
  <div v-if="viewApp">
    <HeaderSection/>
    <FilterTable
      @search-send="searchData"
      :myThemes="myThemes"
      :myRows="myRows"
    />
  </div>
  <div v-else class="loader">
    <img class="loading" src="../assets/spinner.gif" alt="loading">
  </div>
</template>

<script>
import axios from 'axios';
import HeaderSection from './HeaderSection.vue';
import FilterTable from './FilterTable.vue';

export default {
  name: 'HomePage',
  components: {
    HeaderSection,
    FilterTable
  },
  data() {
    return {
      viewApp: false,
      goTable: false,
      id: 0,
      myThemes: [],
      myRows: [],
      startDate: '',
      endDate: '',
      selectedThemes: []
    }
  },
  created() {
      this.sendData(this.id);
  },
  methods: {
    sendData(id) {
      axios.post('http://localhost/funcion.php', {
        id: id
      }).then(response => {
        const data = response.data;
        const myArray = data.map(item => ({ id: item.id, tema: item.tema }));
        // console.log(myArray);
        this.viewApp = true;
        this.myThemes = myArray;
      }).catch(error => {
        console.log(error);
      });
    }, 
    searchData(datos) {
      // console.log("llego a home: " + datos.startDate);
      // console.log("llego a home:: " + datos.endDate);
      // console.log("llego a home:: " + datos.selectedThemes);
      axios.post('http://localhost/funcion.php', {
        id: datos.selectedThemes,
        startDate: datos.endDate,
        endDate: datos.endDate
      }).then(response => {
        const data = response.data;
        this.goTable = true;
        this.myRows = data;
        console.log(data);
        // const myArray = data.map(item => ({ id: item.id, tema: item.tema }));
      }).catch(error => {
        console.log(error);
      });
    }
  }
}
</script>

<style>
  .loader {
    text-align: center;
  }
</style>