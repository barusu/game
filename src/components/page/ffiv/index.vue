<template>
  <main class="ffiv">
    <div class="guide">
      <router-link :to="{name: 'materia'}">采集460</router-link>
      <router-link :to="{name: 'GatheringTimer'}">采集</router-link>
    </div>
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