<template>
  <main class="component">
    <div class="sidebar">
      <div class="oo-menu">
        <p class="oo-menu-title"> <router-link :to="{name: 'component'}">Component</router-link> </p>
        <input type="checkbox" id="ck_general" class="oo-ck">
        <p class="oo-menu-group">
          <label for="ck_general" class="oo-menu-group-title">
            <div>General</div>
            <span class="oo-arrow"></span>
          </label>
          <ul class="oo-menu-item-list">
            <li class="oo-menu-item"> <router-link :to="{name: 'color'}">Color</router-link> </li>
            <li class="oo-menu-item"> <router-link :to="{name: 'text'}">Text</router-link> </li>
            <li class="oo-menu-item"> <router-link :to="{name: 'button'}">Button</router-link> </li>
            <li class="oo-menu-item"> <router-link :to="{name: 'list'}">List</router-link> </li>
          </ul>
        </p>
        <input type="checkbox" id="ck_feedback" class="oo-ck">
        <p class="oo-menu-group">
          <label for="ck_feedback" class="oo-menu-group-title">
            <div>Feedback</div>
            <span class="oo-arrow"></span>
          </label>
          <ul class="oo-menu-item-list">
            <li class="oo-menu-item"> <router-link :to="{name: 'message'}">Message</router-link> </li>
          </ul>
        </p>
        <input type="checkbox" id="ck_form" class="oo-ck">
        <p class="oo-menu-group">
          <label for="ck_form" class="oo-menu-group-title">
            <div>Data Entry</div>
            <span class="oo-arrow"></span>
          </label>
          <ul class="oo-menu-item-list">
            <li class="oo-menu-item"> <router-link :to="{name: 'input'}">Input</router-link> </li>
            <li class="oo-menu-item"> <router-link :to="{name: 'select'}">Select</router-link> </li>
            <li class="oo-menu-item"> <router-link :to="{name: 'checkbox'}">Checkbox</router-link> </li>
            <li class="oo-menu-item"> <router-link :to="{name: 'upload'}">Upload</router-link> </li>
          </ul>
        </p>
        <input type="checkbox" id="ck_special" class="oo-ck">
        <p class="oo-menu-group">
          <label for="ck_special" class="oo-menu-group-title">
            <div>Special</div>
            <span class="oo-arrow"></span>
          </label>
          <ul class="oo-menu-item-list">
            <li class="oo-menu-item"> <router-link :to="{name: 'fontmask'}">Font Mask</router-link> </li>
          </ul>
        </p>
        <input type="checkbox" id="ck_project" class="oo-ck">
        <p class="oo-menu-group">
          <label for="ck_project" class="oo-menu-group-title">
            <div>Project</div>
            <span class="oo-arrow"></span>
          </label>
          <ul class="oo-menu-item-list">
            <li class="oo-menu-item"> <router-link :to="{name: 'icon'}">Icon</router-link> </li>
            <li class="oo-menu-item"> <router-link :to="{name: 'layout'}">Layout <span class="key">iframe</span></router-link> </li>
            <li class="oo-menu-item"> <router-link :to="{name: 'layout-vue'}">Layout <span class="key">.vue</span></router-link> </li>
          </ul>
        </p>
      </div>
    </div>
    <router-view class="content"></router-view>
    <div class="clear-box"></div>
    <o-footer class="footer"></o-footer>
  </main>
</template>

<script>
  import oFooter from '@/components/footer';
  export default {
    components: {
      oFooter
    },
    data () {
      return {};
    }
  }
</script>

<style lang="scss">
  $borderColor: #ddd;
  .component {
    font-size: .16rem;
    .sidebar {
      min-height: 100px;
      width: 20%;
      float: left;
      box-shadow: 1px 0 0 0 $borderColor;
    }
    .content {
      overflow: hidden;
      border-left: 1px solid $borderColor;
      text-align: left;
      padding: 1em 2em;
      h2 {
        font-size: .28rem;
        font-family: Helvetica, Arial, sans-serif;
        line-height: 2.5;
        .chinese {
          font-size: .26rem;
        }
      }
      h3 {
        font-size: .24rem;
        font-family: Helvetica, Arial, sans-serif;
        line-height: 2.5;
        margin-top: .5em;
        .chinese {
          font-size: .22rem;
        }
        span {
          font-weight: 300;
        }
      }
      h5 {
        font-size: .16rem;
        font-family: Helvetica, Arial, sans-serif;
        line-height: 2.5;
        margin-top: .5em;
      }
      p {
        line-height: 1.5;
        color: #678;
        font-size: .14rem;
        margin-bottom: .5em;
        &.tip {
          display: inline-block;
          line-height: 1;
          border-radius: 1px;
          background: #fdbc40;
          box-shadow: 0 0 0 0.5em #fdbc40;
          font-size: 12px;
          color: #fff;
        }
        .keyword {
          color: #ff3102;
        }
      }
    }
    .footer {
      border-top: 1px solid $borderColor;
    }
  }
</style>
