<template>
  <div id="navbar">
    <nav class="navbar navbar-expand-lg navbar-dark darker fixed-top">
      <div class="container-fluid">
        <router-link :to="{ name: 'local' }" class="navbar-brand">
          <img src="../assets/images/logo.svg" type="image/svg" alt="covidnepal" class="logo" />
        </router-link>
        <button
          class="navbar-toggler no-outline"
          type="button"
          data-toggle="collapse"
          data-target="#navdown-bar"
          aria-controls="navdown-bar"
          aria-expanded="false"
          aria-label="See Navigation Contents"
        >
          <span>
            <ion-icon name="reorder-three-outline" class="larger"></ion-icon>
          </span>
        </button>

        <div class="collapse navbar-collapse" id="navdown-bar">
          <ul class="navbar-nav ml-auto">
            <li class="nav-item">
              <span
                class="nav-link text-warning cursor"
                v-if="updateExists"
                @click="refreshApp"
                @mouseover="showMessage"
                @mouseout="goBack"
              >{{this.defMessage}}</span>
            </li>
            <li class="nav-item">
              <router-link :to="{ name: 'maps' }" class="nav-link">Districts</router-link>
            </li>
            <li class="nav-item">
              <router-link :to="{ name: 'provinces' }" class="nav-link">Provinces</router-link>
            </li>
            <li class="nav-item">
              <router-link :to="{ name: 'local' }" class="nav-link">Nepal</router-link>
            </li>
            <li class="nav-item">
              <router-link :to="{ name: 'global' }" class="nav-link">WorldWide</router-link>
            </li>
            <li class="nav-item">
              <router-link :to="{ name: 'news' }" class="nav-link">News</router-link>
            </li>
            <li class="nav-item">
              <router-link :to="{ name: 'about' }" class="nav-link">About</router-link>
            </li>
            <li class="nav-item">
              <a target="_blank" class="nav-link" href="https://t.me/covidnepal_bot">Telegram</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "navbar",
  data() {
    return {
      refreshing: false,
      registration: null,
      updateExists: false,
      defMessage: "Update Available!"
    };
  },
  created() {
    document.addEventListener("swUpdated", this.showRefreshUI, { once: true });
    navigator.serviceWorker.addEventListener("controllerchange", () => {
      if (this.refreshing) return;
      this.refreshing = true;
      window.location.reload();
    });
  },
  methods: {
    showMessage(e) {
      this.defMessage = "Click to Update!";
    },
    goBack(e) {
      this.defMessage = "Update Available!";
    },
    showRefreshUI(e) {
      this.registration = e.detail;
      this.updateExists = true;
    },
    refreshApp() {
      this.updateExists = false;
      if (!this.registration || !this.registration.waiting) {
        return;
      }
      this.registration.waiting.postMessage("skipWaiting");
    }
  }
};
</script>

<style>
.logo {
  height: 32px;
}

.no-outline {
  outline: none !important;
  border: 0 !important;
}

.cursor {
  cursor: pointer;
}
</style>
