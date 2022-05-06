<template>
  <div class="app">
    <user-table />
  </div>
</template>

<script>
import UserTable from "@/components/UserTable.vue";
export default {
  name: "App",
  components: {
    UserTable,
  },
  data: () => ({
    users: [],
  }),
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app {
  box-shadow: 0px 4px 27px rgba(230, 231, 234, 0.78);
}
</style>