<template>
  <v-dialog v-model="dialog" max-width="500px">
    <template v-slot:activator="{ on }">
      <v-btn color="error" dark class="mb-2 mr-5" v-on="on">刪除訂單</v-btn>
    </template>

    <v-card>
      <v-card-title>
        <span class="headline" style="font-family: 微軟正黑體!important">新增訂單</span>
      </v-card-title>

      <v-card-text>確認要刪除嗎？</v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="removeOrder">確認</v-btn>
        <v-btn color="error darken-1" text @click="dialog = false">取消</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import { db } from "@/plugins/db";

export default {
  data() {
    return {
      dialog: false
    };
  },
  methods: {
    removeOrder() {
      this.selected.forEach(element => {
        db.ref("/orders/" + element.id ).remove()
      });

      this.dialog = false;
    }
  },
  props: [
    "selected"
  ]
};
</script>