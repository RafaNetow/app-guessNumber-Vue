<template>
  <div class="container d-flex justify-content-center">

    <div class="spinner-border text-primary spineer" role="status" v-if="showSpinner">
      <span class="sr-only">Loading...</span>
    </div>

    <form class="border p-5 w-50 rounded" v-if="window=='wantToPlay'" >
      <div class="form-group">
        <h2 class="mb-5">¿Deseas Jugar?</h2>
      </div>
      <div class="form-group form-check ">
        <button type="submit" class="btn btn-primary w-50" v-on:click="viewWindow('insertNameUser')">Iniciar Juego</button>
      </div>
    </form>

    <form class="border p-5 w-50 rounded" v-if="window == 'insertNameUser'" >
      <div class="form-group">
        <h5 class="mb-5">¿Cúal es tu nombre?</h5>
        <input type="text" v-model="nameGamer" class="form-control" placeholder="Ingresa tu nombre" required>
      </div>
      <div class="form-group form-check ">

        <button @click="viewWindow('guessNumber')" class="btn btn-primary w-25 mr-5">Aceptar</button>
        <button @click="viewWindow('wantToPlay')" type="submit" class="btn btn-primary w-25">Cancelar</button>
      </div>
    </form>

    <form class="border p-5 w-50 rounded" v-if="window == 'guessNumber'" >
      <div class="form-group">
        <h5 class="mb-5">¿Qué número estoy pensando...?</h5>
        <input type="number"  v-model="guessNumberForUser" class="form-control" placeholder="Ingresa número" required>
      </div>
      <div class="form-group form-check">
        <button type="submit" @click="guessNumber" v-if="!endGame" class="btn btn-primary">Adivinar</button>
        <button type="submit" @click="restartGame" v-if="endGame" class="btn btn-primary">Reiniciar Juego</button>

      </div>
    </form>

  </div>

</template>

<script>
export default {
  name: 'guessNumber',
  data () {
    return {
      nameGamer: '',
      endGame: false,
      window: 'wantToPlay',
      guessNumberForUser: 0,
      numberRandom: 0,
      attempsNumber: 0,
      showSpinner: false
    }
  },
  methods: {
    getNumberRandom: function () {
      this.numberRandom = Math.floor(Math.random() * 100)
      setTimeout(() => this.showSpinner = false, 500)
    },
    viewWindow: function (window) {
      if (window === 'insertNameUser') {
        this.showSpinner = true
        this.getNumberRandom()
      }
      this.window = window
    },
    guessNumber: function () {
      if (this.numberRandom < Number(this.guessNumberForUser)) {
        this.attempsNumber++
        this.$swal.fire({
          type: 'error',
          title: 'Oops...',
          text: 'Estoy pensando un número más bajo!!!`'
        })
        this.attempsNumber++
      } else if (this.numberRandom > Number(this.guessNumberForUser)) {
        this.$swal.fire({
          type: 'error',
          title: 'Oops...',
          text: 'Estoy pensando un número más alto!!!`'
        })
        this.attempsNumber++
      } else if (this.numberRandom === Number(this.guessNumberForUser)) {
        this.$swal(`Correcto!!! ${this.nameGamer}`, `Lo has logrado en ${this.attempsNumber} intentos`, `success`)
        this.endGame = true
      }
    },
    restartGame: function () {
      this.getNumberRandom()
      this.nameGamer = ''
      this.window = 'wantToPlay'
      this.attempsNumber = 0
      this.guessNumberForUser = 0
      this.endGame = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.spineer {
  position: relative;
  left: 27%;
  margin-top: 10%;
}
</style>
