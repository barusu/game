<style lang="scss">
</style>

<template>
  <div class="color">
    <h2><span>Color</span> <span class="chinese">配色</span></h2>
    <p>我喜欢的一些配色.</p>
    <h3><span>Base</span></h3>
    <div class="clearfix">
      <div class="item" v-for="(c, index) in colors1" :style="{background: c}" @click="copy(index, 'colorEl1')"><input type="text" readonly :value="c" ref='colorEl1'></div>
    </div>
    <div class="clearfix">
      <div class="item" v-for="(c, index) in colors1" :style="{color: c, borderColor: c}" @click="copy(index, 'colorEl1')" v-html="c"></div>
    </div>
    <h3><span>China</span></h3>
    <div class="clearfix">
      <div class="item" v-for="(c, index) in colors2" :style="{background: c}" @click="copy(index, 'colorEl2')"><input type="text" readonly :value="c" ref='colorEl2'></div>
    </div>
    <div class="clearfix">
      <div class="item" v-for="(c, index) in colors2" :style="{color: c, borderColor: c}" @click="copy(index, 'colorEl2')" v-html="c"></div>
    </div>
    <h3><span>Mac</span></h3>
    <div class="clearfix">
      <div class="item" v-for="(c, index) in colors3" :style="{background: c}" @click="copy(index, 'colorEl3')"><input type="text" readonly :value="c" ref='colorEl3'></div>
    </div>
    <div class="clearfix">
      <div class="item" v-for="(c, index) in colors3" :style="{color: c, borderColor: c}" @click="copy(index, 'colorEl3')" v-html="c"></div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        colors1: ['#409eff', '#67c23a', '#e6a23c', '#f56c6c', '#bbb'],
        colors2: ["#f0f4f3", "#d9eaf1", "#d5f0e7", "#a1e3c7", "#b36a63", "#d6b311", "#c96527", "#ff3102", "#9c2833", '#f25500', '#f00'],
        colors3: ['#fc605d', '#fdbc40', '#34c849']
      };
    },
    methods: {
      copy(index, eln) {
        try {
          this.$refs[eln][index].select();
          document.execCommand('copy');
        }catch(e) {console.log(e); }
      }
    }
  }
</script>
