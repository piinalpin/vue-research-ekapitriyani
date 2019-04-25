<template>
  <div class="app">
    <div class="app-body">
      <main class="main" style="background-image: url(img/background.jpg)">
        <div class="container-fluid">
          <router-view></router-view>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DefaultContainer',
  computed: {
    name () {
      return this.$route.name
    },
    list () {
      return this.$route.matched.filter((route) => route.name || route.meta.label )
    }
  }
}
</script>
