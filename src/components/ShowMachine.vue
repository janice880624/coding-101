<template>
  <v-card>
    <v-card-title>
      <h2>查詢機台</h2>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="machines"
      :search="search"
      class="elevation-1"
    ></v-data-table>
  </v-card>
</template>

<script>
import { db } from "@/plugins/db.js";

export default {
  data() {
    return {
      headers: [
        { text: "機台編號", value: "id" },
        { text: "機台狀態", value: "status" },
        { text: "處理訂單", value: "orderHanding"}
      ],
      machines: []
    };
  },
  firebase() {
    return {
      machines: db.ref("/machines")
    };
  }
};
</script>