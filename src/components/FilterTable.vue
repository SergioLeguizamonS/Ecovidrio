<template>
    <div class="layer_principal" v-if="!viewTable">
        <div class="filter">
            <FilterThemes 
                @update-view-table="viewTable = $event"
                @data-send="consultarData"
                :myThemes="myThemes"
            >
            </FilterThemes>
        </div>
        
        <div v-if="!viewTable" class="table">
            <div class="validate">
                <img src="../assets/idea.png" alt="idea">
                <h2>No hay datos que mostrar.</h2>
                <h3>Selecciona un rango de fechas y temas para validar.</h3>
            </div>
        </div>

        <!-- <div v-if="myRows.length === 0" class="table">
            no hay datos para mostrar

        </div> -->
        <!-- <div v-else class="table">
            <div class="validate">
                <img src="../assets/idea.png" alt="idea">
                <h2>No hay datos que mostrar.</h2>
                <h3>Selecciona un rango de fechas y temas para validar.</h3>
            </div>
        </div> -->
    </div>
    <div v-else class="layer_table">
        <TableShow
            @update-view-table="viewTable = $event"
            :myRows="myRows"
        />
    </div>
    <!-- <div v-else>
        no hay data en la consulta
    </div> -->
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
    consultarData(datos) {
        console.log("llego: " + datos.startDate +' '+ '00:00:00');
        console.log("llego: " + datos.endDate +' '+ '23:59:59');
        console.log("llego: " + datos.selectedThemes);
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
        width: 85%;
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
        width: 50%;
        height: auto;
        background: #FFFFFF;
        border-right: 1px solid #CCCCCC;
    }
    .table {
        width: 50%;
        height: auto;
        background: #FFFFFF;
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
            width: 100%;
            height: 300px;
            position: absolute;
            top: 520px;
        }
    }
</style>