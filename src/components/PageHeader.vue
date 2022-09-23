<template>
  <header class="header">
    <h1>Vue-order</h1>
    <div class="theme-button">
      <span>Темная тема</span>
      <button @click="changeTheme">
        <div class="circle"></div>
        <span>off</span>
      </button>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    changeTheme: function () {
      this.darkTheme = !this.darkTheme;
      this.$emit("changeTheme", this.darkTheme);
    },
  },
};
</script>

<style lang="scss" scoped></style>
