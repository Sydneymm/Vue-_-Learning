<template>
  <div id="root">
		<div class="todo-container">
			<div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList :todos="todoList" ></MyList>
        <MyFooter :todos="todoList" :totalCheckToggle="totalCheckToggle" @clearThings="clearThings"></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
import MyFooter from './components/MyFooter.vue';
import MyHeader from './components/MyHeader.vue';
import MyList from './components/MyList.vue';
import pubsub from 'pubsub-js';

export default {
  name: 'App',
  data() {
    return {
      //由于todos是MyHeader组件和MyFooter组件都在使用，所以放在App中（状态提升）
      todoList: JSON.parse(localStorage.getItem('todoList')) || [],
    }
  },
  components: {
    MyFooter,
    MyHeader,
    MyList
  },
  methods: {
    addTodo(todoObj) {
      this.todoList.unshift(todoObj);
    },
    toogleCheck(id) {
      this.todoList.forEach(element => {
        if(element.id === id ) {
          element.done = !element.done;
        }
      });
    },
    totalCheckToggle(isChecked) {
      this.todoList.forEach(i => i.done = isChecked);
    },
    clearThings() {
      this.todoList = this.todoList.filter(todo => !todo.done)
    }
  },
  watch: {
			todoList:{
				deep:true,
				handler(value){
					localStorage.setItem('todoList',JSON.stringify(value))
				}
			}
	},
  mounted() {
    this.$bus.$on('toogleCheck', this.toogleCheck);
    this.pubsubId = pubsub.subscribe('deleteTodo', (_, id)=> {
      this.todoList = this.todoList.filter(item => item.id !== id);
    })
  },
  beforeDestroy() {
    this.$bus.$off('toogleCheck');
    pubsub.unsubscribe(this.pubsubId);
  }
}
</script>

<style>
  /*base*/
	body {
		background: #fff;
	}
	.btn {
		display: inline-block;
		padding: 4px 12px;
		margin-bottom: 0;
		font-size: 14px;
		line-height: 20px;
		text-align: center;
		vertical-align: middle;
		cursor: pointer;
		box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
		border-radius: 4px;
	}
	.btn-danger {
		color: #fff;
		background-color: #da4f49;
		border: 1px solid #bd362f;
	}
	.btn-danger:hover {
		color: #fff;
		background-color: #bd362f;
	}
	.btn:focus {
		outline: none;
	}
	.todo-container {
		width: 600px;
		margin: 0 auto;
	}
	.todo-container .todo-wrap {
		padding: 10px;
		border: 1px solid #ddd;
		border-radius: 5px;
	}
</style>