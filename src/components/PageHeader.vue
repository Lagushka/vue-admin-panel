<template>
  <header class="header">
    <h1>Vue-order</h1>
    <div class="toggle-theme">
      <span>Темная тема</span>
      <button @click="changeTheme">
        <img v-if="!darkTheme" src="../assets/toggle-off.svg" alt="" width="80">
        <img class="turn-on" v-if="darkTheme" src="../assets/toggle-on.svg" alt="" width="80">
        <div v-if="darkTheme"></div>
        <span v-if="!darkTheme" class="off-text">off</span>
        <span v-if="darkTheme" class="on-text">on</span>
      </button>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      darkTheme: false,
    };
  },
  methods: {
    changeTheme: function () {
      this.darkTheme = !this.darkTheme;
      if (this.darkTheme) {
        document.documentElement.setAttribute('theme', 'dark');
      } else {
        document.documentElement.setAttribute('theme', 'light');
      }
    },
  },
};
</script>

<style lang="scss" scoped>
  header {
    display: flex;
    flex-direction: row;
    box-sizing: border-box;
    width: 100%;
    border: 3px solid;
    justify-content: space-between;
    align-items: center;

    h1 {
      margin-left: 50%;
      transform: translateX(-50%);
    }

    .toggle-theme {
      display: flex;
      flex-direction: row;
      align-items: center;
      button {
        position: relative;
        border: none;
        display: flex;
        cursor: pointer;

        .turn-on {
          z-index: 1;
        }

        div {
          position: absolute;
          height: 35px;
          width: 67px;
          background-color: white;
          z-index: 0;
          top: 23px;
          left: 13px;
          border-radius: 30%;
        }

        span {
          position: absolute;
          font-size: 16px;
        }

        .off-text {
          color: white;
          right: 30%;
          top: 38%
        }

        .on-text {
          color: black;
          z-index: 1;
          top: 38%;
          left: 30%;
        }
      }
    }
  }
</style>
