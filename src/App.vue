<template>
  <div id="app">
    <cabecera></cabecera>
    <div class="inputs">
      <b-input-group size="lg" >
          <b-form-input class="input" type="text" v-model="texto" placeholder="Introduce nuevo recordatorio" v-on:keydown.enter = "crearNota" ></b-form-input>
          <b-form-input class="input" type="text" v-model="empiezaPor" placeholder="Buscar"></b-form-input>
      </b-input-group> 
    </div>

      <hr>
          <div class="numNotas">
              <p><b-icon icon="bar-chart-fill"></b-icon>  {{totalPendientes}} Tareas pendientes de un total de {{totalTareas}} |  <span v-on:click="borrarTareasCompletadas"><b-icon icon="x" scale="1.5"></b-icon> Borrar tareas completadas</span></p>
              
          </div>
      <hr>

    <transition-group name="mielemento" tag="h1">
    <nota
    v-for="nota in listaRecordatoriosFiltrada"
    v-bind:key="nota.titulo"
    v-bind:nota="nota"
    v-on:cambioEnEstado="actualizarLocalStorage"
    v-on:cambioEnPrioridad="actualizarLocalStorage"
    v-on:borraTarea="borraTarea(nota)"
    ></nota>
    

    </transition-group>
    <pie></pie>

    <h1 v-for="(nota2, index) in listaRecordatoriosFiltrada" v-bind:key="index">{{nota2.title}}</h1>
   
  
  
  </div>

 
</template>

<script>
import cabecera from './components/cabecera'
import pie from './components/pie'
import nota from './components/nota'


export default {
  name: 'App',
  mounted(){
    if (localStorage.listaTareas)
        this.listaRecordatorios = JSON.parse(localStorage.listaTareas);
  },

  data(){
    return{
      listaRecordatorios: [],
      texto: "",
      empiezaPor: ""

    }
  },

  computed:{
        totalTareas: function(){
           return this.listaRecordatorios.length;
        },

        totalPendientes: function(){
            let total = 0;
            for (let i=0; i<this.listaRecordatorios.length; i++){
                if(!this.listaRecordatorios[i].completado){
                    total++;
                }
            }
            
            return total;
        },

        listaRecordatoriosFiltrada: function()
        {
            let listaFiltrada;
            if(this.empiezaPor == "") 
                listaFiltrada = this.listaRecordatorios;
            else
                listaFiltrada = this.listaRecordatorios.filter((recordatorio)=>
            {
                if (recordatorio.titulo.indexOf(this.empiezaPor)>=0){
                    return true;
                }
                else{
                    return false;
                }


            });

            // ordenar el array
            listaFiltrada.sort((nota1, nota2)=>
            {
                if(nota1.prioridad > nota2.prioridad){
                    return -1;
                }
                if(nota1.prioridad < nota2.prioridad){
                    return 1;
                }

                // si son iguales 
                return 0;

            });


            return listaFiltrada;
        }


  },

  methods:{
        crearNota: function()
        {
            this.listaRecordatorios.push({
                titulo: this.texto,
                prioridad: 2,
                fechaCreacion: new Date(),
                completado: false,
            });
            this.texto = "";
            this.actualizarLocalStorage();
            
        },

        actualizarLocalStorage: function()
        {
            localStorage.listaTareas = JSON.stringify(this.listaRecordatorios);
        },

        borrarTareasCompletadas: function () 
        { 
            this.listaRecordatorios = this.listaRecordatorios.filter((nota)=>
            {
                return !nota.completado;
            });
            this.actualizarLocalStorage();
        },

         borraTarea: function(nota)
        {
          console.log(this.listaRecordatorios)
          this.listaRecordatorios = this.listaRecordatorios.filter((recordatorio)=>{
            if(recordatorio.titulo != nota.titulo){
              return recordatorio;
            }
             
            }
          );
              this.actualizarLocalStorage();
              
              
        },
  },
    
  components: {
    cabecera,
    nota,
    pie
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background-color: black;
  height: 100vh;
  width: 100vw;
}



hr{
  background-color: gray;
}

.numNotas{
  text-align: left;
}

.numNotas>p{
  color: white;
}

.numNotas>p>span{
  color: orange;
}


.mielemento-enter-active,
.mielemento-leave-active {
  transition: opacity 3s;
}

.mielemento-enter, 
.mielemento-leave-top{
  opacity: 0;
}



</style>
