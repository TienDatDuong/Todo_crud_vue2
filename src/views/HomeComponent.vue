<template>
  <div class="container">
    <div>
      <h1 class="lable">
        <p>Todo Lists</p>
      </h1>
      <h2 class="lable_discriptions">Create new todo item</h2>
      <div class="container_form_input">
        <form class="form_input">
          <div class="form_input_container">
            <p>Title:</p>
            <input
              type="text"
              placeholder="Please enter your title..."
              class="form_input_title"
              v-model="todoItem.title"
              required
              @keydown="handleInputTitle"
            />
            <div v-if="isErrorTitle">
              <p class="error">please enter this field</p>
            </div>
          </div>
          <div class="form_input_container">
            <p>Discriptions:</p>
            <input
              type="text"
              placeholder="Please enter your Discriptions..."
              class="form_input_title"
              v-model="todoItem.discriptions"
              required
              @keydown="handleInputDiscriptions"
            />
            <div v-if="isErrorDiscriptions">
              <p class="error">please enter this field</p>
            </div>
          </div>
        </form>
      </div>
      <button class="btn_user" @click="addTodo">Submit</button>
    </div>
    <div v-if="todoList.length">
      <TodoListComponents :listWorks="todoList" @editUser="setUser" />
    </div>
  </div>
</template>

<script>
import TodoListComponents from "./TodoListComponents.vue";
import { v4 as uuidv4 } from "uuid";
export default {
  name: "TodoList",
  props: {},
  components: {
    TodoListComponents,
  },
  data() {
    return {
      todoList: [],
      isErrorTitle: false,
      isErrorDiscriptions: false,
      todoItem: {
        title: "",
        id: "",
        discriptions: "",
      },
      months: ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"],
    };
  },
  watch: {
    todoList(newValue) {
      localStorage.setItem("newValue", JSON.stringify(newValue));
    },
    deep: true,
  },
  mounted() {
    this.todoList = JSON.parse(localStorage.getItem("newValue")) || [];
  },
  methods: {
    setUser(val) {
      this.todoItem = { ...val };
      this.isErrorDiscriptions = false;
      this.isErrorTitle = false;
    },
    handleInputTitle() {
      this.isErrorTitle = false;
    },
    handleInputDiscriptions() {
      this.isErrorDiscriptions = false;
    },
    addTodo: function () {
      if (this.todoItem.id) {
        this.todoList = this.todoList.map((item) => {
          if (item.id === this.todoItem.id) {
            return { ...this.todoItem };
          } else {
            return item;
          }
        });
        this.todoItem.title = "";
        this.todoItem.id = "";
        this.todoItem.discriptions = "";
        return;
      }
      if (this.todoItem.title === "" || this.todoItem.discriptions === "") {
        if (this.todoItem.title === "") this.isErrorTitle = true;
        if (this.todoItem.discriptions === "") this.isErrorDiscriptions = true;
        return;
      }

      this.todoList.push({
        date:
          this.months[new Date().getMonth()] +
          "-" +
          new Date().getDay() +
          "-" +
          new Date().getFullYear(),
        title: this.todoItem.title,
        discriptions: this.todoItem.discriptions,
        id: uuidv4(),
      });
      this.todoItem.title = "";
      this.todoItem.id = "";
      this.todoItem.discriptions = "";
    },
  },
};
</script>
<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.container {
  width: 1200px;
  margin: auto;
  font-size: 16px;
}

.lable {
  height: 15vh;
  color: #4cb1f8;
  font-size: 48px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.lable > p {
  display: flex;
  justify-content: center;
  align-items: center;
}

h2 {
  font-size: 32px;
  color: #4cb1f8;
  margin-left: 20%;
  margin-bottom: 20px;
}

.form_input {
  display: flex;
  flex-direction: column;
  justify-content: start;
  margin-left: 26%;
  font-size: 16px;
  font-weight: 800;
}

.form_input_title {
  width: 55%;
  height: 32px;
  border-radius: 10px;
  padding: 20px;
  outline: #4cb1f8;
  border: 1px solid #4cb1f8;
}

.form_input_container {
  margin-bottom: 2%;
}

.form_input_container > p {
  margin-right: 5%;
  width: 50px;
  display: inline-block;
}

.btn_user {
  margin-left: 26%;
  width: 49%;
  padding: 6px 0;
  font-size: 18px;
  font-weight: 500;
  border-radius: 10px;
  border: 1px solid #4cb1f8;
  color: #fff;
  background-color: #003054;
}

.error {
  color: #dc3545;
  padding-top: 1%;
  margin-left: 10%;
  font-size: 12px;
}

/* responsive */
@media screen and (max-width: 1024px) {
  .lable {
    font-size: 36px;
  }

  .lable_discriptions {
    font-size: 24px;
  }

  .form_input,
  input::placeholde {
    font-size: 14px;
  }

  .container {
    width: 1024px;
  }
}

@media screen and (max-width: 960px) {
  .container {
    width: 100%;
  }

  .lable {
    font-size: 30px;
  }

  .lable_discriptions {
    font-size: 24px;
  }

  .form_input {
    font-size: 12px;
  }
}

@media screen and (max-width: 640px) {
  .lable {
    font-size: 24px;
  }

  .lable_discriptions {
    font-size: 18px;
  }

  .form_input,
  input::placeholde {
    font-size: 10px;
  }
}
</style>