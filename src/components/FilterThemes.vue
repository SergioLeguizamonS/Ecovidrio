<template>
    <div>
        <h1>Bienvenido</h1>
    </div>
    <form class="form" v-on:submit.prevent="submitForm">
      <div class="form-group">
        <label for="start-date">Fecha de inicio:</label>
        <input type="date" id="start-date" v-model="startDate" class="form-control" required>
      </div>
      
      <div class="form-group">
        <label for="end-date">Fecha final:</label>
        <input type="date" id="end-date" v-model="endDate" class="form-control"  required>
      </div>
      
      <div class="form-group checkboxes">
        <label class="label">
            <input type="checkbox" v-model="selectAll">
            Seleccionar todo
        </label>
        <div v-for="(attraction, index) in myThemes" :key="index">
            <label class="label">
            <input type="checkbox" class="left" :value="attraction.id" v-model="selectedThemes">
            {{ attraction.tema}}
            </label>
        </div>
    </div>
      
      <button type="submit" class="btn btn-primary">Filtrar</button>
    </form>
  </template>

    <script>
    export default {
        name : 'FilterThemes',
        emits: ['update-view-table', 'data-send'],
        props: {
            type: Boolean,
            myThemes: {
              type: Object,
              required: true
            }
        },
        data() {
            return {
                startDate: '',
                endDate: '',
                selectAll: false,
                viewTable: false,
                selectedThemes: [],
                themes: [
                    "Parque de Atracciones de Madrid",
                    "PARQUE WARNER",
                    "AQUOPOLIS",
                    "Selwo Marina",
                    "Selwo Aventura",
                    "TELEFÉRICO BENALMÁDENA"
                ]
            }
        },
        computed: {
          fechaInicialObj() {
            return new Date(this.startDate);
          },
          fechaFinalObj() {
            return new Date(this.endDate);
          }
        },
        watch: {
            selectAll: function(value) {
                if (value) {
                    this.selectedThemes = [...this.themes];
                } else {
                    this.selectedThemes = [];
                }
            }
        },
        methods: {
            submitForm() {
                if (this.validarDatos()) {
                    // Hacer algo con los datos enviados
                    
                    this.$emit('update-view-table', this.viewTable);
                    this.$emit('data-send', {
                      startDate: this.startDate,
                      endDate: this.endDate,
                      selectedThemes: this.selectedThemes
                    });
                    // console.log('Datos enviados:', this.startDate, this.endDate, this.selectedThemes);
                }
                },
                validarDatos() {
                if (this.startDate && this.endDate && this.selectedThemes.length > 0) {
                    this.viewTable = true;
                }
                if (this.fechaInicialObj > this.fechaFinalObj) {
                  const fechaInicio = document.getElementById('start-date');
                  const fechaFinal = document.getElementById('end-date');
                  fechaInicio.setAttribute('class', 'error');
                  fechaFinal.setAttribute('class', 'error');
                  console.log('La fecha inicial no puede ser mayor a la fecha final');
                  return false;
                } else {
                  const fechaInicio = document.getElementById('start-date');
                  const fechaFinal = document.getElementById('end-date');
                  fechaInicio.setAttribute('class', 'form-control');
                  fechaFinal.setAttribute('class', 'form-control');
                }

                if(this.selectedThemes.length === 0) {
                    const label = document.querySelectorAll('.label');
                    label.forEach((element) => {
                      element.style.color = 'red';
                    });
                    // label.style.color = 'red';
                    console.log("faltan datos");
                    this.viewTable = false;
                    this.$emit('updateViewTable', this.viewTable);
                    return false;
                }
                return true;
            },
        },
    }
    </script>
  
  <style>
  h1 {
    color: #666666;
    text-align: center;
  }
  .form {
    display: flex;
    flex-direction: column;
    width: 80%;
    margin: 0 auto;
    /* margin-top: 50px; */
  }
  
  .form-group {
    margin-bottom: 20px;
  }
  
  .form-group label {
    display: block;
    font-weight: bold;
    margin-bottom: 5px;
    color: #666666;
  }
  
    .form-control {
        width: 100%;
        /* padding: 10px; */
        font-size: 16px;
        border-radius: 5px;
        border: 1px solid #cfcfcf;
    }
  
    .checkboxes {
        display: flex;
        flex-direction: column;
        font-size: 13px;
    }

    .checkboxes label {
        /* margin-bottom: 10px; */
        font-weight: normal;
    }
  
  .btn {
    padding: 10px 20px;
    font-size: 16px;
    border-radius: 5px;
    border: none;
    background-color: #1645a1;
    color: #fff;
    cursor: pointer;
    margin-bottom: 20px;
  }
  
  .btn:hover {
    background-color: #1645a1;
  }
  .left {
    margin-left: 24px;
  }
  .error {
    width: 100%;
    /* padding: 10px; */
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid red;
  }
  </style>
  

  