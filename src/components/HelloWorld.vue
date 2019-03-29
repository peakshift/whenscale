<template>
  <div class="bitcoin-tps half">
    <h1>
      {{msg}}
      <button @click="btcTransactions()">BTC</button>
      <button @click="ethTransactions()">ETH</button>
      <span class="loading" v-show="loading">LOADING...</span>
      <!-- <button @click="fillData()">Randoooom</button> -->
    </h1>
    <line-chart :chartData="datacollection"></line-chart>
  </div>
</template>

<script>
import axios from 'axios'
import LineChart from './charts/LineChart'

export default {
  name: 'HelloWorld',
  components: {
    LineChart
  },
  data () {
    return {
      datacollection: null,
      bitcoinTps: null,
      loading: null,
      errored: null,
      timestamps: null,
      tps: null,
      msg: 'Transactions per second'
    }
  },
  mounted () {
    this.btcData()
  },
  methods: {
    switchData (d) {

    },
    btcData () {
      this.loading = true

      axios.get('https://api.blockchain.info/charts/transactions-per-second?timespan=1days&rollingAverage=24hours&format=json')
        .then((res) => {
          this.timestamps = res.data.values.map((value) => {
            return value.x
          })

          this.tps = res.data.values.map((value) => {
            return value.y
          })

          this.datacollection = {
            labels: this.timestamps,
            datasets: [
              {
                label: 'Bitcoin Transactions Per Second',
                backgroundColor: '#f87979',
                data: this.tps
              }
            ]
          }

          console.log('response', res.data.values)
        })
        .catch((err) => {
          this.errored = true
          console.log('error', err)
        })
        .finally(() => {
          this.loading = false
        })
    },
    ethData () {
      this.loading = true

      axios.get('https://api.blockchain.info/charts/transactions-per-second?timespan=1days&rollingAverage=24hours&format=json')
        .then((res) => {
          this.timestamps = res.data.values.map((value) => {
            return value.x
          })

          this.tps = res.data.values.map((value) => {
            return value.y
          })

          this.datacollection = {
            labels: this.timestamps,
            datasets: [
              {
                label: 'Bitcoin Transactions Per Second',
                backgroundColor: '#f87979',
                data: this.tps
              }
            ]
          }

          console.log('response', res.data.values)
        })
        .catch((err) => {
          this.errored = true
          console.log('error', err)
        })
        .finally(() => {
          this.loading = false
        })
    },
    fillData () {
      this.datacollection = {
        labels: this.timestamps,
        datasets: [
          {
            label: 'Bitcoin Transactions Per Second',
            backgroundColor: '#f87979',
            xAxisID: 'x',
            yAxisID: 'y',
            data: this.tps
          }
        ]
      }
    },
    getRandomInt () {
      return Math.floor(Math.random() * (50 - 5 + 1)) + 5
    }
  }
}
</script>

<style scoped>
h1 {
  text-align: left;
  text-transform: uppercase;
  font-size: 14px;
}
.loading {
  background-color: #AF000;
  border-radius: 1rem;
  padding: 0.3rem;
  font-weight: bold;
  float: right;
}
.half {
  width: 50%;
  padding: 2rem;
  background: #FFF;
  border-radius: 3px;

}
</style>
