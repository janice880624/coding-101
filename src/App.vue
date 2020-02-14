<template>
  <v-app>
    <v-container style="max-width: 1000px;">
      <ShowMachine class="mt-5" />
      <ShowOrder class="mt-8" />
      <ShowSensorData class="mt-5" />
    </v-container>
  </v-app>
</template>

<script>
import ShowOrder from "@/components/order/ShowOrder"
import ShowMachine from "@/components/ShowMachine"
import ShowSensorData from "@/components/ShowSensorData"

export default {
  components: {
    ShowOrder,
    ShowMachine,
    ShowSensorData
  }
};
</script>

<style>
* {
  font-family: '微軟正黑體'!important;
}
</style>