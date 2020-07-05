<template>
  <div id="app">
    <Mylist v-bind:title="message" v-on:result-event="appAction" />
    <hr>
    <div><table v-html="log"></table></div>
  </div>
</template>

<script>
import Mylist from './components/Mylist.vue'
//↓main.jsのnew Vue部分
export default {
  name: 'app',
  components: {
    //Mylistを呼び出し
    Mylist
  },
  data:function() {
    return {
      message: 'メモを入力してください',
      result: [],
    }
  },
  computed: {
    log:function() {
      var table = '<tr><th class="head">my list</th></tr>'
      for(var i in this.result) {
        /*
        table += '<tr><td>' + this.result[i][0] + '</td><th>' + 
        this.result[i][1] + '</th></tr>'
        */
        table += '<tr><td>' + this.result[i][0] + '</td></tr>'
      }
      return table
    }
  },
  created:function() {
    var items = localStorage.getItem('log')
    var logs = JSON.parse(items)
    if(logs != null) {
      this.result = logs
    }
  },
  methods: {
    appAction:function(exp, res) {
    //配列の先頭に追加
      this.result.unshift([exp, res])
      if(this.result.length > 5) {
        //配列の末尾を削除
        this.result.pop()
      }
      var log = JSON.stringify(this.result)
      //ブラウザ上のローカルストレージに保存
      localStorage.setItem('log', log)
    }
  }
}
//↑main.jsのnew Vue部分
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 5px;
}
tr th.head {
  background-color: black;
  color: white;
}
table {
  border: 1px solid black;
  margin-left: auto;
  margin-right: auto;
}
</style>