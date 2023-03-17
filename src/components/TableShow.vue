<template>
    <div class="table-container">
        <img class="excel" src="../assets/excel.png" alt="download" @click="descargarExcel">
        <img class="close" src="../assets/cerrar.png" alt="close" @click="closed">
        <table>
        <thead>
          <tr>
            <th>Fecha</th>
            <th>Medio</th>
            <th>Titular</th>
            <th>URL</th>
            <th>Tema</th>
            <th>Soporte</th>
            <th>Categoría</th>
            <th>VP</th>
            <th>Titular</th>
            <th>Relevancia</th>
            <th>Fotografía</th>
            <th>Nuevo VC</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in myRows" :key="item">
            <td>{{ item.fulldate }}</td>
            <td>{{ item.paper }}</td>
            <td>{{ item.title }}</td>
            <td><a href="item.article_url">{{ item.article_url}}</a></td>
            <td>{{ item.tema}}</td>
            <td>{{ item.p_or_d }}</td>
            <td>{{ item.categoria }}</td>
            <td>{{ item.vp }}</td>
            <td>{{ item.is_in_title }}</td>
            <td>{{ item.custom_relevancia }}</td>
            <td>{{ item.is_in_picture }}</td>
            <td>{{ item.vc }}</td>
          </tr>
        </tbody>
      </table>
    </div>
</template>

<script>
import * as XLSX from 'xlsx';

export default {
    name: 'TableShow',
    emits: ['update-view-table'],
    props: {
        type: Boolean,
        myRows: {
            type: Array
        },
    },
    data() {
        return {
            tableData: [
                {
                id: 1,
                fecha: "2022-02-01",
                medio: "El País",
                titular: "El titular de la noticia",
                url: "https://elpais.com/",
                soporte: "Prensa",
                categoria: "Política",
                valorPublicitario: 1500,
                mention: "Sí",
                relevancia: "Alta",
                fotografia: "https://via.placeholder.com/150",
                nuevoVC: "No",
                },
            ],
        };
    },
    methods: {
        descargarExcel() {
        const worksheet = XLSX.utils.json_to_sheet(this.myRows);
        const workbook = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(workbook, worksheet, 'Datos');
        XLSX.writeFile(workbook, 'datos.xlsx');
        },
        closed() {
            this.$emit('update-view-table', false);
        }   
    }
}
</script>

<style>
    .table-container {
        width: 100%;
        max-height: 520px;
        overflow-x: auto;
        margin: 1rem 0;
        background: #FFFFFF;
    }
    
    table {
        border-collapse: collapse;
        width: 100%;
        margin-top: 20px;
    }
    tr {
        background: #FFFFFF;
    }

    th,
    td {
        text-align: left;
        padding: 0.5rem;
        border-bottom: 1px solid #ddd;
        font-size: 13px;
    }
    
    th {
        background-color: #11338c;
        color: #FFFFFF;
        font-weight: bold;
    }
    
    tbody tr:hover {
        background-color: #f5f5f5;
    }
    
    img {
        max-width: 100px;
        height: auto;
    }
    .excel {
        cursor: pointer;
        margin-left: 15px;
        margin-top: 20px;
        width: 35px;
    }
    .close {
        position: absolute;
        right: 15px;
        top: 40px;
        width: 35px;
        cursor: pointer;
    }
</style>