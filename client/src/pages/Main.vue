<template>
    <section id="main">
        <div class="btn-corner">
            <i @click="changePage('addForm')" class="far fa-plus-square"></i>
        </div>
        <!-- MAIN CONTAINER -->
        <div class="container">
            <h1 @click="changePage('home')" style="text-align: center; margin: 2em 0; cursor:pointer;">Hacktiv8 Students</h1>
            <div v-if="pageName == 'home'" class="container-category">
                <Category 
                    v-for="phase in phases" 
                    :key="phase.category" 
                    :phaseData="phase" 
                    :studentData="students"
                    @deleteStudentData="deleteStudent"
                    @editFormButton="editPage"
                    >
                </Category>
            </div>
            <AddForm v-else-if="pageName == 'addForm'" @addNewStudent="addStudent"></AddForm>
            <EditForm 
                v-else-if="pageName == 'editForm'" 
                :studentDetail="studentDetail"
                @editMethod="editData">
            </EditForm>
        </div>
    </section>
</template>

<script>
import Category from '../components/Category'
import AddForm from '../components/AddForm'
import EditForm from '../components/EditFrom'
import axios from '../config/axios'
export default {
    name: 'Main',
    data() {
        return {
            pageName: 'home',
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
            ],
            students: [],
            studentDetail: {}
        }
    },
    methods: {
        changePage(name) {
            this.pageName = name
        },
        editPage(payload) {
            this.pageName = payload.pageName
            this.studentDetail = payload.data
        },
        editData(payload) {
            axios({
                method: 'put',
                url: `/students/${payload.id}`,
                data: {
                    category: payload.category,
                    name: payload.name
                }
            })
                .then(({ data }) => {
                    let index = this.students.findIndex(el => el.id == payload.id)
                    this.students.splice(index, 1, data)
                    this.changePage('home')
                })
                .catch(err => {
                    console.log(err, '<<<<<<<<<<<<<< error')
                })
        },
        deleteStudent(payload) {
            axios({
                method: 'delete',
                url: `/students/${payload.id}`,
            })
                .then(_=> {
                    let index = this.students.findIndex(el => el.id == payload.id)
                    this.students.splice(index, 1)
                })
                .catch(err => {
                    console.log(err, '<<<<<<<<<<<<<< error')
                })
        },
        addStudent(payload) {
            axios({
                method: 'post',
                url: '/students',
                data: {
                    name: payload.name,
                    category: payload.category
                }
            })
                .then(({ data }) => {
                    this.students.push(data)
                    this.changePage('home')
                })
                .catch(err => {
                    console.log(err, '<<<<<<<<<<<<<< error')
                })
        },
        fetchStudents() {
            axios({
                method: 'get',
                url: `/students`
            })
                .then(({ data }) => {
                    this.students = data
                })
                .catch(err => {
                    console.log(err, '<<<<<<<<<<<<<< error')
                })
        }
    },
    created() {
        this.fetchStudents()
    },
    components: {
        Category, AddForm, EditForm
    }
}
</script>

<style>

</style>