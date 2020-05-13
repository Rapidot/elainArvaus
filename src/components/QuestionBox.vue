<template>
  <div class="question-box-container">
    <b-jumbotron>

      <template v-slot:lead>
        {{ xQuestion.question}}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(vastaus, listIndex) in sekoitetutVastauksetArr" :key="listIndex"
        @click.prevent="listaTieto(listIndex)"
        :class="vastausClass(listIndex)">
          {{ vastaus }}
        </b-list-group-item>
      </b-list-group>
      <b-button v-on:click="vastausValittu" variant="primary" :disabled="listaIndexi === null || clickedValittu === true">
        Valittu
      </b-button>
      <b-button v-on:click="seuraava" variant="success">
        Seuraava kyssäri
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash' //JS kirjasto datan louhimiseen
  export default {
    props:{
      xQuestion: Object,
      seuraava: Function,
      lisaa1: Function
    },
    data(){
      return {
        listaIndexi: null,
        oikeaVastaus: null,
        sekoitetutVastauksetArr: [],
        clickedValittu: false
      }
    },
    computed:{
/*       vastaukset(){
        let apu = [...this.xQuestion.incorrect_answers]
        apu.push(this.xQuestion.correct_answer)
        return apu
      }
 */    },
    watch:{ //triggaa, jos propsit päivittyvät
      xQuestion: {
        immediate: true, //pakottaa triggaamaan jo ensimmäisestä datan talletuksesta lähtien
        handler() {
          this.listaIndexi = null
          this.clickedValittu = false
          this.sekoitetutVastaukset()
        }
      }
    },
    methods: {
      listaTieto(listIndex) {
        this.listaIndexi = listIndex
      },
      vastausValittu(){
        let vastausOikein = false

        if (this.listaIndexi === this.oikeaVastaus) {
          vastausOikein = true
        }
        this.lisaa1(vastausOikein)
        this.clickedValittu = true
      },
      // eslint-disable-next-line vue/no-dupe-keys
      sekoitetutVastaukset() {
        let apu = [...this.xQuestion.incorrect_answers, this.xQuestion.correct_answer]
        this.sekoitetutVastauksetArr = _.shuffle(apu)
        this.oikeaVastaus = this.sekoitetutVastauksetArr.indexOf(this.xQuestion.correct_answer)
        //console.log("oikea vastaus = " + this.oikeaVastaus)
      },
      vastausClass(listIndex){
        let vastausClass = ''
        
        if (!this.clickedValittu && this.listaIndexi === listIndex)
        {
          vastausClass = 'valintaCSS'
        }
        else if (this.clickedValittu && this.oikeaVastaus === listIndex)
        {
          vastausClass = 'okCSS'
        }
        else if (this.clickedValittu && this.listaIndexi === listIndex && this.oikeaVastaus !== listIndex)
        {
          vastausClass = 'eiOkCSS'
        }
        return vastausClass
      }
    }
  }
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover{
  background-color: #eeeeee;
  cursor: pointer
}
.btn {
  margin: 0 5px;
}
.valintaCSS {
  background-color: rgb(0, 217, 255);
}
.okCSS {
  background-color: lightgreen;
}
.eiOkCSS {
  background-color: rgb(255, 72, 0);
}
</style>