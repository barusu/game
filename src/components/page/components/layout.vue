<template>
  <div class="oo-layout" @mousedown="showMenu" :class="{'nopd': !config.borderType}">
    <div class="operation" v-if="isEdit">
      <a href="javascript:;" @click="save">保存</a>
    </div>
    <div class="content" ref="content" :style="wrapperStyle" :class="{'nopd': !config.listType}">
      <div class="card" v-for="(i, index) in list" :style="i.style">
        <div class="card-body">
          <div class="card-content" :is="i.component" :option="i" ref="item"></div>
          <div class="card-opration" @mousedown.self="move(i)" :class="{'murderer': i.id == murdererID, 'confirm': i.id == waitDelID}" v-if="isEdit">
            <div class="se" @mousedown.self="resize(i)"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import $ from '@/libs/ajax';

  var ucw,          // 矩阵单点宽度
    uch,            // 矩阵单点高度
    past = null,    // 储存鼠标按下时的信息
    target = null,  // 当前操作的元素
    matrix,         // 矩阵（二维数组）
    that,           // vue对象的引用
    victim = [],    // 由于此次操作造成位置变化的元素列表
    victimFlag;     // 善后的递归操作是否正在进行中

  /* eslint-disable no-constant-condition */
  /* eslint-disable no-unused-vars */
  /* eslint-disable no-use-before-define */
  // 根据宽高寻找可放置的空位坐标
  function getPointInMatrix(w, h) {
    var poi = 0, x, y, t;
    while(true) {
      if(matrix[poi]) {
        x = 0;
        if(matrix[poi].some((n, i) => {
          if(n === 0) x++;
          else x = 0;
          if(x >= w) {
            y = 1;
            outer:
            while(true) {
              if(y >= h) {
                x = ++i;
                return true;
              }
              if(matrix[poi + y]) {
                for(t = 0; t < w; t++) {
                  if(matrix[poi + y][i - t] !== 0) break outer;
                }
                y++;
              }else {
                matrix.push(Array(12).fill(0));
                y++;
              }
            }
          }
          return false;
        })) break;
        poi++;
      }else matrix.push(Array(12).fill(0));
    }
    return {x: x - w, y: poi};
  }
  // 添加卡片到矩阵中
  function addBoxToMatrix(p) {
    var w, h;
    for(w = 0; w < p.w; w++) {
      for(h = 0; h < p.h; h++) {
        matrix[p.y + h][p.x + w] = p.id;
      }
    }
    that.lines = matrix.length;
  }
  // 从矩阵中移除卡片
  function removeBoxMatrix(id) {
    matrix.forEach(i => {
      i.forEach((j, index) => {
        if(j === id) i[index] = 0;
      });
    });
  }
  // 插入卡片到矩阵中
  function insertBoxToMatrix(p) {
    var tem, w, h, victim = [];
    for(w = 0; w < p.w; w++) {
      for(h = 0; h < p.h; h++) {
        if(!matrix[p.y + h]) matrix[p.y + h] = Array(12).fill(0);
        else {
          if(matrix[p.y + h][p.x + w] !== 0) {
            tem = matrix[p.y + h][p.x + w];
            victim[tem] = victim[tem] || tem;
            removeBoxMatrix(tem);
          }
        }
      }
    }
    addBoxToMatrix(p);
    victim.forEach(i => {
      resetBox(that.getItem(i), p);
    });
  }
  function checkMatrix(x1, x2, y1, y2, id) {
    if(typeof x1 !== typeof 0 || typeof x2 !== typeof 0 || typeof y1 !== typeof 0 || typeof y2 !== typeof 0) return false;
    var i;
    id = id || 0;
    if(x1 < 0 || x2 > 12 || y1 < 0) return false;
    var rs = {x: x1, y: y1};
    for(;y1 < y2; y1++) {
      if(!matrix[y1]) matrix[y1] = Array(12).fill(0);
      else {
        for(i = x1; i < x2; i++) {
          if(matrix[y1][i]) {
            rs = null;
            if(matrix[y1][i] !== id) return false;
          }
        }
      }
    }
    return rs;
  }
  // 重新排列卡片
  function resetBox(card, murderer) {
    var list = [], time = 4, distance = 0, direction = [true, true, true, true, true], point;
    victim[card.id] = card;
    while(time) {
      distance++;
      if(direction[4]) {
        point = checkMatrix(card.ox, card.ox + card.w, card.oy, card.oy + card.h, murderer.id);
        if(point === false) {
          direction[4] = false;
        }
        if(point) break;
      }
      if(direction[0]) {
        point = checkMatrix(card.ox, card.ox + card.w, card.oy - distance, card.oy + card.h - distance, murderer.id);
        if(point === false) {
          direction[0] = false;
          time--;
        }
        if(point) break;
      }
      if(direction[1]) {
        point = checkMatrix(card.ox - distance, card.ox + card.w - distance, card.oy, card.oy + card.h, murderer.id);
        if(point === false) {
          direction[1] = false;
          time--;
        }
        if(point) break;
      }
      if(direction[2]) {
        point = checkMatrix(card.ox + distance, card.ox + card.w + distance, card.oy, card.oy + card.h, murderer.id);
        if(point === false) {
          direction[2] = false;
          time--;
        }
        if(point) break;
      }
      if(direction[3]) {
        point = checkMatrix(card.ox, card.ox + card.w, card.oy + distance, card.oy + card.h + distance, murderer.id);
        if(point === false) {
          direction[3] = false;
          time--;
        }
        if(point) break;
      }
    }
    if(point) {
      card.x = point.x;
      card.y = point.y;
      addBoxToMatrix(card);
      updateStyle(card);
      return true;
    }else if(murderer) {
      card.y = murderer.y + murderer.h;
      insertBoxToMatrix(card);
      updateStyle(card);
    }
    return false;
  }
  // 更新卡片位置
  function updateStyle(card) {
    if(!card.style) card.style = {};
    card.style.width = 25 * card.w / 3 + '%';
    card.style.height = uch * card.h + 'px';
    card.style.left = 25 * card.x / 3 + '%';
    card.style.top = uch * card.y + 'px';
  }
  // 检查善后情况（防止dealVictim执行次数过多）
  function inspectVictim() {
    if(victimFlag) {
      setTimeout(inspectVictim, 40);
    }else dealVictim();
  }
  // 善后工作 （移动过的元素尝试向其原坐标移动）
  function dealVictim() {
    var flag = false;
    victim.forEach(i => {
      if(i) {
        removeBoxMatrix(i.id);
        if(resetBox(i, false)) {
          if(i.ox === i.x && i.oy === i.y) {
            victim[i.id] = null;
          }
          flag = true;
        }else {
          addBoxToMatrix(i);
        }
      }
    });
    if(flag) {
      setTimeout(dealVictim, 40);
      victimFlag = true;
    }else {
      victimFlag = false;
    }
  }
  // 删除空白行
  function deleteBlankLine() {
    var waitForDeathList = [], poi = 0, flag, mark = [], card;
    matrix.forEach((i, index) => {
      flag = true;
      i.forEach(item => {
        if(item !== 0) {
          flag = false;
          if(!mark[item]) {
            mark[item] = true;
            if(poi) {
              card = that.getItem(item);
              card.oy = card.y = card.y - poi;
              updateStyle(card);
            }
          }
        }
      });
      if(flag) {
        poi++;
        waitForDeathList.push(index);
      }
    });
    while(waitForDeathList.length) {
      matrix.splice(waitForDeathList.pop(), 1);
    }
  }
  // 鼠标移动事件
  function move(event){
    if(target) {
      var e = event ? event: window.event;
      if(past.type) { // 区分移动与缩放
        target.style.left = ucw * target.ox + e.clientX - past.x + 'px';
        target.style.top = uch * target.oy + e.clientY - past.y + 'px';
        target.x = target.ox + Math.round((e.clientX - past.x) / ucw);
        target.y = target.oy + Math.round((e.clientY - past.y) / uch);
        if(target.x < 0) target.x = 0;
        if(target.y < 0) target.y = 0;
        if(target.x + target.w > 12) target.x = 12 - target.w;
      }else {
        target.style.width = ucw * past.w + e.clientX - past.x + 'px';
        target.style.height = uch * past.h + e.clientY - past.y + 'px';
        if((ucw * past.w + e.clientX - past.x) < ucw * target.minx) target.style.width = ucw * target.minx + 'px';
        if((uch * past.h + e.clientY - past.y) < uch * target.miny) target.style.height = uch * target.miny + 'px';
        target.w = past.w + Math.round((e.clientX - past.x) / ucw);
        target.h = past.h + Math.round((e.clientY - past.y) / uch);
        if(target.x + target.w > 12) target.w = 12 - target.x;
        if(target.w < target.minx) target.w = target.minx;
        if(target.h < target.miny) target.h = target.miny;
      }
      removeBoxMatrix(target.id);
      insertBoxToMatrix(target);
      inspectVictim();
    }
  }
  // 单次操作结束事件
  function over() {
    if(target) {
      target.ox = target.x;
      target.oy = target.y;
      victim.forEach(i => {
        if(i) {
          i.ox = i.x;
          i.oy = i.y;
        }
      });
      victim = [];
      updateStyle(target);
      target = null;
      deleteBlankLine();
      that.lines = matrix.length;
      that.murdererID = 0;
    }
    window.removeEventListener('mousemove', move);
    window.removeEventListener('mouseup', over);
  }

  export default {
    components: {},
    props: ['id', 'items', 'host', 'isEdit', 'config'],
    data() {
      return {
        list: [],
        lines: 0,
        murdererID: 0,
        isShowMenu: false,
        menuPoint: {x: 0, y: 0},
        configID: 0,
        uch: 0,
        waitDelID: 0
      };
    },
    watch: {
      'config': {
        deep: true,
        handler: function() {
          this.$nextTick(() => {
            ucw = Math.floor(this.$refs.content.clientWidth / 12);
            this.uch = uch = Math.floor(ucw * 0.618);
            this.list.forEach(i => {
              updateStyle(i);
            });
            this.$nextTick(() => {
              this.$forceUpdate();
              this.$refs.item.forEach(i => {
                if(i.update) {
                  setTimeout(() => {
                    i.update();
                  }, 1);
                }
              });
            });
          });
        }
      }
    },
    methods: {
      save() {
        this.$emit('saved');
        this.list.forEach(i => {
          this.updateItem(i);
        });
      },
      loadDate() {
        if(!this.id) return;
        $.get(`${this.host}layout/getAll/${this.id}`, rs => {
          var option, poi, index = 0;
          if(Array.isArray(rs)) {
            this.backup = {};
            rs.forEach(i => {
              this.backup[i.id] = i.json && i.json != 'undefined' && JSON.parse(i.json) || {};
              option = i.json && i.json != 'undefined' && JSON.parse(i.json) || {};
              option.id = i.id;
              option.minx = option.minx || 2;
              option.miny = option.miny || 2;
              option.w = option.w || 2;
              option.h = option.h || 2;
              if(index < this.items.length) {
                option.component = this.items[index];
                index++;
                if(!(option.x || option.x === 0) || !(option.y || option.y === 0)) {
                  poi = getPointInMatrix(option.w, option.h);
                  option.x = option.ox = poi.x;
                  option.y = option.oy = poi.y;
                  this.updateItem(option);
                }else {
                  if(!checkMatrix(option.x, option.x + option.w, option.y, option.y + option.h)) {
                    poi = getPointInMatrix(option.w, option.h);
                    option.x = option.ox = poi.x;
                    option.y = option.oy = poi.y;
                    this.updateItem(option);
                  }else {
                    option.ox = option.x;
                    option.oy = option.y;
                  }
                }
                addBoxToMatrix(option);
                updateStyle(option);
                this.list.push(option);
              }else {
                $.delete(`${this.host}layout/delete/${i.id}`, rs => {
                  if(rs && rs.status) {
                    removeBoxMatrix(i.id);
                    this.list.splice(index, 1);
                    deleteBlankLine();
                    this.waitDelID = 0;
                  }else {
                    this.$emit('msg', rs.info || '删除失败');
                  }
                });
              }
            });
            while(index < this.items.length) {
              option = {component: this.items[index], w: 2, h: 2};
              poi = getPointInMatrix(option.w, option.h);
              option.x = option.ox = poi.x;
              option.y = option.oy = poi.y;
              index++;
              $.post(`${this.host}layout/save`, {
                type: this.id,
                name: 'vueVersion',
                json: JSON.stringify(option)
              }, data => {
                if(!data.status) {
                  this.$emit('msg', data.info || '保存失败');
                  console.log(data);
                }
              });
            }
          }else if(rs === 401) {
            this.$emit('msg', '没有权限访问');
            // this.$router.replace({name: 'login'});
          }
        });
      },
      updateItem(i) {
        var tem = this.backup[i.id];
        if(tem.x !== i.x || tem.y !== i.y || tem.w !== i.w || tem.h !== i.h) {
          tem.x = i.x;
          tem.y = i.y;
          tem.w = i.w;
          tem.h = i.h;
          $.post(`${this.host}layout/update`, {
            type: this.id,
            id: i.id,
            name: 'vueVersion',
            json: JSON.stringify(tem)
          }, data => {
            if(!(data && data.status)) {
              this.$emit('msg', data.info || '更新失败');
            }
          });
        }
      },
      showMenu() {
        if(event.button === 2) {
          this.isShowMenu = true;
          this.menuPoint.x = event.clientX;
          this.menuPoint.y = event.clientY;
        }else {
          this.isShowMenu = false;
        }
      },
      move(i) {
        over();
        if(event.button === 2) return;
        this.murdererID = i.id;
        target = i;
        past = {x: event.clientX, y: event.clientY, type: true};
        window.addEventListener('mousemove', move);
        window.addEventListener('mouseup', over);
      },
      resize(i) {
        over();
        if(event.button === 2) return;
        this.murdererID = i.id;
        target = i;
        past = {x: event.clientX, y: event.clientY, w: i.w, h: i.h, type: false};
        window.addEventListener('mousemove', move);
        window.addEventListener('mouseup', over);
      },
      getItem(id) {
        for(var i = 0; i < this.list.length; i++) {
          if(this.list[i].id === id) return this.list[i];
        }
      }
    },
    computed: {
      wrapperStyle() {
        return {
          height: this.lines * this.uch + 'px',
          marginBottom: (this.isEdit ? '100px' : null)
        };
      },
      menuStyle() {
        return {
          top: this.menuPoint.y + 'px',
          left: this.menuPoint.x + 'px'
        }
      }
    },
    mounted() {
      this.$nextTick(() => {
        this.loadDate();
        ucw = Math.floor(this.$refs.content.clientWidth / 12);
        this.uch = uch = Math.floor(ucw * 0.618);
      });
      matrix = [];
      that = this;
    },
    beforeDestroy() {
      matrix = null;
      over();
    }
  };
</script>

<style lang="scss">
  .oo-layout {
    padding: .2rem 5% .1rem;
    &.nopd {
      padding: 0;
    }
    .menu {
      position: fixed;
      font-size: 16px;
      line-height: 2;
      padding: 0 1em;
      border-radius: 4px;
      background: #fff;
      box-shadow: 1px 1px 6px rgba(0,0,0,.1);
      cursor: pointer;
    }
    .operation {
      font-size: 12px;
      text-align: right;
      border-bottom: 1px solid #ccc;
      margin-bottom: .1rem;
      > * {
        margin: 4px .1rem 4px 0;
      }
      a {
        line-height: 1.5;
        border-radius: 4px;
        padding: 1px .5em;
        vertical-align: middle;
        display: inline-block;
        background: #409eff;
        color: #fff;
        &.red {
          background: #f56c6c;
        }
        &.forgive {
          background: #67c23a;
        }
      }
    }
    .content {
      position: relative;
      padding: 0;
      border: none;
      &.nopd .card{
        padding: 0;
      }
    }
    .card {
      position: absolute;
      padding: 5px;
      transition: all .04s;
    }
    .card-body {
      position: relative;
      width: 100%;
      height: 100%;
      &:hover {
        box-shadow: 1px 1px 6px rgba(0,0,0,.1);
      }
    }
    .card-opration {
      position: absolute;
      top: 0; left: 0;
      width: 100%;
      height: 100%;
      padding: 16px;
      box-shadow: 0 0 0 0 rgba(255,255,255,.1) inset;
      cursor: move;
      overflow: hidden;
      transition: all .34s;
      .to-edit {
        height: 100%;
        cursor: pointer;
      }
      .se {
        position: absolute;
        bottom: 0; right: 0;
        display: block;
        width: 16px;
        height: 16px;
        border: 3px solid transparent;
        opacity: 0;
        cursor: se-resize;
        transition: all .34s;
        &::after {
          content: '';
          display: block;
          width: 100%;
          height: 100%;
          border: 2px solid rgba(0,0,0,.35);
          border-top-color: transparent;
          border-left-color: transparent;
        }
      }
      &.murderer {
        z-index: 19950210;
      }
      &.murderer,
      &:hover {
        box-shadow: 0 0 0 .16rem rgba(255,255,255,.5) inset;
        background: rgba(255,255,255,.15);
        .del,
        .se {
          opacity: 1;
        }
      }
      &.confirm {
        .delconfirm {
          opacity: 1;
          pointer-events: all;
          transform: scale(1);
        }
      }
    }
    .card-content {
      height: 100%;
      cursor: default;
      font-size: .16rem;
      user-select: none;
      overflow: hidden;
    }
  }
</style>