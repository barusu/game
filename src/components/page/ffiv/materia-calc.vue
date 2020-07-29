<template>
  <div class="materia-calc">
    <div class="main">
      <p><o-button type="info" @click="save">保存</o-button> <span v-text="Quality1"></span> / 2421 - <span v-text="Quality2"></span> / 2345 - <span v-text="Quality3"></span> / 836 </p>
      <div class="line" v-for="i in temp" :key="i.data">
        <span class="name" v-text="i.name"></span>
        <o-select v-model="data[i.data][index]" :skin="i.materia[index] ? 'base error' : 'base'" :data="mdata[i.materia[index]]" placeholder="" v-for="(m, index) in i.materia" :key="index"></o-select>
        <span> <span v-text="i.qualityEx1"></span> - <span v-text="i.qualityEx2"></span> - <span v-text="i.qualityEx3"></span> </span>
      </div>
    </div>
    <span @click="log">test</span>
  </div>
</template>

<script>
const c460 = [
  {
    name: '主',
    data: 0,
    quality1: 714,
    quality2: 408,
    quality3: 0,
    qualityEx1: 126,
    qualityEx2: 72,
    qualityEx3: 8,
    materia: [0, 1, 2, 3, 4]
  }, {
    name: '副',
    data: 1,
    quality1: 408,
    quality2: 714,
    quality3: 0,
    qualityEx1: 72,
    qualityEx2: 126,
    qualityEx3: 8,
    materia: [0, 1, 2, 3, 4]
  }, {
    name: '头',
    data: 2,
    quality1: 0,
    quality2: 408,
    quality3: 0,
    qualityEx1: 24,
    qualityEx2: 72,
    qualityEx3: 8,
    materia: [0, 0, 1, 2, 3]
  }, {
    name: '身',
    data: 3,
    quality1: 408,
    quality2: 204,
    quality3: 7,
    qualityEx1: 72,
    qualityEx2: 36,
    qualityEx3: 1,
    materia: [0, 0, 1, 2, 3]
  }, {
    name: '手',
    data: 4,
    quality1: 306,
    quality2: 0,
    quality3: 7,
    qualityEx1: 54,
    qualityEx2: 24,
    qualityEx3: 1,
    materia: [0, 0, 1, 2, 3]
  }, {
    name: '腰',
    data: 5,
    quality1: 0,
    quality2: 89,
    quality3: 0,
    qualityEx1: 21,
    qualityEx2: 15,
    qualityEx3: 7,
    materia: [0, 1, 2, 3, 4]
  }, {
    name: '腿',
    data: 6,
    quality1: 204,
    quality2: 20,
    quality3: 7,
    qualityEx1: 36,
    qualityEx2: 4,
    qualityEx3: 1,
    materia: [0, 0, 1, 2, 3]
  }, {
    name: '脚',
    data: 7,
    quality1: 20,
    quality2: 204,
    quality3: 0,
    qualityEx1: 4,
    qualityEx2: 36,
    qualityEx3: 8,
    materia: [0, 0, 1, 2, 3]
  }, {
    name: '耳',
    data: 8,
    quality1: 0,
    quality2: 0,
    quality3: 79,
    qualityEx1: 21,
    qualityEx2: 21,
    qualityEx3: 14,
    materia: [0, 1, 2, 3, 4]
  }, {
    name: '脖',
    data: 9,
    quality1: 0,
    quality2: 0,
    quality3: 79,
    qualityEx1: 21,
    qualityEx2: 21,
    qualityEx3: 14,
    materia: [0, 1, 2, 3, 4]
  }, {
    name: '手',
    data: 10,
    quality1: 0,
    quality2: 0,
    quality3: 79,
    qualityEx1: 21,
    qualityEx2: 21,
    qualityEx3: 14,
    materia: [0, 1, 2, 3, 4]
  }, {
    name: '指',
    data: 11,
    quality1: 0,
    quality2: 0,
    quality3: 41,
    qualityEx1: 21,
    qualityEx2: 21,
    qualityEx3: 7,
    materia: [0, 1, 2, 3, 4]
  }, {
    name: '指',
    data: 12,
    quality1: 0,
    quality2: 0,
    quality3: 41,
    qualityEx1: 21,
    qualityEx2: 21,
    qualityEx3: 7,
    materia: [0, 1, 2, 3, 4]
  }
];
const type = {
  g: ['名匠', '巨匠', '魔匠'],
  c: ['达识', '博识', '器识']
};
const Value = {
  g: [
    [0, 0, 0, 0, 0, 0, '作业16', '作业14', '作业21'],
    [0, 0, 0, 0, 0, 0, '加工10', '作14', '作21'],
    [0, 0, 0, 0, 0, 0, '作16', '作14', '作21']
  ],
  c: [
    ['获', 3, 4, 5, 6, 10, 15, 12, 20],
    ['鉴', 3, 4, 5, 6, 10, 15, 12, 20],
    ['GP', 1, 2, 3, 4, 6, 8, 7, 9]
  ]
};

export default {
  components: {},
  data() {
    return {
      temp: c460,
      data: [
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0]
      ],
      typeNum: 3,
      type: 'c'
    };
  },
  methods: {
    log() {
      console.log(this.data);
    },
    save() {
      localStorage.materiac460 = JSON.stringify(this.data);
    }
  },
  computed: {
    mdata() {
      var tem = [], s = [], d = [];
      for(var i = 0; i < this.typeNum; i++) {
        s.push({value: Value[this.type][i][8] * 10 + i, label: `${type[this.type][i]}捌型 (${Value[this.type][i][0]}${Value[this.type][i][8]})`});
        s.push({value: Value[this.type][i][6] * 10 + i, label: `${type[this.type][i]}陆型 (${Value[this.type][i][0]}${Value[this.type][i][6]})`});
        d.push({value: Value[this.type][i][7] * 10 + i, label: `${type[this.type][i]}柒型 (${Value[this.type][i][0]}${Value[this.type][i][7]})`});
        d.push({value: Value[this.type][i][5] * 10 + i, label: `${type[this.type][i]}伍型 (${Value[this.type][i][0]}${Value[this.type][i][5]})`});
        d.push({value: Value[this.type][i][4] * 10 + i, label: `${type[this.type][i]}肆型 (${Value[this.type][i][0]}${Value[this.type][i][4]})`});
      }
      tem.push(s, s, d, d, d);
      return tem;
    },
    Quality1() {
      var t = 0;
      this.temp.forEach((i, index) => {
        t += i.quality1;
        var tt = 0;
        this.data[index].forEach(d => {
          if(d % 10 === 0) tt += d / 10 >> 0;
        });
        t += Math.min(tt, i.qualityEx1);
      });
      return t;
    },
    Quality2() {
      var t = 0;
      this.temp.forEach((i, index) => {
        t += i.quality2;
        var tt = 0;
        this.data[index].forEach(d => {
          if(d % 10 === 1) tt += d / 10 >> 0;
        });
        t += Math.min(tt, i.qualityEx2);
      });
      return t;
    },
    Quality3() {
      var t = 0;
      if(this.type == 'c') t += 400;
      this.temp.forEach((i, index) => {
        t += i.quality3;
        var tt = 0;
        this.data[index].forEach(d => {
          if(d % 10 === 2) tt += d / 10 >> 0;
        });
        t += Math.min(tt, i.qualityEx3);
      });
      return t;
    }
  },
  mounted() {
    if(localStorage.materiac460) {
      this.data = JSON.parse(localStorage.materiac460);
    }
  }
}
</script>

<style lang="scss">
.materia-calc {
  font-size: 12px;
  width: 90%;
  margin: 0 auto;
  .o-select {
    width: 10em;
    margin-right: .5em;
    &.error .select {
      border-color: #fc605d;
    }
  }
  p {
    text-align: left;
    margin-bottom: .5em;
  }
  .line {
    text-align: left;
    margin-bottom: 5px;
  }
  .name {
    display: inline-block;
    width: 2em;
    text-align: center;
  }
}
</style>