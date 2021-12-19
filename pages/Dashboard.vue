<template>
  <div>
    <h3>Bar Chart Example in Vue</h3>
    <v-row justify="center" align="center">
      <v-col md="7">
        <v-card>
          <bar-chart></bar-chart>
        </v-card>
      </v-col>
      <v-spacer />

      <v-col md="5" >
        <v-card >
          <h4 > จำนวนคนใน Type Area </h4>
          <polar-area-chart></polar-area-chart>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import BarChart from "@/components/BarChart";
import PolarAreaChart from '@/components/PolarChart'
//const url = "http://localhost:5000/api/village";
export default {
  
  components: {
    BarChart,
    PolarAreaChart,
  },

  //methods: {
    //async initialize () {
      //const res = await this.$axios.get(url);
      //this.personlist = res.data.response;
    //},
  //},
}
</script>