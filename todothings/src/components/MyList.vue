<template>
  <ul class="todo-main">
	<transition-group name="todoList">
		<MyItem v-for="t in todos" :key="t.id" :todo="t" ></MyItem>
	</transition-group>
  </ul>
</template>

<script>
import MyItem from './MyItem.vue'
export default {
  name: 'MyList',
  components: {
    MyItem
  },
  props: ['todos']
}
</script>

<style scoped>
	/*main*/
	.todo-main {
		margin-left: 0px;
		border: 1px solid #ddd;
		border-radius: 2px;
		padding: 0px;
	}

	.todo-empty {
		height: 40px;
		line-height: 40px;
		border: 1px solid #ddd;
		border-radius: 2px;
		padding-left: 5px;
		margin-top: 10px;
	}

	.todoList-enter-active,
	.todoList-leave-active {
	transition: all 0.5s ease;
	}
	.todoList-enter-from,
	.todoList-leave-to {
	opacity: 0;
	transform: translateX(30px);
	}
</style>