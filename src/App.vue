<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <!-- <TodoInput v-on:하위 컴포넌트에서 발생시킨 이벤트 이름="현재 컴포넌트의 메서드 명"></TodoInput> -->
    <TodoInput v-on:addTodoItem="addOneItem"></TodoInput>
    <!-- <TodoList v-bind: 내려보낼 프롭스 속성이름 = "현재 위치의 컴포넌트 데이터 속성"></TodoList> -->
    <TodoList  v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneComplete" v-bind:propsdata="todoItems"></TodoList>
    <TodoFooter v-on:clearAll="clearAllItems"></TodoFooter>
  </div>
</template>

<script>

import TodoHeader   from './components/TodoHeader.vue';
import TodoInput    from './components/TodoInput.vue';
import TodoList     from './components/TodoList.vue';
import TodoFooter   from './components/TodoFooter.vue';

export default {
  
  data: function () {
    return {
      todoItems: []
    }
  },

  created: function () {
    if(localStorage.length > 0){
            for(let locStKey of Object.keys(localStorage)){
                if(locStKey != "loglevel:webpack-dev-server"){
                    let locStValue = JSON.parse(localStorage.getItem(locStKey));
                    if('item' in locStValue) { this.todoItems.push(locStValue); }
                }
            }
        }
  },

  components: {
    // 컴포넌트 태그명 : 컴포넌트 내용
    'TodoHeader'   : TodoHeader,
    'TodoInput'    : TodoInput,
    'TodoList'     : TodoList,
    'TodoFooter'   : TodoFooter
  },

  methods: {
    addOneItem: function(todoItem) {
        let obj = { completed: false, item: todoItem } ;

        localStorage.setItem(todoItem, JSON.stringify(obj));
        this.todoItems.push(obj);
    },

    removeOneItem: function(todoItem, index){
        localStorage.removeItem(todoItem.item);
        this.todoItems.splice(index, 1);
    },

    toggleOneComplete: function(todoItem, index){
        /** Update Local Storage Item */
        localStorage.removeItem(todoItem.item);
        localStorage.setItem(todoItem.item, JSON.stringify(todoItem));

        this.todoItems[index].completed = !this.todoItems[index].completed;
    },

    clearAllItems: function(){
        localStorage.clear();
        this.todoItems = [];
    }
  }
}

</script>

<style>
body{
  text-align: center;
  background-color: #f6f6f6;
}

input{
  border-style: groove;
  width: 200px;
}

button{
  border-style: groove;
}

.shadow{
  box-shadow: 5px 10px 10px rgba(0,0,0,0.03);
}
</style>
