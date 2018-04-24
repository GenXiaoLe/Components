<template>
  <div id="app">
    <p>{{addName}}</p>
    <CanvasCom></CanvasCom>
    <Demo :isNumber="number">
        <p slot="header">一段文字23 23333</p>
        <p>一段文字 23333</p>
        <p slot="footer">再来一段文字</p>
    </Demo>
    <input type="text" v-model="number" />
    <input type="button" value="增加" @click="addNumber">
    <div>
      <p v-if="isShow">{{message}}</p>
      <input type="button" value="显示" @click="isShowMessage" />
    </div> 
    <ul>
      <li v-for="item in array" @click="clickArray(item)" :key="item.id">{{item.name}}</li>
    </ul>
  </div>
</template>

<script>
import Demo from './components/Demo';
import CanvasCom from './components/Canvas';
export default {
  name: 'App',
  components: {
    Demo,
    CanvasCom
  },
  data() {
    return {
      number: 0,
      message: '显示一行文字',
      isShow: false,
      array: [
        {name: '第一行文字', id: 0},
        {name: '第2行文字', id: 1},
        {name: '第3行文字', id: 2},
        {name: '第4行文字', id: 3},
        {name: '第5行文字', id: 4},
        {name: '第6行文字', id: 5},
      ],
      firstName: 'first'
    }
  },
  /* created() {
    alert('created');
  },
  mounted() {
    alert('mounted');
  },
  updated() {
    alert('updated');
  }, */
  computed: {
    addName() {
      return this.firstName = this.firstName + this.number;
    }
  },
  methods: {
    addNumber() {
      this.number += 1;
    },
    isShowMessage() {
      this.isShow = !this.isShow;
    },
    clickArray(item) {
        alert(`1name${item.name}----id${item.id}`);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
