<template>
  <v-dialog v-model="dialog" max-width="500px" ref="test">
    <template v-slot:activator="{ on }">
      <v-btn color="primary" dark class="mb-2" v-on="on">新增訂單</v-btn>
    </template>

    <v-card>
      <v-card-title>
        <span class="headline" style="font-family: 微軟正黑體!important">新增訂單</span>
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
                  v-model="orderInfo.machine"
                  :items="machinesForFormSelect"
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
        <v-btn color="blue darken-1" text @click="setOrderInfo">新增</v-btn>
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
      machinesForFormSelect: ["M01", "M02", "M03", "M04", "M05"],
      orderInfo: {
        orderName: "",
        customer: "",
        machine: "",
        completeDate: "",
        status: "未處理"
      }
    }
  },
  methods: {
    saveDate(date) {
      this.$refs.menu.save(date);
    },
    setOrderInfo() {
      if (this.$refs.orderForm.validate()) {
        this.snackbar = true;

        const uniqueId = db.ref("orders/").push(this.orderInfo).key;
        db.ref("orders/" + uniqueId).update({
          id: uniqueId
        });

        this.$refs.orderForm.reset();
        this.dialog = false;
      }
    }
  }
};
</script>