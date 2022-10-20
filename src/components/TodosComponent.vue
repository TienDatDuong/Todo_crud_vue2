<template>
  <div class="container_todos">
    <div class="container_todos_table">
      <table>
        <tr>
          <th>STT</th>
          <th>Name</th>
          <th>Age</th>
          <th>Discriptions</th>
          <th>Actions</th>
        </tr>
        <tr v-for="(users, index) in listUser" :key="index">
          <td>{{ index  }}</td>
          <td>{{ users.name }}</td>
          <td>{{ users.age }}</td>
          <td>{{ users.discriptions }}</td>
          <td class="action">
            <input
              type="submit"
              value="Delete"
              class="action_btn_delete"
              @click="deleteUser(listUser, index)"
            />
            <input
              type="submit"
              value="Edit"
              class="action_btn_edit"
              @click="$emit('editUser', users, index)"
            />
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
// import { ref } from 'vue'
export default {
  name: "TodosComponent",
  props: {
    listUser: {
      require: true,
    },
  },
  data() {
    return {};
  },

  methods: {
    deleteUser: function (userList, index) {
      console.log('delete',userList,index);
      userList.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.container_todos_table {
  display: flex;
  justify-content: center;
}

.container_todos {
  margin: 5% 0;
}

table {
  border: 1px solid #4cb1f8;
  width: 50%;
}

tr {
  align-items: center;
}

td,th {
  border: 1px solid #4cb1f8;
  min-width: 20%;
  text-align: center;
  text-overflow:ellipsis;
  border-collapse: collapse;
}
th{
  background-color: #ccc;
  border-collapse: collapse;
}

.action {
  display: flex;
  justify-content: space-around;
}

.action_btn_delete,.action_btn_edit {
  min-width: 80px;
  margin: 6px;
  border-radius: 10px;
  border: none;
  padding: 8px 0 ;
}
.action_btn_delete {
  background-color: #dc3545;
  color: #fff;
}
.action_btn_edit {
  background-color: #ffc107;
  color: #000;
}
/* responsive */
@media screen and (max-width: 1024px) {
  .container_todos{
    font-size: 14px;
  }
}
@media screen and (max-width: 960px) {
  .container_todos{
    font-size: 12px;
  }
}
@media screen and (max-width: 640px) {
  .container_todos{
    font-size: 10px;
  }
}
</style>