<template>

  <div>

    <div v-if="model.name" style="background-color: #ccc;">

      <!--使用draggable组件-->
      <draggable v-model="myArray" :options="{group:'people'}" @start="drag" @end="drop" :move="checkMove">
        <!--<div v-for="element in myArray">{{element.name}}</div>-->


        <div class="itemRow" :style="{ marginLeft:model.level*20+'px' }">
        <span v-show="model.children.length" style="color: blue;cursor: pointer;"
              @click="expandOrCollapse">{{model.isOpen ? '-' : '+'}}</span>
          <span>{{model.name}}</span>
          <span class="addNode" @click="add">添加</span>
          <span class="removeNode" @click="remove(model)">删除</span>
          <span class="updateNode" @click="edit">修改</span>
          <span class="asce" v-show="model.children.length" @click="orderAsce">↑</span>
          <span class="desc" v-show="model.children.length" @click="orderDesc">↓</span>
        </div>

      </draggable>

    </div>

    <hello v-if="model.isOpen" v-for="row in model.children" :model="row" :length="model.children.length"></hello>

  </div>

</template>

<script>

  // 导入vuedraggable插件
  import draggable from 'vuedraggable';

  export default {

    components: {
      // 声明draggable组件
      draggable
    },

    name: 'hello',


    // 使用`编辑树`组件需要传递的数据
    props: {
      // 编辑树对象
      model: {
        type: Object
      },

      length: {
        type: Number
      }
    },

    data () {
      return {
        dataModel: this.model
      }
    },

    methods: {

      // 添加节点
      add(){

        console.log(this);

        let val = prompt();
        if (val) {

          this.model.children.push({
            name: val,
            level: this.model.level + 1,
            isOpen: true,
            children: []
          });
        }

      },

      // 移除节点
      remove(model){
        var self = this;

        alert('确认删除吗?');

        if (this.$parent.model) {
          this.$parent.model.children.forEach((item, index) => {
            if (item.name == model.name) {
              self.$parent.model.children.splice(index, 1);
            }
          })
        } else {

          this.$root.root = {};

        }
      },

      // 编辑节点名称
      edit(){
        let rename = prompt('请输入修改后的节点名称');

        // 使用正则进行重命名的差错校验
        if (!rename.length) {

          alert('请输入正确的节点名称');
          return;
        }

        this.model.name = rename;
      },

      /**
       * 展开/收起功能
       */
      expandOrCollapse(){
        this.model.isOpen = !this.model.isOpen;
      },

      // 升序排列
      orderAsce(){

        function compare(property) {
          return function (a, b) {
            var value1 = a[property];
            var value2 = b[property];
            return value1 - value2;
          }
        }

        this.model.children.sort(compare('name'));

      },

      // 降序排列
      orderDesc(){
        this.orderAsce();
        this.model.children.reverse();
      },

      watch: {}
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus" rel="stylesheet/stylus">

  .addNode
    color: red
    cursor: pointer

  .removeNode
    color: green
    cursor: pointer

  .updateNode
    color: yellow
    cursor: pointer

  .asce
    color: orange
    cursor: pointer

  .desc
    color: purple
    cursor: pointer

  .test {
    position: relative
    left: 10px
  }

  @import "./mixin.styl"
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

  .itemRow {
    text-align: left;
    margin-top: 10px
    margin-bottom: 10px
    background-color: navajowhite
  }


</style>
