<template>
  <div v-if="viewApp">
    <HeaderSection/>

    <FilterTable
      @search-send="searchData"
      @clear-rows="clearRows"
      :myThemes="myThemes"
      :myRows="myRows"
      :empty="empty"
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
      selectedThemes: [],
      empty:false
    }
  },
  created() {
      this.sendData(this.id);
  },
  methods: {
    clearRows($event){
       if($event) this.myRows = []
    },
    sendData(subclient_id) {
      axios.post('https://epservices.eprensa.com/Parques_reunidos/getTemas.php', {
        subclient_id: subclient_id
      }).then(response => {
        const data = response.data;
        const temas = data.map(item => ({ id: item.id, tema: item.tema }));

        this.viewApp = true;
        this.myThemes = temas;
      })
      .catch(error => {
        console.log(error);
      });
    }, 
    searchData(datos) {
      this.myRows = [];
      this.empty = false;

      axios.post('https://epservices.eprensa.com/Parques_reunidos/getCompanies.php', {
        selectedTemas: datos.selectedThemes,
        startDate: datos.startDate,
        endDate: datos.endDate
      }).then(response => {
        const data = response.data;
        this.goTable = true;
        this.myRows = data;
        this.empty = !this.myRows.length;

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