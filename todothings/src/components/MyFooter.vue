<template>
  <div class="todo-footer">
		<label>
			<!-- <input type="checkbox" :checked="isAll" @change="checkAll"/> -->
			<input type="checkbox" :checked="isAllcheck" @change="totalCheckbox(!isAllcheck)" :totalCheckToggle="totalCheckToggle"/>
			<!-- <input type="checkbox" v-model="isAllCheck" :totalCheckToggle="totalCheckToggle"/> -->
		</label>
		<span>
			<span>已完成{{doneTotal}}</span> / 全部{{todos.length}}
		</span>
		<button class="btn btn-danger" @click="clearThings">清除已完成任务</button>
	</div>
</template>

<script>
export default {
  name: 'MyFooter',
  props: ['todos', 'totalCheckToggle'],
  computed: {
    doneTotal() {
      return this.todos.reduce((pre, cur) => pre + (cur.done ? 1 : 0), 0);
    },

    isAllcheck() {
		return this.doneTotal === this.todos.length && this.todos.length !== 0;
    }
  },

  methods: {
    totalCheckbox(isChecked) {
      this.totalCheckToggle(isChecked);
    },
	/* totalCheckbox(e) {
      this.totalCheckToggle();
    } */
	clearThings() {
		this.$emit('clearThings')
	}

  }
}
</script>

<style scoped>
	/*footer*/
	.todo-footer {
		height: 40px;
		line-height: 40px;
		padding-left: 6px;
		margin-top: 5px;
	}

	.todo-footer label {
		display: inline-block;
		margin-right: 20px;
		cursor: pointer;
	}

	.todo-footer label input {
		position: relative;
		top: -1px;
		vertical-align: middle;
		margin-right: 5px;
	}

	.todo-footer button {
		float: right;
		margin-top: 5px;
	}
</style>