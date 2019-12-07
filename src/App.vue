<template>
  <v-app>
    <v-container style="max-width: 1000px;">
      <ShowOrder/>
    </v-container>
  </v-app>
</template>

<script>
import ShowOrder from "@/components/order/ShowOrder"

export default {
  components: {
    ShowOrder,
  }
};
</script>

<style>
* {
  font-family: '微軟正黑體'!important;
}
</style>