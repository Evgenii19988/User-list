<template>
  <div class="email-filter">
    <div class="email-filter__input">
      <div class="email-filter__search">
        <img src="@/icons/UsersList/search.svg" alt="" />
      </div>
      <input
        v-model="searchText"
        @input="this.$emit('inputEvent', searchText)"
        class="input"
        type="text"
        placeholder="Поиск по имени или e-mail"
      />
    </div>
    <div class="email-filter__clear">
      <div class="email-filter__text" @click="clearFilter">Очистить фильтр</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchText: null,
    };
  },
  methods: {
    clearFilter() {
      this.searchText = "";
      this.$emit("clearFilter");
    },
  },
};
</script>

<style lang="scss">
.email-filter {
  background-color: #fff;
  background: #fff;
  box-shadow: 0px 18px 15px rgba(148, 148, 148, 0.15);
  border-radius: 7px;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 0 72px 0;
  &__input {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0 4.5% 0 1.7%;
    margin: 12px 0 24px 0;
  }

  &__search {
    background: #eceff0;
    height: 2rem;
    display: flex;
    align-items: center;
    border-radius: 4px 0 0 4px;
    & img {
      padding: 0 4px;
    }
  }

  &__clear {
    width: 100%;
    margin: 0 0 16px 0;
    padding: 0 0 0 1.7%;
  }

  &__text {
    &::before {
      content: "";
      background-image: url(@/icons/UsersList/clear.svg);
      background-size: 80%;
      background-repeat: no-repeat;
      padding: 0 15px 0 0;
    }
    user-select: none;
    font-weight: 500;
    font-size: 12px;
    color: #4f4f4f;
    display: inline-block;
    cursor: pointer;
  }
}

.input {
  background: #eceff0;
  border-radius: 0 4px 4px 0;
  border: 0;
  width: 100%;
  height: 2rem;
  outline: none;
  &::placeholder {
    color: #9eaab4;
    background-size: 18px;
  }
}
</style>
