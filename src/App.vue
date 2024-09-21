<template>
<NavBarView></NavBarView>
<router-view></router-view>
</template>

<script>
import NavBarView from './components/NavBarView.vue'

export default {
  name: "APP",
  components: {
    NavBarView
  }
}
</script>

<style>

</style>
