<template>
    <div class="category">
        <div class="category-head">
            <h3>Phase {{ phase.category }}</h3>
            <h3>{{ phase.icon }}</h3>
        </div>
        <div class="category-body">
            <StudentCard 
                v-for="student in studentPerPhase" 
                :key="student.id" 
                :student="student" 
                :phase="phase" 
                @changePage="changePage"
                @deleteStudent="deleteStudent">
            </StudentCard>
        </div>
    </div>
</template>

<script>
import StudentCard from './StudentCard'
export default {
    components: {
        StudentCard
    },
    props: [ 'phase', 'students' ],
    computed: {
        studentPerPhase() {
            let result = []
            this.students.forEach(student => {
                if(student.category == this.phase.category) {
                    result.push(student)
                }
            })
            return result
        }
    },
    methods: {
        changePage(name) {
            this.$emit('changePage', name)
        },
        deleteStudent(id) {
            this.$emit('deleteStudent', id)
        }
    }
}
</script>

<style>

</style>