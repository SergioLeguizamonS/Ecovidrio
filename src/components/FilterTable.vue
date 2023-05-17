<template>
    <div class="layer_principal">
        <div class="filter">
            <FilterThemes 
                @update-view-table="viewTable = $event"
                @data-send="consultarData"
                :myThemes="myThemes"
            >
            </FilterThemes>
        </div>
        
        <div v-if="!viewTable||empty" class="table center">
            <div class="validate">
                <img src="../assets/idea.png" alt="idea">
                <h2>No hay datos que mostrar.</h2>
                <h3>Selecciona un rango de fechas y temas para validar.</h3>
            </div>
        </div>
        <div v-else class="table">
            <div v-if="myRows.length">
                <TableShow @update-view-table="updateVueTable" :myRows="myRows"/>
            </div>
            <div v-else class="center">
                <img class="loading" src="../assets/spinner.gif" alt="loading">
            </div>                
        </div>

        <!-- <div v-if="!viewTable||empty" class="table">
            <div class="validate">
                <img src="../assets/idea.png" alt="idea">
                <h2>No hay datos que mostrar.</h2>
                <h3>Selecciona un rango de fechas y temas para validar.</h3>
            </div>
        </div>
        <div v-else class="layer_table">
            <div v-if="myRows.length">
                <TableShow @update-view-table="updateVueTable" :myRows="myRows"/>
            </div>
            <div class="table-container" v-else>
                <div class="loader">
                    <img class="loading" src="../assets/spinner.gif" alt="loading">
                </div>
            </div>
        </div> -->
    </div>
</template>

<script>
import FilterThemes from './FilterThemes.vue';
import TableShow from './TableShow.vue';

export default {
  name: 'FilterTable',
  emits: ['search-send'],
  props: {
    myThemes: {
      type: Object,
      required: true
    },
    myRows: {
      type: Array
    },
    empty: {
        type: Boolean
    }
  },
  components: {
    FilterThemes,
    TableShow
  },
  data() {
    return {
        viewTable: false,
        startDate: '',
        endDate: '',
        selectedThemes: []
    }
  },
  methods: {
    updateVueTable($event){
        this.viewTable = $event
        this.$emit('clear-rows',true);
    },
    consultarData(datos) {
        this.$emit('search-send', {
            startDate: datos.startDate,
            endDate: datos.endDate,
            selectedThemes: datos.selectedThemes
        });
    }
  }
}
</script>

<style>
    h2 {
        color: #666666;
    }
    h3 {
        color: #666666;
    }
    .layer_principal {
        position: relative;
        left: 0;
        right: 0;
        margin: 0 auto;
        width: 98%;
        height: auto;
        /* background-color: red; */
        margin-top: 30px;
        display: flex;
        
    }
    .layer_table {
        position: relative;
        left: 0;
        right: 0;
        margin: 0 auto;
        width: 85%;
        height: auto;
        /* background-color: red; */
        margin-top: 30px;
        display: flex;
    }
    .filter {
        width: 30%;
        height: auto;
        background: #FFFFFF;
        border-right: 1px solid #CCCCCC;
    }
    .table {
        width: 70%;
        height: auto;
        background: #FFFFFF;
        padding: 10px;
    }
    .center {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .validate {
        text-align: center;
    }
    /* Estilo para dispositivos móviles */
    @media (max-width: 768px) {
        .filter {
            width: 100%;
        }
        .table {
            width: 96%;
            height: auto;
            position: absolute;
            top: 520px;
            margin-top: 45px;
        }
    }
</style>