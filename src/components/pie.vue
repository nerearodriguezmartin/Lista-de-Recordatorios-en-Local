<template lang="html">

  <section class="pie">
    <p>Desarrollado por Nerea Rodríguez Martín</p>
    <p>Código disponible en <a href="https://github.com/nerearodriguezmartin/Lista-de-Recordatorios-en-Local"><b-icon icon="github" variant="success"></b-icon></a></p>
    
  </section>

</template>

<script lang="js">

  export default  {
    name: 'pie',
    props: [],
    mounted () {

    },
    data () {
      return {
        datos: []
      }
    },
    methods: {
      
    },
    computed: {

    }
}


</script>

<style scoped >
  .pie {
    color: gray;
    background-color: black;
  }
</style>
