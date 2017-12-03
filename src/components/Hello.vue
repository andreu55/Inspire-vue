<template>
  <div id="hello">
    <h1 class="text-center">
      {{ laFrase }}
    </h1>
    <ul>
      <li v-for="count in contador">{{ count }}</li>
    </ul>
    <!-- <button type="button" class="btn btn-light mb-3" v-on:click.prevent="thePost">Load</button> -->
    <button type="button" class="btn btn-light mb-3" v-on:click.prevent="mezcla">Mezcla</button>
    <div class="row">
      <div class="col">
        <ul v-for="input in inputs" class="list-group">
          <li v-for="inp in input" class="list-group-item">
            {{ inp.id }}: {{ inp.name }}
          </li>
        </ul>
      </div>
      <div class="col">
        <ul class="list-group">
          <li v-for="output in outputs" class="list-group-item">
            {{ output.frase }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      contador: {
        output: 0,
        input: { 1:0, 2:0, 3:0, 4:0, 5:0, 6:0 }
      },
      u: ["un", "una"],
      e: ["el", "la"],
      laFrase: 'Welcome to Your Vue.js App',
      inputs: [],
      outputs: []
    }
  },
  // http: {
  //   headers: {
  //     'Authorization': 'Bearer 12345'
  //   }
  // },
  mounted: function() {
    this.$http.post('http://localhost/inspireAPI/public/api/comunes', {

    }).then(function(data) {
      this.inputs = data.body.inputs
      this.outputs = data.body.outputs
    })
  },
  methods: {
    mezcla: function() {

      var frase = this.outputs[this.contador.output].frase
      var pos = frase.search("#")

      // // Mientras encontremos simbolos # en la frase
      while(pos != -1) {
      //   // Sacamos el tipo
      //   console.log(frase)
      //   console.log(pos+1)
      //   console.log(pos+2)
        var tipo = frase.substring(pos+1, pos+2);
        var articulo = frase.substring(pos+2, pos+3);
        console.log(tipo)
        console.log(articulo)
      //
      //   // si encuentra articulo en modo #1u o #1e saltamos una posicion más al construir la frase
      //   if (articulo == "u" || articulo == "e") { extra = 1; }
      //   else { extra = 0; }
      //
        frase = frase.substring(0, pos) + this.getInput(tipo) + frase.substring(pos+2); // +extra
        pos = frase.search("#");
      }

      this.laFrase = frase
      this.contador.output++;
    },
    getInput: function(tipo) {

      var input = this.inputs[tipo][this.contador.input[tipo]].name
      this.contador.input[tipo]++;

      return input
    }
  }
}

// function getInput(tipo, articulo) {
//   var input = inputs[tipo-1][i.input[tipo]];
//   i.input[tipo] = i.input[tipo] + 1;
//   while (input == undefined) {
//     i.input[tipo] = 0;
//     input = inputs[tipo-1][i.input[tipo]];
//   }
//   // si encuentra articulo en modo #1u o #1e saltamos una posicion más al construir la frase
//   if (articulo == "u" || articulo == "e") {
//     if (articulo == "u") { articulo = u[sexos[tipo-1][i.input[tipo]]] + " "; }
//     if (articulo == "e") { articulo = e[sexos[tipo-1][i.input[tipo]]] + " "; }
//   } else { articulo = ""; }
//   // TODO: Revisar porque aveces aparece como undefined
//   if (articulo == "undefined ") {
//     articulo = "";
//   }
//   input = articulo + input;
//   // input = input.toLowerCase();
//   return input;
// }
// function muestraFrase() {
//   // Cogemos la siguiente frase disponible
//   var frase = outputs[i.output];
//   var pos = frase.search("#");
//   var tipo;
//   var input;
//   // Mientras encontremos simbolos # en la frase
//   while(pos != -1) {
//     // Sacamos el tipo
//     tipo = frase.substring(pos+1, pos+2);
//     articulo = frase.substring(pos+2, pos+3);
//     // si encuentra articulo en modo #1u o #1e saltamos una posicion más al construir la frase
//     if (articulo == "u" || articulo == "e") { extra = 1; }
//     else { extra = 0; }
//     input = "<span class='input_" + tipo + "' data-tipo=" + tipo + " data-articulo=" + articulo + ">" + getInput(tipo, articulo) + "</span>";
//     // if (pos != 0) { input = input.toLowerCase(); }
//     // Generamos la frase de nuevo con el campo sustituido
//     frase = frase.substring(0, pos) + input + frase.substring(pos+2+extra);
//     pos = frase.search("#");
//   }
//   $('#test').text(frase);
//   $('#frase').html(frase);
// }
// // Para cambiar los inputs al clickar en ellos
// $("#frase").on("click", "span[class^='input_']", function() {
//   // tipo = parseInt(this.attributes["data-tipo"].value);
//   tipo = parseInt($(this).data("tipo"));
//   // articulo = this.attributes["data-articulo"].value;
//   articulo = $(this).data("articulo");
//   $(this).text(getInput(tipo, articulo));
// });

</script>

<style scoped>
#hello {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

a {
  color: #42b983;
}
</style>
