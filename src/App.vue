<template>
  <div id="app">
    <div id="nav">
      <router-link to="/v-viewer">V-viewer</router-link> |
      <router-link to="/practice_model">model选项</router-link> |
      <router-link to="/practice_grid">grid栅格布局</router-link> |
      <router-link to="/practice_drag">自由拖拽选择区域</router-link> |
      <router-link to="/practice_table">动态渲染video标签中的src</router-link>
    </div>
    <router-view />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
