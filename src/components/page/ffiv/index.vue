<template>
  <main class="ffiv">
    <router-link :to="{name: 'materia'}">TEST</router-link>
    <router-view/>
  </main>
</template>

<script>
export default {
  components: {},
  data() {
    return {};
  },
  methods: {},
  computed: {}
}
</script>

<style lang="scss">
.ffiv {
  font-size: 16px;
}
</style>