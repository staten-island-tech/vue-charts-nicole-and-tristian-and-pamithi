<template>
  <h1>Crime Based on Age</h1>
  <main>
    <div class="flex-container">
      <div class="chartcontainer">
        <Bar :data="chartData" :options="options"/>
      </div>
    </div>
  </main>
</template>

<script>
import { Bar} from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: { Bar },
  data () {
    return {
      loaded: false,
      chartData: {
        labels: ['<18', '18-24', '25-44', '45-64', '65+'],
        datasets: [
          {
            backgroundColor: ['#f7706e', '#497397', '#49f493', '#a16395', '#e9e572'],
            data: [54,178,564,189,15]
          }
        ],
        options:{
          responsive:true,
          maintainAspectRatio: false
        }
      }
    }
  },
  mounted:function (){
    this.fetchData()
  },
  methods: {
  fetchData: async function () {
      try {
        const result = await fetch('https://data.cityofnewyork.us/resource/uip8-fykc.json')
        const data = await result.json()
        console.log(data)
        const ages = [
          data.filter((item) => item.age_group === '<18').length,
          data.filter((item) => item.age_group === '18-24').length,
          data.filter((item) => item.age_group === '25-44').length,
          data.filter((item) => item.age_group === '45-64').length,
          data.filter((item) => item.age_group === '65+').length,

      ]

    this.chartData.datasets[1].data = ages;
      console.log(ages)


      } catch (error) {
        console.log(error)
      }

    }
  }
}
</script>
  
  <style scoped>

h1 {
  font-family: 'Hind', sans-serif;
  background-color: lightblue;
  margin: 4rem 35rem 1.5rem;
  border-radius: 1rem;
  font-size: 3rem;
  text-align: center;
}

  .flex-container {
    display: flex;
    flex: auto;
    justify-content: center;
  }
 
  .chartcontainer {
    margin-top: 50px;
    height: 650px;
    width: 1200px;
  }
  </style>
