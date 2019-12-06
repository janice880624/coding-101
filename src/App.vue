<template>
  <v-app>
    <v-container style="max-width: 800px;">
      <Order/>
    </v-container>
  </v-app>
</template>

<script>
import Order from "@/components/Order"

export default {
  components: {
    Order
  }
};
</script>

<style>
* {
  font-family: '微軟正黑體'!important;
}
</style>