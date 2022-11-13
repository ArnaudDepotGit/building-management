<template>
  <div id="app">
    <header>
      <div class="title">{{title}}</div>
    </header>

    <section class="main-content w-50 mx-auto">
      <main-navigation @panel-change="updatePanel"></main-navigation>
      <windows-list v-if="currentPanel.name === 'PanelWindows'"></windows-list>
      <rooms-list v-if="currentPanel.name === 'PanelRooms'"></rooms-list>
    </section>
  </div>
</template>

<script>
import MainNavigation from './components/MainNavigation.vue';
import RoomsList from './components/RoomsList.vue';
import WindowsList from './components/WindowsList.vue';

export default {
  name: 'App',
  components: {
    MainNavigation,
    RoomsList,
    WindowsList
  },
  data: function() {
    return {
      title: 'Building management',
      currentPanel: {}
    }
  },
  methods: {
    updatePanel(newPanel) {
      this.currentPanel = newPanel;
    }
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.title {
  font-size: 2em;
  margin-bottom: 1em;
}

</style>
