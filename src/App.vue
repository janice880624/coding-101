<template>
  <v-app>
    <v-container style="max-width: 1000px;">
      <ShowMachine />
      <ShowOrder class="mt-5" />
    </v-container>
  </v-app>
</template>

<script>
import ShowOrder from "@/components/order/ShowOrder"
import ShowMachine from "@/components/ShowMachine"

export default {
  components: {
    ShowOrder,
    ShowMachine
  }
};
</script>

<style>
* {
  font-family: '微軟正黑體'!important;
}
</style>