<template>
  <div class="title-bar">
    <div class="title-label">{{title}}</div>
    <div class="title-buttons">
      <div class="title-min-btn" @click="minimize()" v-if="minBtn">&#128469;&#xFE0E;</div>
      <div class="title-max-btn" @click="maximize()" v-if="maxBtn">&#128471;&#xFE0E;</div>
      <div class="title-close-btn" @click="close()" v-if="closeBtn">&#128473;&#xFE0E;</div>
    </div>
  </div>
</template>

<script>
const { remote } = window.require("electron");

import swal from "sweetalert";

export default {
  props: {
    title: {
      type: String,
      default: "Schetovod",
    },
    minBtn: {
      type: Boolean,
      default: false,
    },
    maxBtn: {
      type: Boolean,
      default: false,
    },
    closeBtn: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      remote: remote,
    };
  },
  methods: {
    minimize: function () {
      remote.getCurrentWindow().minimize();
    },
    maximize: function () {
      if (remote.getCurrentWindow().isMaximized())
        remote.getCurrentWindow().unmaximize();
      else remote.getCurrentWindow().maximize();
    },
    close: function () {
      swal({
        title: "Ви впевнені що хочете вийти з програми?",
        buttons: ["Ні", "Так"],
        icon: "warning",
        dangerMode: true,
      }).then((willDelete) => {
        if (willDelete) {
          remote.app.exit();
        }
      });
    },
  },
};
</script>

<style scoped>
.title-bar {
  user-select: none;
  display: flex;
  justify-content: space-between;
  width: 100%;
  height: 30px;
  padding: 0;
  margin: 0;
  background-color: #303030;
  -webkit-app-region: drag;
  cursor: default;
}

.title-label {
  color: whitesmoke;
  padding: 5px;
}

.title-buttons {
  -webkit-app-region: no-drag;
  display: flex;
  justify-content: flex-end;
  align-items: flex-end;
}

.title-min-btn,
.title-max-btn,
.title-close-btn {
  text-align: center;
  box-sizing: border-box;
  width: 30px;
  height: 30px;
  padding-top: 5px;
  border: none;
  background-color: #303030;
  color: rgb(146, 146, 146);
  font-size: medium;
  cursor: pointer;
}

.title-min-btn:hover,
.title-max-btn:hover {
  background-color: #4b4e4e;
  color: #fff;
}

.title-close-btn:hover {
  background-color: #9b373f;
  color: #fff;
}
</style>