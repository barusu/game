<style lang="scss">
  header {
    position: fixed;
    top: 0; left: 0;
    width: 100%;
    color: #444;
    z-index: 9;
    height: .6rem;
    padding: 0 0 0 5%;
    font-size: .2rem;
    background: #fff;
    transition: box-shadow .5s;
    box-shadow: 0 1px #ddd;
    &:hover {
      box-shadow: 0 1px 6px rgba(0,0,0,.2);
    }
    & + main {
      margin-top: .6rem;
    }
    .logo {
      width: .6rem;
      height: 100%;
      float: left;
      padding-bottom: .06rem;
      text-align: center;
      cursor: pointer;
      perspective: 1000px;
      img {
        max-width: 100%;
        max-height: 100%;
        pointer-events: none;
        animation: rotateY 6.4s ease-in-out infinite;
        transform-style: preserve-3d;
      }
    }
    .time {
      display: block;
      float: right;
      font-size: 12px;
      line-height: 16px;
      margin: 5px 1em;
      .lt::before { content: "本"; }
      .et::before { content: "艾"; }
      > span::before {
        display: inline-block;
        width: 16px;
        height: 16px;
        margin-right: 3px;
        margin-left: 5px;
        line-height: 16px;
        text-align: center;
        border-radius: 2px;
        background: #aaa;
        color: #fff;
      }
    }
    .title {
      display: block;
      height: 100%;
      font-size: 20px;
      padding: 0 .5em;
      line-height: 3;
      float: left;
      color: #b71c1c;
    }
    nav {
      float: right;
      height: 100%;
      a {
        display: inline-block;
        height: 100%;
        padding: .5em;
        line-height: 2;
        color: #444;
        border-width: 0;
        transition: color .2s linear, border .1s linear;
      }
      .router-link-active {
        color: #0b8;
        border-bottom: 2px solid #0da;
      }
    }
  }
</style>

<template>
  <header>
    <div class="logo" @click="$router.push('/')">
      <img src="../assets/ab_story_bg.png" alt="">
    </div>
    <span class="time">
      <span class="lt" v-text="lt">00:00</span>
      <span class="et" v-text="et">00:00</span>
    </span>
    <span class="title"></span>
    <nav>
      <router-link :to="{name: 'ffiv'}">FFIV</router-link>
      <router-link :to="{name: 'component'}">Component</router-link>
      <router-link :to="{name: 'canvas'}">Canvas</router-link>
      <router-link :to="{name: 'code'}">Code</router-link>
      <router-link :to="{name: 'entry'}">Entry</router-link>
    </nav>
  </header>
</template>

<script>
  function add0(n) {
    return n < 10 ? '0' + n : n;
  }

  export default {
    data() {
      return {
        timeout: null,
        lt: '00:00',
        et: '00:00'
      };
    },
    mounted() {
      this.timeout = setInterval(() => {
        var n = new Date();
        this.lt = `${add0(n.getHours())}:${add0(n.getMinutes())}`;
        this.et = `${add0(n % 4200000 / 175000 >> 0)}:${add0(n % 175000 / 175000 * 60 >> 0)}`;
      }, 500);
    },
    beforeDestroy() {
      clearInterval(this.timeout);
    }
  }
</script>