<template>
  <div>
    <div>取得資料 至 表格建立完成(含事件綁定) 期間，共耗費 <span class="usuage">{{ time.cost }}</span> ms</div>
    <table>
      <thead>
        <th>KEY</th>
        <th>CELL1</th>
        <th>CELL2</th>
        <th>CELL3</th>
        <th>CELL4</th>
        <th>CELL5</th>
        <th>CELL6</th>
        <th>CELL7</th>
        <th>CELL8</th>
        <th>CELL9</th>
      </thead>
      <tbody>
        <tr v-for="(item, i) in originData" :key="item.key" :class="{'selected': row == item.key}" @click="selectTr(item.key)">
          <td>
            <span class="star" :class="{'selected': star[i]===true }" @click.stop="favorite(item, i)"></span>
            {{ item.key }}
          </td>
          <td>{{ item.cell1 }}</td>
          <td>{{ item.cell2 }}</td>
          <td>{{ item.cell3 }}</td>
          <td>{{ item.cell4 }}</td>
          <td>{{ item.cell5 }}</td>
          <td>{{ item.cell6 }}</td>
          <td>{{ item.cell7 }}</td>
          <td>{{ item.cell8 }}</td>
          <td>{{ item.cell9 }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      originData: [],
      originData2: [],
      originData3: {},
      originData4: [],
      select: '',
      time: {
        create: '',
        created: '',
        cost: ''
      },
      star: [],
      status: false,
      row: ''
    }
  },
  methods: {
    getData () {
      const vm = this
      vm.originData = require('@/data/data1.json')// 讀取本地端資料
      vm.originData2 = require('@/data/data2.json')// 讀取本地端資料
      vm.originData3 = require('@/data/data3.json')// 讀取本地端資料
      vm.originData2.forEach((element) => {
        let str = element.key.split('R').join('')
        if (element.key === vm.originData[str].key) {
          vm.originData[str].cell8 = element.cell8
          // vm.originData[str].star = false
          // 將key值轉成索引 依索引填入cell8
        } else {
          // console.log('Data Error')
          return false
        }
      })
      const array = Object.keys(vm.originData3)
      array.forEach((element) => {
        vm.originData4.push({ cell4: vm.originData3[element].cell4, cell9: vm.originData3[element].cell9 })
        // console.log(element, vm.originData3[element].cell4, vm.originData3[element].cell9)
      })// 取出物件索引存入新陣列
      vm.originData4.forEach((element, index) => {
        let str1 = element.cell4.split('R').join('')
        let str2 = str1.split('C4').join('')
        if (element.cell4 === vm.originData[str2].cell4) {
          vm.originData[str2].cell9 = element.cell9
        } else {
          // console.log('Data Error')
          return false
        }
      })// 將cell4值轉成索引 依索引填入cell8
      // console.log('1', this.originData, '4', this.originData4)
    },
    selectTr (value) {
      this.row = value
    },
    favorite (value, index) {
      if (!this.star[index] || this.star[index] === '') {
        this.$set(this.star, index, true)
      } else if (this.star[index] === true) {
        this.$set(this.star, index, false)
      }
    }
  },
  created () {
    const vm = this
    vm.getData()
    vm.time.create = new Date()// 紀錄開始取資料當下時間
    // console.log('create', this.time.create)
  },
  mounted () {
    this.time.created = new Date()// 紀錄DOM建立完成之時間
    // console.log('created', this.time.created)
    this.time.cost = this.time.created - this.time.create// 資料讀取至DOM建立所花費之時間
    // console.log(this.time.cost)
  }
}
</script>
