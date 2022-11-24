<template>
  <div class="table-block">
    <div class="table-block__sorting sorting">
      <div class="sorting__text">Сортировка:</div>
      <ul class="sorting__list">
        <li
          @click="setDateFilter"
          :class="{ activeClass: dateFilter }"
          class="sorting__item"
        >
          Дата регистрации
        </li>
        <li
          @click="setRateFilter"
          :class="{ activeClass: rateFilter }"
          class="sorting__item"
        >
          Рейтинг
        </li>
      </ul>
    </div>
    <div class="table-block__table">
      <table class="table">
        <thead class="table__head">
          <th>Имя пользователя</th>
          <th>E-mail</th>
          <th>Дата регистрации</th>
          <th>Рейтинг</th>
        </thead>
        <tbody class="table__body">
          <tr
            v-for="user in paginatedUsersList"
            :key="user.id"
            class="table__row"
          >
            <td class="table__name">{{ user.username }}</td>
            <td class="table__email">{{ user.email }}</td>
            <td class="table__date">
              {{ formatDate(user.registration_date) }}
            </td>
            <td class="table__rate">{{ user.rating }}</td>
            <td @click="showPopup(user)" class="table__delete">
              <img src="@/icons/UsersList/delete.svg" alt="X" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="table-block__pagination">
      <button @click="prevPage" class="button">Назад</button>
      {{ page }}
      <button @click="nextPage" class="button">Вперед</button>
    </div>
  </div>
  <remove-popup
    @closePopup="closePopup"
    @removeUser="removeUser"
    v-show="isShowPopup"
  />
</template>

<script>
import RemovePopup from "@/components/UsersList/RemovePopup.vue";

export default {
  props: {
    searchText: String,
    clearedFilter: Number,
  },
  components: {
    RemovePopup,
  },
  data() {
    return {
      usersList: null,

      dateFilter: null,
      rateFilter: null,

      page: 1,

      isShowPopup: false,
      deletedUser: null,
    };
  },
  methods: {
    formatDate(date) {
      date = new Date(date);
      return date.toLocaleDateString("ru-RU");
    },
    setDateFilter() {
      this.rateFilter = null;
      if (!this.dateFilter | (this.dateFilter === "decrease")) {
        this.dateFilter = "increase";
      } else {
        this.dateFilter = "decrease";
      }
      this.sortUsersByDate();
    },
    sortUsersByDate() {
      if (this.dateFilter === "increase") {
        this.usersList.sort(
          (a, b) =>
            new Date(a.registration_date) - new Date(b.registration_date)
        );
      } else if (this.dateFilter === "decrease") {
        this.usersList.sort(
          (a, b) =>
            new Date(b.registration_date) - new Date(a.registration_date)
        );
      }
    },
    setRateFilter() {
      this.dateFilter = null;
      if (!this.rateFilter | (this.rateFilter === "decrease")) {
        this.rateFilter = "increase";
      } else {
        this.rateFilter = "decrease";
      }
      this.sortUsersByRate();
    },
    sortUsersByRate() {
      if (this.rateFilter === "increase") {
        this.usersList.sort((a, b) => a.rating - b.rating);
      } else if (this.rateFilter === "decrease") {
        this.usersList.sort((a, b) => b.rating - a.rating);
      }
    },
    prevPage() {
      this.page = this.page > 1 ? --this.page : this.page;
    },
    nextPage() {
      console.log(this.hasNextPage);
      this.hasNextPage ? ++this.page : this.page;
    },
    closePopup(value) {
      this.isShowPopup = value;
    },
    showPopup(user) {
      this.isShowPopup = true;
      this.deletedUser = user;
    },
    removeUser() {
      this.usersList = this.usersList.filter(
        (user) => user.id !== this.deletedUser.id
      );
      this.isShowPopup = false;
    },
  },
  computed: {
    filteredUsersList() {
      return this.usersList?.filter(
        (user) =>
          user.username.toLowerCase().includes(this.searchText.toLowerCase()) |
          user.email.toLowerCase().includes(this.searchText.toLowerCase())
      );
    },
    paginatedUsersList() {
      return this.filteredUsersList?.slice(this.startIndex, this.endIndex);
    },
    startIndex() {
      return (this.page - 1) * 5;
    },
    endIndex() {
      return this.page * 5;
    },
    hasNextPage() {
      return this.filteredUsersList.length > this.endIndex;
    },
  },
  watch: {
    clearedFilter() {
      if (!this.dateFilter && !this.rateFilter) return;
      this.dateFilter = null;
      this.rateFilter = null;
      this.usersList.sort((a, b) => a.id - b.id);
    },
    paginatedUsersList() {
      if (this.paginatedUsersList.length === 0) {
        console.log(this.page);
        while (this.paginatedUsersList.length === 0 && this.page > 1) {
          this.page = this.page - 1;
        }
      }
    },
  },
  mounted() {
    fetch("https://5ebbb8e5f2cfeb001697d05c.mockapi.io/users")
      .then((response) => response.json())
      .then((data) => (this.usersList = data));
  },
};
</script>

<style lang="scss">
.table-block {
  overflow: auto;
  font-size: 10px;
  color: #9eaab4;
  &__sorting {
    display: flex;
  }
  &__pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
    margin-top: 15px;
  }
}
.sorting {
  margin: 0 0 15px 0;

  &__list {
    display: flex;
  }

  &__item {
    margin: 0 0 0 8px;
    border-bottom: 1px dashed #9eaab4;
    cursor: pointer;
    user-select: none;
  }
}
.activeClass {
  color: #333333;
  border-bottom: 1px dashed #333333;
}
.table {
  width: 100%;
  background-color: #fff;
  border-radius: 7px;
  padding: 16px 16px 0 16px;

  &__head {
    & th {
      text-align: start;
      font-weight: 500;
      font-size: 10px;
      color: #9eaab4;
      padding-bottom: 22px;
    }
  }

  &__body {
    font-weight: 500;
    font-size: 12px;
    color: #acacac;
    & td {
      padding: 14px 0 20px 0;
      border-top: 1px solid #f7f7f7;
    }
  }

  &__name {
    font-weight: 700;
    color: #0cb4f1;
    width: 24%;
  }

  &__email {
    width: 24%;
  }

  &__date {
    width: 24%;
  }

  &__rate {
    width: 24%;
  }

  &__delete {
    width: 4%;
    min-width: 10px;
    img {
      cursor: pointer;
      width: 11px;
      height: 11px;
    }
  }
}
.button {
  padding: 4px 24px;
  background: #e0e0e0;
  border-radius: 3px;
  border: 0;
  color: #828282;
  cursor: pointer;
  &:hover {
    background: #0cb4f1;
    color: #fff;
    transition-duration: 0.5s;
  }
}
</style>
