<template>
  <div class="gatheringtimer">
    <textarea v-model="Macro" cols="50" rows="30" readonly></textarea>
  </div>
</template>

<script>
const POINTS = [
  {job: '园', level: 802, time: 0, name: '帝王蕨菜', place: '滞潮村'},
  {job: '矿', level: 800, time: 0, name: '黑玛瑙', place: '广场'},
  {job: '矿', level: 76, time: 0, name: '闪光灵砂', place: '蛇形枝'},
  {job: '矿', level: 802, time: 2, name: '辉钴铁矿', place: '严命城'},
  {job: '园', level: 801, time: 2, name: '檀香木', place: '蛇形枝'},
  {job: '园', level: 802, time: 4, name: '紫草', place: '鼹灵'},
  {job: '矿', level: 801, time: 4, name: '轻铍矿', place: '群花馆'},
  {job: '园', level: 76, time: 4, name: '黑暗灵砂', place: '蛇形枝'},
  {job: '矿', level: 802, time: 6, name: '尊紫碎块', place: '广场'},
  {job: '园', level: 800, time: 6, name: '小柠檬2', place: '工匠村'},
  {},
  {job: '园', level: 802, time: 8, name: '印茄原木', place: '群花馆'},
  {job: '园', level: 801, time: 8, name: '泡茧', place: '乔布要塞'},
  {job: '矿', level: 76, time: 8, name: '黑暗灵砂', place: '乔布要塞'},
  {job: '矿', level: 802, time: 10, name: '梣树明矾', place: '鼹灵'},
  {job: '矿', level: 801, time: 10, name: '重钨矿', place: '鳍人潮池'},
  {job: '园', level: 802, time: 12, name: '帝王蕨菜', place: '滞潮村'},
  {job: '矿', level: 800, time: 12, name: '黑玛瑙', place: '广场'},
  {job: '园', level: 80, time: 12, name: '古树灵砂', place: '水晶都'},
  {job: '矿', level: 802, time: 14, name: '辉钴铁矿', place: '严命城'},
  {job: '园', level: 801, time: 14, name: '檀香木3', place: '蛇形枝'},
  {},
  {job: '园', level: 802, time: 16, name: '紫草', place: '上路客店'},
  {job: '矿', level: 801, time: 16, name: '轻铍矿', place: '群花馆'},
  {job: '矿', level: 80, time: 16, name: '险山灵砂', place: '图姆拉'},
  {job: '矿', level: 802, time: 18, name: '尊紫碎块', place: '广场'},
  {job: '园', level: 800, time: 18, name: '小柠檬', place: '工匠村'},
  {job: '园', level: 802, time: 20, name: '印茄原木', place: '群花馆'},
  {job: '园', level: 801, time: 20, name: '泡茧', place: '乔布要塞'},
  {job: '园', level: 76, time: 20, name: '闪光灵砂', place: '蛇形枝'},
  {job: '矿', level: 802, time: 22, name: '梣树明矾', place: '鼹灵'},
  {job: '矿', level: 801, time: 22, name: '重钨1', place: '鳍人潮池'}
];

export default {
  components: {},
  data() {
    return {
      points: POINTS
    };
  },
  methods: {},
  computed: {
    Macro() {
      var t;
      return this.points.map(i => {
        if(!i.job) return '';
        t = i.time < 10 ? '0' + i.time : i.time;
        if(i.level === 802) t += '00';
        else if(i.level > 80) t += '20';
        else t += '40';
        return `/alarm ${i.job}【${i.place}】${i.name} et ${t} 0`;
      }).join('\n');
    }
  }
}
</script>

<style lang="scss">
.gatheringtimer {
  padding: 1em 2em;
  textarea {
    padding: .5em 1em;
    width: 100%;
    height: 50em;
    line-height: 1.5;
    font-size: 12px;
    resize: none;
  }
}
</style>