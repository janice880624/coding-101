<template>
  <v-row>
    <v-col cols=12 sm=6 md=4 v-for="(sensorName, index) in sensorNames" :key="index">
      <v-card class="pa-5">
        <v-card-text class="text-center">
          <h1 class="text-center mb-5">{{ sensorName }}</h1>
          <div class="pb-0 title">{{ getSensorsData(index) }}</div>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import { db } from "@/plugins/db.js";

export default {
  data() {
    return {
      sensorNames: [
        "PM 2.5",
        "PM 10",
        "濕度",
        "亮度",
        "溫度"
      ],
      sensors: []
    }
  },
  computed: {
    getSensorsData() {
      return (index) => {
        let datas = this.sensors[index]
        let last_index = Object.keys(datas).pop()
        
        let data = this.sensors[index][last_index]
        return data[Object.keys(data)[0]].toFixed(2)
      }
    }
  },
  firebase() {
    return {
      sensors: db.ref("/sensor")
    };
  }
}
</script>