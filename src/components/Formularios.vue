<template>
  <div class="container text-center">

    <div v-show="activado === false && loading === false" class="btn-group-vertical" style="margin-top:60px;">
      <button type="button" @click="activado = $event.target.name" class="btn btn-primary btn-lg" name="Alumno_Regular">Constancia de alumno regular</button>
      <button type="button" @click="activado = $event.target.name" class="btn btn-primary btn-lg" name="Asistencia_Examen" style="margin-top:40px;">Constancia de asistencia a examen</button>
      <button type="button" @click="activado = $event.target.name" class="btn btn-primary btn-lg" name="Porcentaje_Materias" style="margin-top:40px;">Porcentaje de materias probadas</button>
      <button type="button" @click="activado = $event.target.name" class="btn btn-primary btn-lg" name="Titulo_Tramite" style="margin-top:40px;">Certificado de título en trámite</button>
    </div>


<!-- Comienza div del formulario para Alumno_Regular  -->
    <div v-if="activado === 'Alumno_Regular'" class="row">
      <hr>
			<h1>{{nombres[activado]}}</h1>
      <br>
       <p>
           {{leyenda}}
  		</p>
			<hr>
      <div class="col-12">
        <h4>Para poder descargar el formulario es necesaria la siguiente informacion: </h4>
        <div v-if='Object.keys(carreras).length > 1' style="margin-left:25%; margin-right:25%;">
          <select id="curso" class="form-control" v-model="carrera" style="margin-top:30px;">
            <option value="" checked>Seleccione su carrera</option>
            <option v-for="carrera in carreras":value="carrera['nombre_carrera']"> {{carrera['nombre_carrera']}} </option>
          </select>
        </div>

        <div class="" style="margin-left:25%; margin-right:25%;">
          <select class="form-control" v-model="curso" style="margin-top:30px;">
            <option value="" checked>Seleccione su curso</option>
            <option v-for="curso in cursos" :value="curso"> {{curso}} </option>
          </select>
        </div>

      </div>

		</div>
<!-- Finaliza div del formulario para Alumno_Regular  -->


<!-- Comienza div del formulario para Asistencia_Examen  -->
    <div v-if="activado === 'Asistencia_Examen'" class="">
      <hr>
      <h1>{{nombres[activado]}}</h1>
      <br>
      <p>
          {{leyenda}}
      </p>
      <hr>
      <div class="col-12">
        <h4>Para poder descargar el formulario es necesaria la siguiente informacion: </h4>
        <div v-if='Object.keys(carreras).length > 1' style="margin-left:25%; margin-right:25%;">
          <select id="curso" class="form-control" v-model="carrera" style="margin-top:30px;">
            <option value="" checked>Seleccione su carrera</option>
            <option v-for="carrera in carreras":value="carrera['nombre_carrera']"> {{carrera['nombre_carrera']}} </option>
          </select>
        </div>

        <div class="" style="margin-left:25%; margin-right:25%;">
          <select class="form-control" v-model="curso" style="margin-top:30px;">
            <option value="" checked>Seleccione su curso</option>
            <option v-for="curso in cursos" :value="curso"> {{curso}} </option>
          </select>
        </div>

        <div class="" style="margin-left:25%; margin-right:25%;">
          <select class="form-control" v-model="materia" style="margin-top:30px;">
            <option value='' checked>Seleccione su asignatura</option>
            <option v-for="materia in materias" :value="materia"> {{materia}} </option>
          </select>
        </div>

        <div class="contenedor">

            <div class="datepicker">
              <h4>Seleccione la fecha del parcial / final.</h4>
              <!-- Comienzo de datepicker -->
              <Datepicker :disabledDates="LimiteFecha" :language="es" inline v-model="date_select" required>

              </Datepicker>
              <!-- Fin del datepicker -->
            </div>
        </div>

      </div>

    </div>
<!-- Finaliza div del formulario para Asistencia_Examen  -->


<!-- Comienza div del formulario para Porcentaje_Materias  -->
		<div v-if="activado === 'Porcentaje_Materias'" class="">
			<hr>
			<h1>{{nombres[activado]}}</h1>
      <br>
      <p>
          {{leyenda}}
      </p>
			<hr>
      <div v-if='Object.keys(carreras).length > 1' class="col-12">
        <h4>Para poder descargar el formulario es necesaria la siguiente informacion: </h4>
        <div style="margin-left:25%; margin-right:25%;">
          <select id="curso" class="form-control" v-model="carrera" style="margin-top:30px;">
            <option value="" checked>Seleccione su carrera</option>
            <option v-for="carrera in carreras":value="carrera['nombre_carrera']"> {{carrera['nombre_carrera']}} </option>
          </select>
        </div>
      </div>

		</div>


<!-- Comienza div del formulario para Titulo_Tramite -->
    <div v-if="activado === 'Titulo_Tramite'" class="">
      <hr>
			<h1>{{nombres[activado]}}</h1>
			<br>
      <p>
          {{leyenda}}
      </p>
			<hr>
      <div v-if='Object.keys(carreras).length > 1' class="col-12">
        <h4>Para poder descargar el formulario es necesaria la siguiente informacion: </h4>
        <div style="margin-left:25%; margin-right:25%;">
          <select id="curso" class="form-control" v-model="carrera" style="margin-top:30px;">
            <option value="" checked>Seleccione su carrera</option>
            <option v-for="carrera in carreras":value="carrera['nombre_carrera']"> {{carrera['nombre_carrera']}} </option>
          </select>
        </div>
      </div>

		</div>


<!-- Comienza div de botones de los formularios  -->
		<div v-if="activado != false" style="margin-top:50px;">
      <button type="button" @click="function() {activado = false; curso = ''; materia = ''; date_select= new Date(); if (Object.keys(carreras).length > 1) {carrera = '';}}" class="btn btn-danger btn-lg" name="Menu_Formularios">Menu Formularios</button>
      <button type="button"  @click="generarPdf" class="btn btn-primary btn-lg" :disabled="boton" name="Enviar" style="margin-left:40px;">Enviar</button>
    </div>

    <div style="margin-top:10%;">
      <ring-loader class="cargando" :loading="loading"></ring-loader>
    </div>
    <br>

  </div>
</template>


<script>

	import configUrl from '../services/config';
  import axios from 'axios';
  import Datepicker from 'vuejs-datepicker';
  import {es} from 'vuejs-datepicker/dist/locale';
  import RingLoader from 'vue-spinner/src/RingLoader.vue';

	var url = configUrl.apiUrl;

	export default{
		name:"formularios",
		data(){
			return{
        es: es,
        date_select: new Date(),
				activado: false,
				nombres: {'Alumno_Regular':'Constancia de alumno regular',
                  'Titulo_Tramite':'Certificado de Título en trámite',
                  'Asistencia_Examen':'Constancia de asistencia a examen',
                  'Porcentaje_Materias':'Constancia de Porcentaje de Materias'},
        leyenda: `El documento debe ser presentado en preceptoria para la verificación de los datos.
                   En caso de tener información errónea o desactualizada, por favor comunicarse con preceptoria. `,
        cursos: [
          '1º A',
          '1º B',
          '1º C',
          '2º A',
          '2º B',
          '2º C',
          '3º A',
          '3º B',
          '3º C',
        ],
        curso: '',
        carreras: [],
        carrera: '',
        objMaterias: {},
        materia: '',
        loading: true,
			}
		},
    computed: {
      boton: function() {
        if (this.activado === 'Asistencia_Examen') {
          if (Object.keys(this.carreras).length > 1) {
            if (this.curso === '' || this.carrera === '' || this.materia === '') {
              return true
            }
            else {
              return false
            }
          }
          else {
            if (this.curso === '' || this.materia === '') {
              return true
            }
            else {
              return false
            }
          }
        }
        else if (this.activado === 'Alumno_Regular') {
          if (Object.keys(this.carreras).length > 1) {
            if (this.curso === '' || this.carrera === '') {
              return true
            }
            else {
              return false
            }
          }
          else {
            if (this.curso === '') {
              return true
            }
            else {
              return false
            }
          }
        }
        else {
          if (Object.keys(this.carreras).length > 1) {
            if (this.carrera === '') {
              return true
            }
            else {
              return false
            }
          }
        }
      },
      materias: function() {
        return this.objMaterias[this.carrera];
      },
      LimiteFecha: function () {
          // dia actual, mes actual y año actual
          let limite_dia = new Date().getDate();
          let limite_mes = new Date().getMonth();
          let limite_año = new Date().getFullYear();
          return {
            // la fecha hasta la que se inhabilitan dias.
            to: new Date(limite_año,limite_mes,limite_dia),
            // la fecha desde la que se deshabilitan dias, hoy + 2 mes.
            // from: new Date(limite_año,limite_mes + 2,limite_dia),
            // Desactivados los Domingos.
            days: [0],
          }
      },
    },
		methods: {

      generarPdf() {
        let dia_select = this.date_select.getDate();
        let mes_select = this.date_select.getMonth() + 1;
        let anio_select = this.date_select.getFullYear();
        let direccion = this.activado + '.php';
        let get = '?token=' + localStorage.getItem('token');
        get += '&legajo=' + JSON.parse(localStorage.getItem('datos')).legajo;
        get += '&curso=' + this.curso;
        get += '&carrera=' + this.carrera;
        get += '&materia=' + this.materia;
        get += '&dia_select=' + dia_select;
        get += '&mes_select=' + mes_select;
        get += '&anio_select=' + anio_select;
        let urlpdf = url + direccion + get;
        window.open(urlpdf , '_blank');
        this.activado = false;
        this.curso = '';
        if (Object.keys(this.carreras).length > 1) {
          this.carrera = '';
        }
        this.materia = '';
        this.date_select= new Date();
      },

      getCarreras() {
				var token = localStorage.getItem('token');
				var legajo = JSON.parse(localStorage.getItem('datos')).legajo;
				var urlSituacion = url +'situacionAcademica.php'
				axios.get(urlSituacion,{
					params: {
						legajo: legajo,
						token: token
					}
				}).then(res => {
					console.log(res.data);
					this.carreras = res.data;

          // Agregado por InTec
          for (let key in this.carreras) {
            let carrera = this.carreras[key];
            let objMateria = carrera['materias'];
            let lista_de_materias = [];
            for (let key in objMateria) {
              let materia = objMateria[key];
              lista_de_materias.push(materia['nombre_materia']);

              // for (let prop in materia) {
              //   console.log(prop + " = " + materia[prop]);
              // }
            }
            this.$set(this.objMaterias, carrera['nombre_carrera'], lista_de_materias);
          }
          if (Object.keys(this.carreras).length > 1) {
            this.carrera = '';
          }
          else if (Object.keys(this.carreras).length === 1) {
            for (let key in this.carreras) {
              let carrera = this.carreras[key];
              this.carrera = carrera['nombre_carrera'];
            }
          }
					this.loading = false;
				}).catch(err => {
				})
			},

    },

    mounted() {
			this.getCarreras();
		},
    components:{
      Datepicker,
      RingLoader,
    },
  }
</script>

<style>
	.cargando {
		display: inline-block;
	}
  .contenedor{

  }
  .datepicker{
    padding-top: 30px;
    margin-left: auto;
    margin-right: auto;
    width: 300px;
    min-height: 200px;
  }

</style>
