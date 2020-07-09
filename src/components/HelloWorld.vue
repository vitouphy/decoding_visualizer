
<template>
  <div class="app">
    
    <div style="width: 50%; display: inline-block; background-color: white;">

      <div>
        <h1 class="title">Decoding Strategies</h1>
        <h2 class="subtitle">Text Generation</h2>
        <br />
      </div>
      <form @submit="checkForm">
        <div style="display: inline-flex; width: 50%; ">
          <input class="input"  v-model="word" type="text" placeholder="Type Your Next Word">
          <button class="button is-primary">Go</button>
        </div>
      </form>
      <br />
      
      <div style="text-align: left">
        <h2 class="subtitle" style="font-weight: 500">Story:</h2>
        <p>{{ sentence }}</p>
      </div>
    </div>

    <hr />
    <div class="columns"> 
      <div class="column is-half">
   
        <div class="select">
          <select v-model="decode_method">
            <option value="default">Default</option>
            <option value="random">Random Sampling</option>
            <option value="nucleus">Nucleus Sampling</option>
          </select>
        </div>

        <div class="columns" style="margin-top: 10px; padding-left: 10px; padding-right: 10px;">
          <div class="column" v-if="decode_method == 'random'">
            <span>Temperature: {{slider_value}}</span>
            <vue-slider v-model="slider_value" v-bind="slide_option"/>
          </div>
          <div class="column" v-if="decode_method == 'random'">
            <span>TopK</span>
            <vue-slider v-model="slider_value" v-bind="slide_option"/>
          </div>
        </div>

        <!-- <line-chart :chart-data="datacollection"></line-chart> -->
        <bar-chart :chart-data="datacollection"></bar-chart>
        <!-- <button @click="fillData()">Randomize</button> -->
      </div>
    </div>
  </div>

  </div>
</template>

<script>

import LineChart from './LineChart.js'
import BarChart from './BarChart.js'
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/antd.css'

export default {
  name: 'HelloWorld',
  components: {
    LineChart,
    BarChart,
    VueSlider
  },
  data () {
    return {
      sentence: '',
      word: '',
      datacollection: null,
      slider_value: 0,
      slide_option: {
        min: 0,
        max: 1,
        interval: 0.01,
      },
      decode_method: "default"
    }
  },
  mounted () {
    // this.fillData()
  },
  methods: {
    checkForm () {
      this.sentence += this.word.trim() + " "
      this.word = ''
      this.fillData()
    },
    fillData () {
      var labels = []
      var data = []
      for (var i=0; i<100; i++) {
        var tmp_label = Math.random().toString(36).substring(2, 15)
        labels.push(tmp_label)
        data.push(this.getRandomInt())
      }
      this.datacollection = {
        labels: labels,
        datasets: [
          {
            label: 'Distribution of Vocabulary',
            backgroundColor: '#f87979',
            data: data
          }
        ]
      }
    },
    getRandomInt () {
      return Math.random().toFixed(2)
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

.small {
  max-width: 600px;
  margin:  150px auto;
}
</style>
