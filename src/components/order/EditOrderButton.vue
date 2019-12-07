<template>
  <v-dialog v-model="dialog" max-width="500px" ref="test">
    <template v-slot:activator="{ on }">
      <v-icon small class="mr-2" v-on="on">mdi-brush</v-icon>
    </template>

    <v-card>
      <v-card-title>
        <span class="headline" style="font-family: 微軟正黑體!important">編輯訂單</span>
      </v-card-title>

      <v-card-text>
        <v-container>
          <v-form ref="orderForm">
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="orderInfo.orderName"
                  label="訂單編號"
                  required
                  :rules="[v => !!v || 'This term is required']"
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="orderInfo.customer"
                  label="客戶名稱"
                  required
                  :rules="[v => !!v || 'This term is required']"
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="6" md="4">
                <v-select
                  v-model="orderInfo.status"
                  :items="formSelect.status"
                  label="訂單狀態"
                  :rules="[v => !!v || 'This term is required']"
                  required
                ></v-select>
              </v-col>

              <v-col cols="12" sm="6" md="4">
                <v-select
                  v-model="orderInfo.machine"
                  :items="formSelect.machines"
                  label="負責機器"
                  :rules="[v => !!v || 'This term is required']"
                  required
                ></v-select>
              </v-col>

              <v-col cols="12" sm="6" md="4">
                <v-menu
                  ref="menu"
                  :close-on-content-click="false"
                  transition="scale-transition"
                  offset-y
                  min-width="300px"
                >
                  <template v-slot:activator="{ on }">
                    <v-text-field
                      v-model="orderInfo.completeDate"
                      label="完成日期"
                      readonly
                      v-on="on"
                      required
                      :rules="[v => !!v || 'This term is required']"
                    ></v-text-field>
                  </template>

                  <v-date-picker
                    ref="picker"
                    v-model="orderInfo.completeDate"
                    min="1999-01-01"
                    @change="saveDate"
                  ></v-date-picker>
                </v-menu>
              </v-col>
            </v-row>
          </v-form>
        </v-container>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="updateOrderInfo">確認</v-btn>
        <v-btn color="error darken-1" text @click="dialog = false">取消</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import { db } from "@/plugins/db.js";

export default {
  data() {
    return {
      dialog: false,
      formSelect: {
        machines: ["M01", "M02", "M03", "M04", "M05"],
        status: ["未處理", "已處理", "處理中"]
      }
    }
  },
  props: [
    "orderInfo"
  ],
  methods: {
    saveDate(date) {
      this.$refs.menu.save(date);
    },
    updateOrderInfo() {
      if (this.$refs.orderForm.validate()) {
        this.snackbar = true;

        db.ref("orders/" + this.orderInfo.id).update(this.orderInfo);

        this.$refs.orderForm.reset();
        this.dialog = false;
      }
    }
  }
};
</script>