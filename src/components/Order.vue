<template>
  <div>
    <v-card>
      <v-card-title>
        查詢訂單
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="orders"
        :search="search"
      ></v-data-table>
    </v-card>

    <h1 class="font mt-5">增加訂單</h1>
    <v-form
      ref="form"
      lazy-validation
    >
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

      <v-btn
        class="mt-3"
        color="error"
        @click="resetForm"
      >
        重置
      </v-btn>

      <v-btn
        class="mt-3 ml-3"
        color="primary"
        @click="setFirebase"
      >
        提交
      </v-btn>
    </v-form>
  </div>
</template>

<script>
import { db } from '@/plugins/db'

export default {
  data() {
    return {
      orders: [],
      customer: "",
      machines: ['M01', 'M02', 'M03', 'M04', 'M05'],
      machine_select: "",
      date: "",
      search: "",
      headers: [
        { text: '訂單名稱', value: 'name' },
        { text: '客戶名稱', value: 'customer' },
        { text: '狀態', value: 'status' },
        { text: '負責機器', value: 'machine' },
        { text: '日期', value: 'date' }
      ]
    };
  },
  watch: {
    menu(val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
    },
  },
  methods: {
    setFirebase() {
      // const user = { ...this.user }
      if (this.$refs.form.validate()) {
        this.snackbar = true;
        const order = {
          customer: this.customer,
          machine: this.machine_select,
          date: this.date,
          status: "未處理"
        };

        order.name = this.$md5(String(Date.now()))

        db.ref('orders/').push(order);
      }
    },
    save(date) {
      this.$refs.menu.save(date)
    },
    resetForm() {
      this.machine_select = "";
      this.customer = "";
      this.date = "";
    }
  },
  firebase() {
    return {
      orders: db.ref("/orders")
    }
  }
};
</script>

