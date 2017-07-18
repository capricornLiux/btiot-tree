<template>
  <div id="app">
    <!--<img src="./assets/logo.png">-->
    <hello :model="root" :length="1"></hello>
  </div>
</template>

<script>
  import Hello from './components/Hello'

  export default {
    name: 'app',
    components: {
      Hello
    },

    data(){
      return {
        root: {
          // 节点名
          name: 'root',
          level: 0,
          isOpen:true,
          // 子节点
          children: [
            {
              name: 'class11',
              level:1,
              isOpen:true,
              children: [
                {
                  name: 'class21',
                  level:2,
                  isOpen:true,
                  children: []
                },
                {
                  name: 'class22',
                  level:2,
                  isOpen:true,
                  children: []
                }
              ]
            },
            {
              name: 'class12',
              level: 1,
              isOpen:true,
              children: []
            }
          ]
        }
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
