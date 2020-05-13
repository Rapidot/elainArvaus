<template>
  <div id="app">
    <Header
      :xNumOikein="numOikein"
      :xNumTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :xQuestion="questions[index]"
            :seuraava="seuraava"
            :lisaa1="lisaa1"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numOikein: 0,
      numTotal: 0
    }
  },
  methods: {
    seuraava() {
      this.index++
    },
    lisaa1(vastausOk){
      if (vastausOk) {
        this.numOikein++
      }
      this.numTotal++
      
    }
  },
  mounted: function() {
    fetch( 'https://opentdb.com/api.php?amount=10&category=27&type=multiple',{
      method: 'get'
    } )
      .then(( response ) => {
        return response.json()
      })
      .then((responseData) => {
        this.questions = responseData.results
      })
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
}
</style>
