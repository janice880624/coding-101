<template>
  <div>
    <v-card class="mt-5">
      <v-card-title>
        <h2>增加訂單</h2>
      </v-card-title>
      <v-card-text>
        <v-form ref="addOrder" lazy-validation>
          <v-text-field
            v-model="customer"
            label="客戶名稱"
            :rules="[v => !!v || 'This term is required']"
          ></v-text-field>
          <v-select
            v-model="machine_select"
            :items="machines"
            label="選擇機台"
            :rules="[v => !!v || 'This term is required']"
          ></v-select>

          <v-menu
            ref="menu"
            :close-on-content-click="false"
            transition="scale-transition"
            offset-y
            min-width="300px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                v-model="date"
                label="選擇日期"
                readonly
                v-on="on"
                required
                :rules="[v => !!v || 'This term is required']"
              ></v-text-field>
            </template>

            <v-date-picker
              ref="picker"
              v-model="date"
              :max="new Date().toISOString().substr(0, 10)"
              min="1950-01-01"
              @change="save"
            ></v-date-picker>
          </v-menu>

          <v-btn class="mt-3" color="error" @click="resetAddOrderForm">重置</v-btn>

          <v-btn class="mt-3 ml-3" color="primary" @click="addOrder">提交</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import { db } from "@/plugins/db";

export default {
  data() {
    return {
      orders: [],
      customer: "",
      machines: ["M01", "M02", "M03", "M04", "M05"],
      machine_select: "",
      date: "",
      headers: [
        { text: "訂單名稱", value: "name" },
        { text: "客戶名稱", value: "customer" },
        { text: "狀態", value: "status" },
        { text: "負責機器", value: "machine" },
        { text: "日期", value: "date" }
      ]
    };
  },
  watch: {
    menu(val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = "YEAR"));
    }
  },
  methods: {
    addOrder() {
      // const user = { ...this.user }
      if (this.$refs.addOrder.validate()) {
        this.snackbar = true;
        const order = {
          customer: this.addcustomer,
          machine: this.addmachine_select,
          date: this.date,
          status: "未處理"
        };

        order.name = this.$md5(String(Date.now()));

        db.ref("orders/").push(order);
      }
    },
    save(date) {
      this.$refs.menu.save(date);
    },
    resetAddOrderForm() {
      this.machine_select = "";
      this.customer = "";
      this.date = "";
    }
  },
  firebase() {
    return {
      orders: db.ref("/orders")
    };
  }
};
</script>
