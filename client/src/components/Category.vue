<template>
    <div class="category">
        <div class="category-head">
            <h3>Phase {{ phaseData.category }}</h3>
            <h3>{{ phaseData.icon }}</h3>
        </div>
        <div class="category-body">
            <StudentCard 
                v-for="student in studentPerPhase" 
                :key="student.id" 
                :studentDetail="student"
                :phase="phaseData"
                @deleteStudentCard="deleteStudent"
                @editFormButtonFromCard="editFormButton">
            </StudentCard>
        </div>
    </div>
</template>

<script>
import StudentCard from './StudentCard'
export default {
    props: [ 'phaseData', 'studentData' ],
    components: {
        StudentCard
    },
    methods: {
        deleteStudent(payload) {
            this.$emit('deleteStudentData', payload)
        },
        editFormButton(payload) {
            this.$emit('editFormButton', payload)
        }
    },
    computed: {
        studentPerPhase() {
            let result = []
            this.studentData.forEach(student => {
                if(student.category == this.phaseData.category) {
                    result.push(student)
                }
            })
            return result
        }
    }
}
</script>

<style>

</style>