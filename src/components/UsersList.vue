<template>
  <div class="container">
    <div class="users-list">
      <h1 class="users-list__title">Список пользователей</h1>
      <users-list-filter
        @inputEvent="getSearchText"
        @clearFilter="clearFilter"
      />
      <users-list-table
        :clearedFilter="clearedFilter"
        :searchText="searchText"
      />
    </div>
  </div>
</template>

<script>
import UsersListFilter from "./UsersList/UsersListFilter.vue";
import UsersListTable from "./UsersList/UsersListTable.vue";

export default {
  components: {
    UsersListFilter,
    UsersListTable,
  },
  data() {
    return {
      searchText: "",
      clearedFilter: 0,
    };
  },
  methods: {
    getSearchText(value) {
      this.searchText = value;
    },
    clearFilter() {
      this.searchText = "";
      ++this.clearedFilter;
    },
  },
};
</script>

<style scoped lang="scss">
.container {
  max-width: 991px;
  padding: 0 15px;
  margin: 0 auto;
  font-size: 12px;
}
.users-list {
  &__title {
    font-weight: 700;
    font-size: 24px;
    line-height: 28px;
    color: #333333;
    margin: 0.8rem 0 1.2rem 0;
  }
}
</style>
