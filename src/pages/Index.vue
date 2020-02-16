<template>
  <q-page class="flex flex-center column row">
    <span id="attempts">Intentos disponibles: {{ attempts }}</span>
    <span class="q-mb-xl" id="clue" size="lg"> Pista: {{ clue }} </span>
    <div
      class="full-width row inline wrap justify-evenly items-center content-center q-mt-xl"
    >
      <span class="letter" v-for="(space, index) in spaces" :key="index">
        {{ existingLetters.includes(space) ? space : "" }}
      </span>
    </div>

    <div class="justify-center col-xs-9 col-md-10 col-lg-10 col-xl-12">
      <q-input
        outlined
        v-model="letter"
        type="text"
        label="Ingresa una letra"
        class="q-mt-xl q-mx-auto"
        @input="tryLetter()"
        focused
      />
    </div>

    <br /><br />
    <div id="entered-letters" v-if="failedLetters.length">
      Letras que has digitado:
      <br />
      <span v-for="(letter, index) in failedLetters" :key="index">
        {{ letter }}
      </span>
    </div>

    <div
      class="full-width row inline wrap justify-evenly items-center content-center"
    >
      <q-btn
        color="primary"
        icon="replay"
        rounded
        :label="$q.screen.gt.xs ? 'Reiniciar' : void 0"
      />
      <q-btn
        color="warning"
        icon="pause"
        rounded
        :label="$q.screen.gt.xs ? 'Pausar' : void 0"
      />
      <q-btn
        color="deep-orange"
        icon="clear"
        rounded
        :label="$q.screen.gt.xs ? 'Abortar' : void 0"
      />
    </div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      attempts: 3,
      clue: "Se puede encontrar fácilmente en los bosques",
      confirm: false,
      spaces: [],
      letter: "",
      lastLetter: "",
      word: "ARBOL",
      existingLetters: [],
      failedLetters: [],
      id: new Date()
    };
  },
  methods: {
    graphWordSpaces() {
      this.spaces = this.word.split("");
    },
    tryLetter(evt) {
      const lett = this.letter.toUpperCase();
      if (
        !this.failedLetters.includes(lett) &&
        !this.existingLetters.includes(lett)
      ) {
        if (this.spaces.includes(lett)) {
          this.existingLetters.push(lett);
          this.graphWordSpaces();
          this.showSuccess(lett);
        } else {
          this.failedLetters.push(lett);
          this.attempts--;
        }
        this.checkCompletion();
      }
      this.letter = "";
    },
    checkCompletion() {
      if (this.spaces.length === this.existingLetters.length) {
        this.$q
          .dialog({
            title: "¡Felicitaciones!",
            message: "Has completado el juego. ¿Quieres volver a empezar?",
            cancel: false,
            persistent: true
          })
          .onOk(() => {
            location.reload();
          });
      } else if (this.attempts === 0) {
        this.$q
          .dialog({
            title: "¡Perdiste! :(",
            message:
              "Has utilizado todas las oportunidades posibles. ¿Quieres volver a empezar?",
            cancel: false,
            persistent: true
          })
          .onOk(() => {
            location.reload();
          });
      }
    },
    showSuccess(letter) {
      this.$q.notify({
        message: "¡Muy bien!",
        caption: "La letra <b>" + letter + "</b> hace parte de la palabra",
        icon: "check",
        color: "secondary",
        html: true
      });
    }
  },
  beforeMount() {
    this.graphWordSpaces();
  }
};
</script>

<style lang="scss" scoped>
#attempts {
  font-size: 16px;
  position: absolute;
  top: 1em;
  left: 1em;
}
.letter {
  width: 150px;
  height: 150px;
  font-size: 110px;
  display: flex;
  justify-content: center;
  align-content: center;
  font-weight: bold;
  background-color: rgba(0, 0, 0, 0.2);
}
label {
  width: 400px;
}
input {
  text-transform: uppercase;
}
#entered-letters {
  span {
    font-size: 40px;
    margin-left: 10px;
  }
}
#clue {
  font-size: 25px;
}

@media (min-device-width: 320px) and (max-device-width: 700px) {
  .letter {
    width: 40px;
    height: 40px;
    font-size: 30px;
  }
}
</style>
