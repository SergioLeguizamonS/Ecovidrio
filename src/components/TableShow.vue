<template>
    <div class="table-container">
        <div>
            <img class="excel" src="../assets/excel.png" alt="download" @click="descargarExcel">
        </div>
        <div>
            <b class="results_counter" v-if="myRows&&myRows.length">
                Resultados: {{myRows.length}}
            </b>
        </div>
        <img class="close" src="../assets/cerrar.png" alt="close" @click="closed">
        <!-- <p>Total de noticias: ({{ this.myRows.length }})</p> -->
        <table>
            <thead>
              <tr>
                <th>Fecha</th>
                <th>Medio</th>
                <th>Titular</th>
                <th>URL</th>
                <th>Tono</th>
                <th>Zona geográfica</th>
                <th>Campañas</th>
                <th>Origen</th>
                <th>Mensaje</th>
                <th>Equipo</th>
                <th>Titular</th>
                <th>Espacio</th>
                <th>Fotografía</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in myRows" :key="item">
                <td>{{ item.fulldate }}</td>
                <td>{{ item.paper }}</td>
                <td>{{ item.title }}</td>
                <td><a :href="item.article_url" target="_blank">{{ item.article_url}}</a></td>
                <td>{{ item.tono }}</td>
                <td>{{ }}</td>
                <td>{{ item.campana }}</td>
                <td>{{ item.origen }}</td>
                <td>{{ item.mensaje }}</td>
                <td>{{ }}</td>
                <td>{{ item.is_in_title }}</td>
                <td>{{ }}</td>
                <td>{{ item.is_in_picture }}</td>
              </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import ExcelJS from 'exceljs';

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
            thColumns: [
                'Fecha',
                'Medio',
                'Titular',
                'URL',
                'Tono',
                'Zona geográfica',
                'Campañas',
                'Origen',
                'Mensaje',
                'Equipo',
                'Titular',
                'Espacio',
                'Fotografía'
            ]
        };
    },
    methods: {
        async descargarExcel() {
    const workbook = new ExcelJS.Workbook();
    const sheet = workbook.addWorksheet('Datos');

    // Escribir los encabezados de columna
    sheet.addRow(this.thColumns);

    // Escribir los datos de las filas
    this.myRows.forEach(item => {
        const rowData = [
            item.fulldate,
            item.paper,
            item.title,
            { text: item.article_url, hyperlink: item.article_url, tooltip: 'Click to open link' },
            item.tono,
            '', // Zona geográfica - No está claro de dónde obtener este dato
            item.campana,
            item.origen,
            item.mensaje,
            '', // Equipo - No está claro de dónde obtener este dato
            item.is_in_title,
            '', // Espacio - No está claro de dónde obtener este dato
            item.is_in_picture
        ];
        sheet.addRow(rowData);
    });

    // Guardar el archivo Excel
    const buffer = await workbook.xlsx.writeBuffer();
    const blob = new Blob([buffer], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
    const url = URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.href = url;
    link.setAttribute('download', 'datos.xlsx');
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    URL.revokeObjectURL(url);
}

    ,closed() {
            this.$emit('update-view-table', false);
        }   
    },
    computed: {
        customRows(){
            return 1;
        }
    },
}
</script>

<style>
    .table-container {
        width: 100%;
        max-height: 520px;
        overflow-x: auto;
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
        float:left;
    }
    .results_counter{
        float: left;
        margin-top: 30px;
        margin-left: 20px;
        font-size: 12px;
        margin-bottom: 25px;
    }
    .close {
        position: absolute;
        right: 25px;
        top: 30px;
        width: 30px;
        cursor: pointer;
    }
</style>