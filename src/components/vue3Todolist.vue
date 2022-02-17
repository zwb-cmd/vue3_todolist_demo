<template>
  <div>
    <section class="todoapp">
      <header class="header">
        <h1>todos</h1>
        <input
          class="new-todo"
          placeholder="What needs to be done?"
          v-model="inputValue"
          autofocus
          @keyup="addList"
        />
      </header>
      <!-- This section should be hidden by default and shown when there are todos -->
      <section class="main">
        <input
          @click="selectAll"
          id="toggle-all"
          :checked="!checked"
          class="toggle-all"
          type="checkbox"
        />
        <label for="toggle-all">Mark all as complete</label>
        <ul class="todo-list">
          <!-- These are here just to show the structure of the list items -->
          <!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
          <li
            v-for="(v, i) in showlist"
            :class="[
              v.state ? 'completed' : '',
              'todo',
              v.edit ? 'editing' : '',
            ]"
          >
            <div class="view">
              <input
                class="toggle"
                type="checkbox"
                :checked="v.state"
                @click="changeState(v)"
              />
              <label @dblclick="editLabel(v)">{{ v.label }}</label>

              <button class="destroy" @click="deleteTodo(v)"></button>
            </div>
            <input
              class="edit"
              v-model="v.label"
              @keyup.enter="saveTodoLabel(v)"
            />
          </li>
        </ul>
      </section>
      <!-- This footer should be hidden by default and shown when there are todos -->
      <footer class="footer">
        <!-- This should be `0 items left` by default -->
        <span class="todo-count"><strong>{{list.filter(el=>!el.state).length}}</strong> item left</span>
        <!-- Remove this if you don't implement routing -->
        <ul class="filters">
          <li>
            <a
              @click="changeTab('all')"
              :class="active == 'all' ? 'selected' : ''"
              href="#/"
              >All</a
            >
          </li>
          <li>
            <a
              @click="changeTab('active')"
              :class="active == 'active' ? 'selected' : ''"
              href="#/active"
              >Active</a
            >
          </li>
          <li>
            <a
              @click="changeTab('completed')"
              :class="active == 'completed' ? 'selected' : ''"
              href="#/completed"
              >Completed</a
            >
          </li>
        </ul>
        <!-- Hidden if no completed items are left ↓ -->
        <button
          v-if="clearFlag"
          class="clear-completed"
          @click="clearCompletedTodo"
        >
          Clear completed
        </button>
      </footer>
    </section>
  </div>
</template>
<script>
import { reactive, toRefs, onMounted, computed, watch } from "vue";
export default {
  name: "todo",
  setup() {
    let todoinfo = reactive({
      list: [],
      active: "all",
    });
    onMounted(() => {
      let todolist = window.localStorage.getItem("todolist");
      if (todolist) {
        todoinfo.list = JSON.parse(todolist);
      } else {
        saveList();
      }
      todoinfo.active = location.href.split("#/")[1]
        ? location.href.split("#/")[1]
        : "all";
      getAllState(todoinfo.list);
    });
    // 保存数据
    function saveList(list = []) {
      if (list.length) {
        window.localStorage.setItem("todolist", JSON.stringify(list));
      } else {
        window.localStorage.setItem("todolist", JSON.stringify(todoinfo.list));
      }
    }
    // 数据变化就保存
    watch(()=>todoinfo.list, (newValue) => {
      saveList(newValue);
    },{
			deep:true,
		});
    // 添加todo
    todoinfo.inputValue = "";
    function addList(e) {
      if (e.key != "Enter") return;
      todoinfo.list.push({
        label: todoinfo.inputValue,
        state: false,
        edit: false,
      });
      todoinfo.inputValue = "";
    }
    // 改变todo状态
    function changeState(v) {
      const index = todoinfo.list.findIndex((el) => el == v);
      todoinfo.list[index].state = !todoinfo.list[index].state;
      let comLen = todoinfo.list.filter((el) => el.state).length;
      if (comLen == todoinfo.list.length) {
        todoinfo.checked = false;
      } else {
        todoinfo.checked = true;
      }
    }
    //改变label
    function editLabel(v) {
      const index = todoinfo.list.findIndex((el) => el == v);
      todoinfo.list[index].edit = true;
    }
    // 删除todo
    function deleteTodo(v) {
      const index = todoinfo.list.findIndex((el) => el == v);
      todoinfo.list.splice(index, 1);
    }
    // 查看任务面板状态tab
    function changeTab(val) {
      todoinfo.active = val;
    }
    // 清楚已完成todo
    function clearCompletedTodo() {
      todoinfo.list = todoinfo.list.filter((el) => !el.state);
    }
		// 是否全选
    todoinfo.checked = true;
    function selectAll() {
      if (todoinfo.checked) {
        todoinfo.list.forEach((el) => {
          el.state = true;
        });
      } else {
        todoinfo.list.forEach((el) => {
          el.state = false;
        });
      }
      todoinfo.checked = !todoinfo.checked;
    }
		// 获取全选状态
    function getAllState(list = []) {
      let comLen = list.filter((el) => el.state).length;
      if (comLen == list.length) {
        todoinfo.checked = false;
      } else {
        todoinfo.checked = true;
      }
    }
		// 保存修改label
    function saveTodoLabel(v) {
      const index = todoinfo.list.findIndex((el) => el == v);
      todoinfo.list[index].edit = false;
    }
    // 展示list
    todoinfo.showlist = computed(() => {
      if (todoinfo.active == "active") {
        return todoinfo.list.filter((el) => !el.state);
      } else if (todoinfo.active == "completed") {
        return todoinfo.list.filter((el) => el.state);
      } else {
        return todoinfo.list;
      }
    });
    // 清楚按钮显示、隐藏
    todoinfo.clearFlag = computed(() => {
      return todoinfo.list.filter((el) => el.state).length ? true : false;
    });
    return {
      ...toRefs(todoinfo),
      addList,
      changeState,
      saveList,
      editLabel,
      deleteTodo,
      changeTab,
      clearCompletedTodo,
      selectAll,
      saveTodoLabel,
    };
  },
};
</script>
<style>
@import url("../../node_modules/todomvc-common/base.css");
@import url("../../node_modules/todomvc-app-css/index.css");
</style>