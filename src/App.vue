<template>
  <users-list />
</template>

<script>
import UsersList from "./components/UsersList.vue";

export default {
  name: "App",
  components: {
    UsersList,
  },
};
</script>

<style lang="scss">
@import url("./assets/nullStyle.scss");
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap");
#app {
  font-family: Inter;
  font-weight: 500;
}
body {
  background-color: #f7f7f7;
}
</style>
