<template>
  <h1>Hello World!</h1>
  <div>
    <canvas id="myChart" width="1000" height="500"></canvas>
  </div>
</template>

<script lang="ts">
  import Chart from 'chart.js/auto';
  import axios from 'axios';

  var values = [];
  var dates = [];
  let myChart;

  export default {
    name: 'DesafioComponent',
    mounted() {
      this.getData();
      const data = {
        labels: dates,
        datasets: [{
          label: 'Data New York Market',
          data: values,
          borderColor: 'rgb(75, 192, 192)',
          backgroundColor: 'rgb(75, 192, 192)',
        }]
      };
      const config = {
        type: 'line',
        data: data,
        options: {}
      };
      myChart = new Chart(
        document.getElementById('myChart'),
        config
      )
    },
    methods: {
      myFunction() {
        console.log('antes', values);
        for(let i = 0; i < values.length; i++){
          values[i] = Math.random() * 10;
        }
        console.log('depois', values);
        myChart.update();
      },
      getData() {
        const response = axios.get('https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=IBM&apikey=demo').then((result) => {
        if(result.status == 200) {
          for(const[date, value] of Object.entries(result.data['Time Series (Daily)'])){
            dates.unshift(date);
            values.unshift(value['2. high']);
          }
          myChart.update();
          console.log('deu fetch', values);
          console.log('dates', dates)
        } else {
          console.error("Ocorreu um erro na API");
        }
        })
      }
    }
  }

</script>