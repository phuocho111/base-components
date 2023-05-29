<template>
  <div v-if="false" class="app-loader-container d-flex flex-column">
    <div class="d-flex flex-row align-self-center my-auto">
      <div class="loader" />
      <div class="fourth-step-loader" />
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    isLoading() {
      const isCallingAPIs =
        this.$store.state.commonAppState.fullScreenLoadingModalDisplay
      return isCallingAPIs
    },
  },
}
</script>

<style lang="scss" scoped>
.app-loader-container {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 10;
  backdrop-filter: blur(8px);
  background-color: rgba(255, 255, 255, 0.2);
  // z-index: 1000;
}

.loader,
.loader:before,
.loader:after,
.fourth-step-loader,
.fourth-step-loader:after {
  background: #32a2ec;
  -webkit-animation: load1 1s infinite ease-in-out;
  animation: load1 1s infinite ease-in-out;
  width: 1em;
  height: 1.2em;
}
.loader,
.fourth-step-loader {
  color: #32a2ec;
  text-indent: -9999em;
  margin: 88px auto 88px 18px;
  position: relative;
  font-size: 9px;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
  -webkit-animation-delay: -0.16s;
  animation-delay: -0.16s;
}
.loader:before,
.loader:after,
.fourth-step-loader:after {
  position: absolute;
  top: 0;
  content: '';
}
.loader:before {
  left: -1.5em;
  -webkit-animation-delay: -0.32s;
  animation-delay: -0.32s;
}
.loader:after,
.fourth-step-loader:after {
  left: 1.5em;
}

.fourth-step-loader {
  animation-delay: 0.01s;
}

.fourth-step-loader:after {
  animation-delay: 0.1s;
}

@-webkit-keyframes load1 {
  0%,
  80%,
  100% {
    box-shadow: 0 0;
    height: 1.8em;
  }
  40% {
    box-shadow: 0 -2em;
    height: 3em;
  }
}
@keyframes load1 {
  0%,
  80%,
  100% {
    box-shadow: 0 0;
    height: 1.8em;
  }
  40% {
    box-shadow: 0 -2em;
    height: 3em;
  }
}

.fourth-step-loader:before {
  display: none;
}
</style>
