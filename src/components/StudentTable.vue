<template>
  <div id="student-table">
    <p v-if="students==null || students.length < 1" class="empty-table">No students</p>
    <table v-else>
      <thead>
        <tr>
          <th>Student id</th>
          <th>Student name</th>
          <th>Student gender</th>
          <th>Student birthday</th>
          <th>Student native_place</th>
          <th>Student major</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="student in students" v-bind:key="student.id">
          <td>{{student.id}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.name" />
          </td>
          <td v-else>{{student.name}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.gender" />
          </td>
          <td v-else>{{student.gender}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.birthday" />
          </td>
          <td v-else>{{student.birthday}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.native_place" />
          </td>
          <td v-else>{{student.native_place}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.major" />
          </td>
          <td v-else>{{student.major}}</td>
          <td v-if="editing === student.id">
            <button v-on:click="editStudent(student)">Save</button>
            <button class="muted-button" v-on:click="cancelEdit(student)">Cancel</button>
          </td>
          <td v-else>
            <button v-on:click="editMode(student)">Edit</button>
            <button v-on:click="$emit('delete:student',student.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "student-table",
  props: {
    students: Array
  },
  data() {
    return {
      editing: null
    };
  },
  methods: {
    editMode(student) {
      this.cachedStudent = Object.assign({}, student);
      this.editing = student.id;
    },
    cancelEdit(student) {
      Object.assign(student, this.cachedStudent);
      this.editing = null;
    },

    editStudent(student) {
      if (student.name === "" || student.gender === "") return;
      this.$emit("edit:student", student.id, student);
      this.editing = null;
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>