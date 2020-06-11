<template>
  <div>
    hello vue !!
    <input type="text" ref="mytext" />
    <button @click="handleAdd()">add</button>

    <ul>
      <li v-for="data in datalist" :key="data">
          {{data}}
      </li>
    </ul>

    <navbar>
      <button @click="isShow=!isShow">navbar-click</button>
    </navbar>
    <sidebar v-show="isShow"></sidebar>
    <input type="text" v-model="childrens.name" />
    <input type="text" v-model="lastName" />
    <button @click="gen">Click</button><br />
    <button @click="arraySet">arrayClick</button><br />
    <button @click="promiseAll">promiseClick</button><br />
    <button @click="objAssign">objectAssign</button><br /><br />
    <button @click="equalsArrayObj">equalsArrayObj</button><br /><br />
    <button @click="abc">abc</button><br /><br />
    <button @click="equalAarry">equalAarry</button><br /><br />
    <button @click="objInArr">objInArr</button><br /><br />
  </div>
</template>

<script>
import navbar from './views/Navbar'
import sidebar from './views/Sidebar'
import axios from 'axios'
export default {
  components: {
    navbar,
    sidebar
  },
  data () {
    return {
      datalist: [],
      isShow: false,
      vendorCode: '',
      vendorId: '',
      code: '',
      childrens: {
        name: '小青',
        age: '28',
        sex: 'men'
      },
      tdArray: ['1', '2'],
      lastName: 'xiaoqing',
      firstName: ''
    }
  },
  mounted () {
    axios.get('/ajax/movieOnInfoList?token=').then(res => {
      console.log(res.data)
    })
  },
  methods: {
    handleAdd () {
      this.datalist.push(this.$refs.mytext.value)
    },
    change () {
      return new Promise((resolve, reject) => {
        resolve('sucessfull')
      })
    },
    async gen () {
      await this.change().then(res => {
        console.log('res', res)
        this.vendorCode = new Date().getTime()
      })
      console.log('vendorCode', this.vendorCode)
    },
    arraySet () {
      const arr1 = [
        { vendorId: '25566', supplierNumber: '021882', supplierName: '鼎石馆' },
        { vendorId: '25566', supplierNumber: '021882', supplierName: '维嘉国际' }
      ]
      const data = [
        { vendorId: '25566', supplierNumber: '021882', vendorName: '鼎石馆', supplierSite: '深圳', vendorSiteId: '03559', name: 'xiaoqing1' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '维嘉国际', supplierSite: '深圳', vendorSiteId: '05699', name: 'xiaoqing2' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '维嘉国际', supplierSite: '广州', vendorSiteId: '06667', name: 'xiaoqing3' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '鼎石馆', supplierSite: '东莞', vendorSiteId: '08974', name: 'xiaoqing4' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '鼎石馆', supplierSite: '北京', vendorSiteId: '06336', name: 'xiaoqing5' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '鼎石馆', supplierSite: '长沙', vendorSiteId: '04188', name: 'xiaoqing6' }
      ]
      const arraydata = []
      for (let i = 0; i < arr1.length; i++) {
        for (let j = 0; j < data.length; j++) {
          console.log('vendorId', arr1[i].vendorId)
          // debugger
          if (arr1[i].vendorId === data[j].vendorId && arr1[i].supplierNumber === data[j].supplierNumber && arr1[i].supplierName === data[j].vendorName) {
            // debugger
            // 方法1
            Object.assign(arr1[i], data[j])
            const _newObj = JSON.stringify(arr1[i])
            const newObj = JSON.parse(_newObj)
            console.log('newObj', newObj)
            arraydata.push(newObj)
            // 方法2
            /* const newObj = Object.assign({}, arr1[i], data[j])
            console.log('newObj', newObj)
            arraydata.push(newObj) */

            // 方法3
            /* arraydata.push(
              {
                vendorId: arr1[i].vendorId,
                supplierNumber: arr1[i].supplierNumber,
                supplierName: arr1[i].supplierName,
                supplierSite: data[j].supplierSite,
                vendorSiteId: data[j].vendorSiteId
              }
            ) */
          }
        }
      }
      console.log('arraydata', arraydata)
      /* Array(6)[
        {vendorId: "25566", supplierNumber: "021882", supplierName: "鼎石馆", supplierSite: "深圳", vendorSiteId: "03559"},
        {vendorId: "25566", supplierNumber: "021882", supplierName: "鼎石馆", supplierSite: "东莞", vendorSiteId: "08974},
        {vendorId: "25566", supplierNumber: "021882", supplierName: "鼎石馆", supplierSite: "北京", vendorSiteId: "06336"}
      ] */
    },
    promiseAll () {
      const p1 = new Promise((resolve, reject) => {
        const res1 = [{ firstName: 'li', lastName: 'xiaoqing', sex: 'man' }]
        resolve(res1)
        console.log('res1', res1)
      })/* .then(res => {
        console.log('res', res)
        resolve(res)
      }) */
      const p2 = new Promise((resolve, reject) => {
        const res2 = [{ firstName: 'liu', lastName: 'dehua', sex: 'man' }]
        resolve(res2)
        console.log('res2', res2)
      })/* .then(res => {
        console.log('res', res)
        resolve(res)
      }) */
      // debugger
      Promise.all([p1, p2]).then(res => {
        console.log(res)
      }).catch(error => {
        console.log(error)
      })
    },
    objAssign () {
      const o1 = { a: 1 }
      const o2 = { b: 2, c: 3 }
      const obj = Object.assign(o1, o2)
      console.log('obj', obj)
      console.log('o1', o1)
      console.log('o2', o2)
    },
    isObject (obj) {
      return Object.prototype.toString.call(obj) === '[object Object]'
    },
    isArray (arr) {
      return Object.prototype.toString.call(arr) === '[object Array]'
    },
    equalsObj (oldData, newData) {
      // 类型为基本类型时,如果相同,则返回true
      if (oldData === newData) return true
      if (this.isObject(oldData) && this.isObject(newData) && Object.keys(oldData).length === Object.keys(newData).length) {
          // 类型为对象并且元素个数相同
          // 遍历所有对象中所有属性,判断元素是否相同
          for (const key in oldData) {
              if (Object.prototype.hasOwnProperty.call(oldData, key)) {
                if (!this.equalsObj(oldData[key], newData[key])) {
                  // 对象中具有不相同属性 返回false
                  return false
                }
              }
          }
      } else if (this.isArray(oldData) && this.isArray(oldData) && oldData.length === newData.length) {
          // 类型为数组并且数组长度相同
          for (let i = 0, length = oldData.length; i < length; i++) {
              if (!this.equalsObj(oldData[i], newData[i])) {
                // 如果数组元素中具有不相同元素,返回false
                return false
              }
          }
      } else {
          // 其它类型,均返回false
          return false
      }
      // 走到这里,说明数组或者对象中所有元素都相同,返回true
      return true
    },
    equalsArrayObj () {
      const arr1 = [
      { vendorId: '25566', supplierNumber: '021882', vendorName: '鼎石馆', supplierSite: '深圳', vendorSiteId: '03559', name: 'lishao' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '维嘉国际', supplierSite: '深圳', vendorSiteId: '05699', name: 'xiaoqing2' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '维嘉国际', supplierSite: '广州', vendorSiteId: '06667', name: 'xiaoqing3' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '鼎石馆', supplierSite: '东莞', vendorSiteId: '08974', name: 'xiaoqing4' }
      ]
      const arr2 = [
        { vendorId: '25566', supplierNumber: '021882', vendorName: '鼎石馆', supplierSite: '深圳', vendorSiteId: '03559', name: 'lishao' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '维嘉国际', supplierSite: '宝安', vendorSiteId: '05699', name: 'lixiao' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '维嘉国际', supplierSite: '广州', vendorSiteId: '06667', name: 'xiaoqing3' },
        { vendorId: '25566', supplierNumber: '021882', vendorName: '鼎石馆', supplierSite: '东莞', vendorSiteId: '08974', name: 'xiaoqing4' }
      ]
      console.log('111111111', this.equalsObj(arr1, arr2))
    },
    abc () {
      const oldRowData = [
        { supplierNumber: '5525', enbleFlag: true, buyerName: 'lixiaoqing' },
        { supplierNumber: '6890', enbleFlag: false, buyerName: 'lishaoqing' }
      ]
      const newRowData = []
      oldRowData.map(item => {
        if (item.enbleFlag === true) {
          item.enbleFlag = 'Y'
        } else if (item.enbleFlag === false) {
          item.enbleFlag = 'N'
        }
        newRowData.push(item)
      })
      console.log('newRowData', newRowData)
    },
    equalAarry () {
      const array1 = [{ Num: 'A', Name: 't1' }, { Num: 'B', Name: 't9' }]
      const array2 = [{ Num: 'A', Name: 't1' }, { Num: 'B', Name: 't2' }, { Num: 'c', Name: 't3' }]
      for (var i = 0; i < array2.length; i++) {
        for (var j = 0; j < array1.length; j++) {
          if (array2[i].Num === array1[j].Num) {
            console.log(JSON.stringify(array2[i]))
            console.log(JSON.stringify(array1[j]))
            if (JSON.stringify(array2[i]) === JSON.stringify(array1[j])) {
              console.log(11111)
              // return true
            } else {
              console.log(22222)
              // return false
            }
          }
        }
        /* if (!isExist) {
          result.push(obj)
        } */
      }
    },
    objInArr () {
      const arr1 = [
        { a: '2233', b: '2020-6-9', c: 'rou' },
        { a: '7777', b: '2020-6-9', c: 'rou' }
      ]
      const arr2 = [
      { a: '2233', b: '2020-6-9', c: 'rou' },
      { a: '3333', b: '2020-6-9', c: 'rou' },
      { a: '4444', b: '2020-6-9', c: 'rou' },
      { a: '5555', b: '2020-6-9', c: 'rou' },
      { a: '6666', b: '2020-6-9', c: 'rou' }
      ]
      debugger
      for (let i = 0; i < arr1.length; i++) {
        for (let j = 0; j < arr2.length; j++) {
          if (arr1[i].a === arr2[j].a && arr1[i].b === arr2[j].b && arr1[i].c === arr2[j].c) {
            console.log('A line has not changed')
          }
        }
      }
      /* const someResult = arr2.some((value, index, arr) => {
        // console.log(value.b)
        return arr1.some((val, index, arr) => {
          return (value.b === val.b && value.a === val.a && value.c === val.c)
        })
        // value.age <= 20
      })
      console.log('someResult', someResult) */
    }
  },
  watch: {
    childrens: {
      handler: (val, oldval) => {
        console.log('val.name', val.name)
      },
      deep: true
    },
    'childrens.name': (val, oldval) => {
      console.log('1111', val + 'aaa')
    },
    lastName: function (val, oldval) {
      this.firstName = val
      console.log('firstName', this.firstName)
    }
  }
}

</script>

<style lang="scss" scoped>

  ul{
    list-style: none;
    li{
      background-color: yellow;
    }
  }
</style>
