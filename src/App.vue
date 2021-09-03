<template>
  <div id="app">
    <h1>燃油车 5位车牌号大全</h1>
    <div class="search">
      <input class="city" type="text" v-model="city" placeholder="请填写城市如：川A">
      <table class="table">
        <tr>
          <th>第一位</th>
          <th>第二位</th>
          <th>第三位</th>
          <th>第四位</th>
          <th>第五位</th>
        </tr>
        <tr>
          <td v-for="(num, i) in rules" :key="i">
            <Radio v-model="rules[i]" :label="0">0-9</Radio>
            <Radio v-model="rules[i]" :label="1">A-Z</Radio>
            <Radio v-if="i === 0" v-model="rules[i]" :label="2">A/B</Radio>
          </td>
        </tr>
      </table>
      <button class="btn" @click="generate">生成车牌</button>
    </div>
    <div class="result">
      <div class="license" v-for="(license, i) in licenseList" :key="i">
        <License>{{license}}</License>
      </div>
    </div>
  </div>
</template>

<script>
import Radio from './components/Radio.vue'
import License from './components/License.vue'
export default {
  components: {
    Radio,
    License
  },
  data() {
    return {
      // 车牌开头
      city: '',
      // 车牌5位各自规则
      rules: [0, 0, 0, 0, 0],
      // 车牌类型，0：纯数字，1：纯字母（中国牌照字母不包含 'I'，'O'）
      type: {
        0: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'],
        1: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'J', 'K', 'L', 'M', 'N', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
        2: ['A', 'B']
      },
      licenseList: []
    }
  },
  methods: {
    generate() {
      if (this.city) {
        const rulesForNum = {
          0: null,
          1: null,
          2: null,
          3: null,
          4: null
        }
        this.rules.map((item, idx) => {
          rulesForNum[idx] = this.type[item]
        })

        const result = []
        for (const n0 of rulesForNum[0]) {
          const finalLicense = new Array(5)
          finalLicense[0] = (n0)
          for (const n1 of rulesForNum[1]) {
            finalLicense[1] = (n1)
            for (const n2 of rulesForNum[2]) {
              finalLicense[2] = (n2)
              for (const n3 of rulesForNum[3]) {
                finalLicense[3] = (n3)
                for (const n4 of rulesForNum[4]) {
                  finalLicense[4] = (n4)
                  result.push(`${this.city}·${finalLicense.join('')}`)
                }
              }
            }
          }
        }

        this.licenseList = result
        console.log('result', result.length, result)
      } else {
        alert('请填写城市前缀')
      }
    }
  }
}
</script>
<style lang="scss">
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  &:focus {
    outline: none;
  }
}
#app {
    padding: 0 50px;
  h1 {
    padding: 0 0 20px 0;
    text-align: center;
  }
  .search {
    display: flex;
    justify-content: center;
    .city {
      width: 140px;
      font-size: 30px;
      text-align: center;
      border-right: none;
      border-width: 1px;
      &::-webkit-input-placeholder{
       font-size: 14px;
      }
    }
    .table {
      margin: 0 20px 0 0;
      th, td {
        width: 100px;
        border: 1px solid #333;
      }
      td {
        height: 60px;
        text-align: center;
      }
      border-collapse: collapse;
    }
    .btn {
      padding: 0 10px;
    }
  }
  .result {
    display: flex;
    flex-wrap: wrap;
    padding: 50px 0 0 0;
    .license {
      padding: 10px;
    }
  }
}
</style>
