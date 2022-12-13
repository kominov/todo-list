<template>
  <div id="app">
    <h1>Список дел</h1>
    <div class="todo-add__inputGroup">
      <input
        @keydown.enter="onAddTodo"
        v-model="todoInputStr" />

      <button
        class="todo-add__button"
        @click="onAddTodo">Добавить задачу</button>
    </div>

    <draggable
      v-if="list.length"
      class="list-group"
      v-model="list"
      v-bind="dragOptions"
      @end="onAddToLocalstorage"
    >
      <transition-group
        tag="div"
        type="transition" name="flip-list">

        <div
          class="list-group-item"
          v-for="element in list"
          :key="element.id"
        >

         <TodoItem
         :todo="element"

         @onRemoveTodo="onRemoveTodo"
         @onCheck="onCheckTodo"
         @onUpdateTodo="onUpdateTodo"
         />

        </div>
      </transition-group>

    </draggable>
    <template v-else>
      <h3>Список дел пуст</h3></template>
  </div>
</template>

<script>
import draggable from 'vuedraggable';
import TodoItem from '@/components/TodoItem.vue';

export default {
  name: 'app',
  components: {
    TodoItem,
    draggable,
  },
  data() {
    return {
      todoInputStr: '',
      list: [],
    };
  },
  mounted() {
    this.list = JSON.parse(localStorage.getItem('todoList')) || [];
  },

  watch: {
    list: {
      handler() {
        this.onAddToLocalstorage();
      },
      deep: true,
    },
  },

  methods: {
    onAddTodo() {
      if (this.todoInputStr) {
        this.list.push({ todo: this.todoInputStr, id: Date.now(), check: false });
        this.todoInputStr = '';
      }
    },

    onRemoveTodo(id) {
      const newList = [...this.list];
      this.list = newList.filter((t) => t.id !== id);
    },

    onCheckTodo(element) {
      element.check = !element.check;
    },

    onUpdateTodo(props) {
      const { element, updateStringTodo } = props;
      element.todo = updateStringTodo;
    },

    onAddToLocalstorage() {
      localStorage.setItem('todoList', JSON.stringify(this.list));
    },
  },
  computed: {
    dragOptions() {
      return {
        animation: 0,
        group: 'description',
        disabled: false,
        ghostClass: 'ghost',
      };
    },
  },
};
</script>

<style lang="scss">
*{
  padding: 0;
  margin: 0;
}
body{
  box-sizing: border-box;
  color: #2c3e50;
  background-color: #e9ecef;
}
#app {
  max-width: 1200px;
  min-height: 100vh;
  padding: 0 20px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0 auto;
}

h1{
  margin: 40px 0;
}

.todo-add__inputGroup{
  margin-bottom: 40px;
  display: flex;
  justify-content: center;
}
.todo-add__inputGroup {
   & input {
     margin-right: 16px;
     padding: 0 8px;

     &:focus-visible {
       outline: none;
     }
   }
}
.todo-add__button {
  color: #fff;
  background-color: #6c757d;
  border:1px solid #6c757d ;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  padding: 8px;
  outline: none;
  cursor: pointer;
}

.flip-list-move {
  transition: transform 0.5s;
}

.no-move {
  transition: transform 0s;
}

.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}

.list-group {
  min-height: 20px;
  width: 600px;
  margin: 0 auto;
}

.list-group-item {
  cursor: move;
}
</style>
