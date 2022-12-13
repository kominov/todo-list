<template>
<div class="todo-add__item">
  <div class="todo-add_item-input">
    <input
      type="text"
      v-model="todoStr"
      @blur="onUpdateTodo"
      @keydown.enter="onUpdateTodo"
    />
  </div>

  <div>
    <span
      class="todo-add__check"
      @click="onCheck">
      <span v-if="todo.check">&#9989;</span>
      <span v-else>&#9940;</span>
    </span>

    <span
      class="todo-add__del"
      @click="onRemoveTodo">&#10008;
    </span>
  </div>
</div>

</template>

<script>
export default {
  name: 'TodoItem',
  emits: ['onCheck', 'onRemoveTodo', 'onUpdateTodo'],
  data() {
    return {
      todoStr: '',
    };
  },
  mounted() {
    this.todoStr = this.todo.todo;
  },
  props: {
    todo: {
      type: Object,
      default: () => ({}),
    },
  },
  methods: {
    onCheck() {
      this.$emit('onCheck', this.todo);
    },

    onRemoveTodo() {
      this.$emit('onRemoveTodo', this.todo.id);
    },

    onUpdateTodo() {
      if (this.todo.todo !== this.todoStr) {
        this.$emit('onUpdateTodo', {
          element: this.todo,
          updateStringTodo: this.todoStr,
        });
      }
    },
  },
};
</script>

<style lang="scss">
.todo-add__item{
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: 0.75rem 1.25rem;
  margin-bottom: -1px;
  background: #fff;
  border: 1px solid rgba(0,0,0,.125);

  & input{
    border: none;
    padding: 2px 8px;
    width: 100%;

    &:focus-visible {
      outline: 1px solid black;
    }
  }
}
.todo-add_item-input{
  width: 70%;
  cursor: pointer;
}
.todo-add__del{
  color: red;
  cursor: pointer;
  font-size: 20px;
}

.todo-add__check{
  margin-right: 8px;

  & > span{
    cursor: pointer;
    font-size: 20px;
  }
}
</style>
