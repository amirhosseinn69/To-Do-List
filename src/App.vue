<script>
import Nav from './components/nav.vue';
import search from './components/search.vue';

export default {
  components: {
    Nav,
    search
  },
  data() {
    return {
      todoLists: [],
      showAddInput: false,
      newTaskText: '',
      editingTask: null,
      searchQuery: ''
    };
  },
  computed: {
    filteredTodoLists() {
      if (!this.searchQuery) return this.todoLists;
      const q = this.searchQuery.toLowerCase();
      return this.todoLists
        .map(list => ({
          ...list,
          todos: list.todos.filter(todo => todo.text && todo.text.toLowerCase().includes(q))
        }))
        .filter(list => list.todos.length > 0);
    }
  },
  methods: {
    addTask() {
      const text = this.newTaskText && this.newTaskText.trim();
      if (!text) return;
      this.todoLists.push({
        title: '',
        todos: [
          {
            id: Date.now(),
            text
          }
        ]
      });
      this.newTaskText = '';
      this.showAddInput = false;
    },
    deleteTask(listIdx, todoIdx) {
      this.todoLists[listIdx].todos.splice(todoIdx, 1);
      if (this.todoLists[listIdx].todos.length === 0) {
        this.todoLists.splice(listIdx, 1);
      }
    },
    startEdit(listIdx, todoIdx, text) {
      this.editingTask = { listIdx, todoIdx, text };
    },
    saveEdit(listIdx, todoIdx) {
      const text = this.editingTask.text && this.editingTask.text.trim();
      if (!text) return;
      this.todoLists[listIdx].todos[todoIdx].text = text;
      this.editingTask = null;
    },
    cancelEdit() {
      this.editingTask = null;
    }
  }
};
</script>

<template>
  <Nav/>
  <search v-model="searchQuery" />
  <div class="lists-container">
    <template v-if="filteredTodoLists.length > 0">
      <div
        v-for="(list, idx) in filteredTodoLists"
        :key="idx"
        class="todo-list"
      >
        <div class="todo" v-for="(todo, tIdx) in list.todos" :key="todo.id">
          <template v-if="editingTask && editingTask.listIdx === idx && editingTask.todoIdx === tIdx">
            <textarea v-model="editingTask.text" rows="2" style="resize: vertical; width: 100%"></textarea>
            <div class="option">
              <button class="edit" @click="saveEdit(idx, tIdx)">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M5 14.5s1.5 0 3.5 3.5c0 0 5.559-9.167 10.5-11"/></svg>
              </button>
              <button class="delete" @click="cancelEdit()">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="M19 4h-3.5l-1-1h-5l-1 1H5v2h14M6 19a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V7H6z"/></svg>
              </button>
            </div>
          </template>
          <template v-else>
            <h3>{{ todo.text }}</h3>
            <div class="option">
              <button class="edit" @click="startEdit(idx, tIdx, todo.text)">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="currentColor" stroke-width="1.5" d="m14.36 4.079l.927-.927a3.932 3.932 0 0 1 5.561 5.561l-.927.927m-5.56-5.561s.115 1.97 1.853 3.707C17.952 9.524 19.92 9.64 19.92 9.64m-5.56-5.561l-8.522 8.52c-.577.578-.866.867-1.114 1.185a6.6 6.6 0 0 0-.749 1.211c-.173.364-.302.752-.56 1.526l-1.094 3.281m17.6-10.162L11.4 18.16c-.577.577-.866.866-1.184 1.114a6.6 6.6 0 0 1-1.211.749c-.364.173-.751.302-1.526.56l-3.281 1.094m0 0l-.802.268a1.06 1.06 0 0 1-1.342-1.342l.268-.802m1.876 1.876l-1.876-1.876"/></svg>
              </button>
              <button class="delete" @click="deleteTask(idx, tIdx)">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="M19 4h-3.5l-1-1h-5l-1 1H5v2h14M6 19a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V7H6z"/></svg>
              </button>
            </div>
          </template>
        </div>
      </div>
    </template>
    <template v-else>
      <div style="text-align:center; color:#888; margin-top:2em;">
        no task yet!
      </div>
    </template>
    <button class="add-note" @click="showAddInput = !showAddInput">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="M13 6a1 1 0 1 0-2 0v5H6a1 1 0 1 0 0 2h5v5a1 1 0 1 0 2 0v-5h5a1 1 0 1 0 0-2h-5z"/></svg>
    </button>
    <div v-if="showAddInput" class="add-input-box">
      <textarea v-model="newTaskText" placeholder="new task" rows="3" style="resize: vertical; width: 100%"></textarea>
      <button @click="addTask">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M5 14.5s1.5 0 3.5 3.5c0 0 5.559-9.167 10.5-11"/></svg>
      </button>
    </div>
  </div>
</template>

<style lang="sass">





</style>

