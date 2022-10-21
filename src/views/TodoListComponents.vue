<template>
  <div class="container_todos">
    <div class="container_todos_table">
      <table v-if="listWorks.length">
        <tr>
          <th>Date</th>
          <th>Title</th>
          <th>Discriptions</th>
          <th>Actions</th>
        </tr>
        <tr v-for="(works, index) in listWorks" :key="index">
          <td>{{ works.date }}</td>
          <td>{{ works.title }}</td>
          <td>
            <div class="text_discriptions">
              <p>{{ works.discriptions }}</p>
            </div>
          </td>
          <td>
            <div class="action">
              <input
                type="submit"
                value="Delete"
                class="action_btn_delete"
                @click="deleteUser(listWorks, index)"
              />
              <input
                type="submit"
                value="Edit"
                class="action_btn_edit"
                @click="$emit('editUser', works, index)"
              />
            </div>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodosComponent",
  props: {
    listWorks: {
      require: true,
    },
  },
  data() {
    return {};
  },

  methods: {
    deleteUser: function (userList, index) {
      console.log("delete", userList, index);
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
  border-collapse: collapse;
}

tr {
  align-items: center;
}

td,
th {
  border: 2px solid #4cb1f8;
  min-width: 20%;
  text-align: center;
  text-overflow: ellipsis;
  padding: 4px;
}
th {
  background-color: #ccc;
  border-collapse: collapse;
  overflow: hidden;
  height: 10%;
  min-width: 20%;
}

.action {
  display: flex;
  justify-content: space-around;
}

.action_btn_delete,
.action_btn_edit {
  min-width: 80px;
  margin: 6px;
  border-radius: 10px;
  border: none;
  padding: 8px 0;
}
.action_btn_delete {
  background-color: #dc3545;
  color: #fff;
}
.action_btn_edit {
  background-color: #ffc107;
  color: #000;
}
.text_discriptions {
  height: 50px;
  padding: 4px;
  align-items: center;
  display: flex;
  justify-content: center;
}
.text_discriptions > p {
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  display: -webkit-box;
  margin: 0;
}

/* responsive */
@media screen and (max-width: 1024px) {
  .container_todos {
    font-size: 14px;
  }
}
@media screen and (max-width: 960px) {
  .container_todos {
    font-size: 12px;
  }
}
@media screen and (max-width: 640px) {
  .container_todos {
    font-size: 10px;
  }
}
</style>