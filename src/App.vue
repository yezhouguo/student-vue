<template>
  <div id="app" class="small-container">
    <h1>Students</h1>
    <student-form
      v-on:add:student="addStudent"
      v-on:search:student="searchStudent"
      v-on:show:student="showStudent"
    />
    <student-table
      v-bind:students="students"
      v-on:delete:student="deleteStudent"
      v-on:edit:student="editStudent"
    />
  </div>
</template>

<script>
import StudentTable from "./components/StudentTable.vue";
import StudentForm from "./components/StudentForm.vue";

export default {
  name: "App",
  components: {
    StudentTable,
    StudentForm
  },
  data() {
    return {
      students: []
    };
  },

  mounted() {
    this.getStudents();
  },

  methods: {
    async editStudent(id, updatedStudent) {
      try {
        await fetch("http://localhost:8083/students/" + id, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(updatedStudent)
        });
      } catch (error) {
        console.error(error);
      }
      this.getStudents();
    },

    async deleteStudent(deleteid) {
      try {
        await fetch("http://localhost:8083/students/" + deleteid, {
          method: "DELETE"
        });
      } catch (error) {
        console.error(error);
      }
      this.getStudents();
    },

    async addStudent(student) {
      try {
        await fetch("http://localhost:8083/students", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(student)
        });
      } catch (error) {
        console.error(error);
      }
      this.getStudents();
    },

    async searchStudent(id) {
      try {
        const response = await fetch("http://localhost:8083/students/" + id, {
          method: "GET"
        });
        if (response.status == 404) {
          this.students = [
            (this.student = {
              id: "null",
              name: "null",
              gender: "null",
              birthday: "null",
              native_place: "null",
              major: "null"
            })
          ];
        } 
        else 
        {
          const tempstudent = await response.json();
          this.students = [(this.student = tempstudent)];
        }
      } catch (error) {
        console.error(error);
      }
    },

    showStudent() {
      this.getStudents();
    },

    async getStudents() {
      try {
        const response = await fetch("http://localhost:8083/students");
        const data = await response.json();
        if (data._embedded != null) this.students = data._embedded.students;
        else {
          this.students = [
            (this.student = {
              id: "null",
              name: "null",
              gender: "null",
              birthday: "null",
              native_place: "null",
              major: "null"
            })
          ];
        }
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
