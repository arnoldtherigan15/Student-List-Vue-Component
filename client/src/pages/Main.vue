<template>
  <section id="main">
        <div class="btn-corner" v-if="pageName == 'home'">
            <i @click="changePage({name: 'addFormPage'})" class="far fa-plus-square"></i>
        </div>
        <h1 @click="changePage({name: 'home'})" style="text-align: center; margin: 2em 0; cursor:pointer">Hacktiv8 Students</h1>
        <div class="container" v-if="pageName == 'home'">
            <div class="container-category">
                <Category 
                  v-for="phase in phases" 
                  :key="phase.category" 
                  :phase="phase" 
                  :students="students" 
                  @changePage="changePage"
                  @deleteStudent="deleteStudent">
                </Category>
            </div>
        </div>
        <AddForm v-else-if="pageName == 'addFormPage'" @addStudents="addStudents"></AddForm>
        <EditForm v-else-if="pageName == 'eidtFormPage'" :studentDetail="studentDetail" @editStudent="editStudent"></EditForm>
    </section>
</template>

<script>
import Category from '../components/Category'
import AddForm from '../components/AddForm'
import EditForm from '../components/EditForm'
import axios from 'axios'
export default {
  components: {
    Category, AddForm, EditForm
  },
  data() {
    return {
      pageName: 'home',
      students: [],
      studentDetail: {},
      phases: [ 
          {
              category: 0,
              icon: '🍊',
              class: 'card-yellow'
          },
          {
              category: 1,
              icon: '🍎',
              class: 'card-red'
          },
          {
              category: 2,
              icon: '🍇',
              class: 'card-purple'
          },
          {
              category: 3,
              icon: '🥑',
              class: 'card-green'
          }
      ]
    }
  },
  methods: {
    changePage(payload) {
      if(payload.student) {
        this.studentDetail = payload.student
      }
      this.pageName = payload.name
    },
    fetchStudents() {
      axios({
          url: 'http://localhost:3000/students',
          method: 'get'
      })
        .then(({data}) => {
            this.students = data
        })
        .catch(err => {
            console.log(err, '>>>>>>>>>> error')
        })
    },
    addStudents(payload) {
      axios({
        url: 'http://localhost:3000/students',
        method: 'post',
        data: {
          name: payload.name,
          category: Number(payload.category)
        }
      })
        .then(({data}) => {
          this.students.push(data)
          this.changePage({ name: 'home' })
        })
        .catch(err => {
            console.log(err, '>>>>>>>>>> error')
        })
    },
    editStudent(payload) {
      axios({
        url: `http://localhost:3000/students/${payload.id}`,
        method: 'put',
        data: {
          name: payload.name,
          category: Number(payload.category)
        }
      })
        .then(({data}) => {
          let index = this.students.findIndex(el => el.id == data.id)
          this.students.splice(index, 1, data)
          this.changePage({ name: 'home' })
        })
        .catch(err => {
            console.log(err, '>>>>>>>>>> error')
        })
    },
    deleteStudent(id) {
      axios({
        url: `http://localhost:3000/students/${id}`,
        method: 'delete'
      })
        .then(_ => {
          let index = this.students.findIndex(el => el.id == id)
          this.students.splice(index, 1)
        })
        .catch(err => {
            console.log(err, '>>>>>>>>>> error')
        })
    }
  },
  created() {
    this.fetchStudents()
  }
}
</script>

<style>

</style>