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
            <p>Name:</p>
            <input type="text" placeholder="Please enter your name..." class="form_input_name" v-model="todoItem.name"
              required @keydown="handleInputName" />
            <div v-if="isErrorName">
              <p class="error">please enter this field</p>
            </div>
            <div v-if="isCheckName">
              <p class="error">
                Please enter at least 8 characters
              </p>
            </div>
          </div>
          <div class="form_input_container">
            <p>Ages:</p>
            <input type="number" placeholder="Please enter your age..." class="form_input_age" v-model="todoItem.age"
              required @keydown="handleInputAge" />
            <div v-if="isErrorAge">
              <p class="error">please enter this field</p>
            </div>
            <div v-if="isCheckAge">
              <p class="error">
                Please re-enter your age
              </p>
            </div>
          </div>
          <div class="form_input_container">
            <p>Discriptions:</p>
            <input type="text" placeholder="Please enter your Discriptions..." class="form_input_age"
              v-model="todoItem.discriptions" required @keydown="handleInputDiscriptions" />
            <div v-if="isErrorDiscriptions">
              <p class="error">please enter this field</p>
            </div>
          </div>
        </form>
      </div>
      <!-- <input type="submit" value="addTodo"  /> -->
      <button class="btn_user" @click="addTodo">Submit</button>
    </div>
    <div v-if="todoList.length">
      <TodosComponent :listUser="todoList" @editUser="setUser" />
    </div>
  </div>
</template>

<script>
import TodosComponent from "./TodosComponent.vue";
import { v4 as uuidv4 } from "uuid";
export default {
  name: "TodoList",
  props: {},
  components: {
    TodosComponent,
  },
  data() {
    return {
      todoList: [],
      isName: false,
      show: false,
      isErrorName: false,
      isErrorAge: false,
      isCheckAge: false,
      isCheckName:false,
      isErrorDiscriptions: false,
      todoItem: {
        name: "",
        age: "",
        id: "",
        discriptions: "",
      },
      editMode: false,
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
      this.todoItem = { ...val }
    },
    handleInputName() {
      this.isErrorName = false
    },
    handleInputAge() {
      this.isErrorAge = false
      this.isCheckAge = false
    },
    handleInputDiscriptions() {
      this.isErrorDiscriptions = false
    },
    addTodo: function () {
      if (this.todoItem.id) {
        this.todoList = this.todoList.map(item => {
          if (item.id === this.todoItem.id) {
            return { ...this.todoItem }
          } else {
            return item
          }
        })
        this.todoItem.name = "";
        this.todoItem.age = "";
        this.todoItem.id = "";
        this.todoItem.discriptions = "";
        return
      }
      if (this.todoItem.name === "" || this.todoItem.age === "" || this.todoItem.discriptions === "") {
        if (this.todoItem.name === "") this.isErrorName = true
        if (this.todoItem.age === "") this.isErrorAge = true
        if (this.todoItem.discriptions === "") this.isErrorDiscriptions = true
        return;
      }
      if (Number(this.todoItem.age) < 0) {
        this.isCheckAge = true
        return
      }
      if(this.todoItem.name.length < 8) {
        this.isCheckName = true
        return
      }
      this.todoList.push({
        name: this.todoItem.name,
        age: this.todoItem.age,
        discriptions: this.todoItem.discriptions,
        id: uuidv4(),
      });
      this.todoItem.name = "";
      this.todoItem.age = "";
      this.todoItem.id = "";
      this.todoItem.discriptions = "";
      console.log("this.todoList", this.todoList);
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

.lable>p {
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

.form_input_name,
.form_input_age {
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

.form_input_container>p {
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

/* reposive */
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