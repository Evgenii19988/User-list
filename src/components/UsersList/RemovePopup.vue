<template>
  <div class="container">
    <div class="popup">
      <div class="popup__question">
        Вы уверены, что хотите удалить пользователя?
      </div>
      <div class="popup__buttons">
        <button @click="this.$emit('removeUser')" class="popup__yes button">
          Да
        </button>
        <button
          @click="this.$emit('closePopup', false)"
          class="popup__no button"
        >
          Нет
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
.container {
  background-color: rgba(0, 0, 0, 0.5);
  position: fixed;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
.popup {
  width: 353px;
  height: 134px;
  background: #fff;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  &__question {
    font-weight: 400;
    font-size: 12px;
    color: #000000;
    text-align: center;
    padding-bottom: 24px;
  }

  &__buttons {
    display: flex;
    gap: 36px;
    justify-content: center;
  }
}
.button {
  padding: 6px 36px;
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
