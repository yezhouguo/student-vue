<template>
  <div id="student-form">
    <button v-if="addstatus == false" v-on:click="ShowAdd()">Add Student Here</button>
    
    <form v-if="addstatus == true" v-on:submit.prevent="handleSubmit">
      <label>Student name</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidName }"
        v-model="student.name"
        v-on:focus="clearStatus"
        v-on:keypress="clearStatus"
      />
      <label>Student gender</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidGender }"
        v-model="student.gender"
        v-on:focus="clearStatus"
      />
      <label>Student birthday</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidBirthday }"
        v-model="student.birthday"
        v-on:focus="clearStatus"
      />
      <label>Student native_place</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidNative_Place }"
        v-model="student.native_place"
        v-on:focus="clearStatus"
      />
      <label>Student major</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidMajor }"
        v-model="student.major"
        v-on:focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Student successfully added</p>
      <button>Add Student</button>
      <button v-if="addstatus == true" v-on:click="ShowAdd()">Cancel</button>
    </form>
    <button v-on:click="ShowSearch()">Search By ID</button>
    <p v-if="searchstatus == false" class="normalStatus"></p>
    <form v-else v-on:submit.prevent="handleSearch">
      <label>Student ID</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': searching && invalidId }"
        v-model="student.id"
        v-on:focus="clearStatus"
        v-on:keypress="clearStatus"
      />
    <button>Search Student</button>
    <button v-if="searchstatus == true" v-on:click="ShowSearch()">Cancel</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "student-form",
  computed: {
    invalidId() {
      return this.student.id === "";
    },
    invalidName() {
      return this.student.name === "";
    },

    invalidGender() {
      return this.student.gender === "";
    },

    invaliBirthday() {
      return this.student.birthday === "";
    },

    invalidNative_Place() {
      return this.student.native_place === "";
    },

    invalidMajor() {
      return this.student.major === "";
    },
  },
  data() {
    return {
      submitting: false,
      searching:false,
      error: false,
      success: false,
      addstatus:false,
      searchstatus:false,
      student: {
        id: "",
        name: "",
        gender: "",
        birthday: "",
        native_place: "",
        major: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidGender || this.invaliBirthday || this.invalidNative_Place || this.invalidMajor) {
        this.error = true;
        return;
      }

      this.$emit("add:student", this.student);
      this.student = {
        id: "",
        name: "",
        gender: "",
        birthday: "",
        native_place: "",
        major: ""
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
      
      this.$emit("show:student");
    },

    handleSearch() {
        this.searching=true;
        this.clearStatus();

        if(this.invalidId)
        {
            this.error=true;
            return;
        }

        this.$emit("search:student", this.student.id);
        
        this.error = false;
        this.success = true;
        this.searching = false;
        

    },

    clearStatus() {
      this.success = false;
      this.error = false;
    },

    ShowAdd() {
        if(this.addstatus==false)
            this.addstatus=true;
        else
            this.addstatus=false;
        this.$emit("show:student");
    },

    ShowSearch(){
        if(this.searchstatus==false)
            this.searchstatus=true;
        else
            this.searchstatus=false;
        this.$emit("show:student");
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>