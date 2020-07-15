<template>
    <div 
        class="card card-yellow"
        :class="phase.class"
    >
        <div class="card-img">
            <img :src="`https://avatars.dicebear.com/api/human/${student.name}.svg`" alt="avatar">
        </div>
        <div class="card-body">
            <p>{{ student.name }}</p>
        </div>
        <div class="card-button">
            <i @click="changePage" class="fas fa-pen-square"></i>
            <i @click="deleteStudent" class="fas fa-minus-square"></i>
        </div>
    </div>
</template>

<script>
export default {
    props: [ 'student', 'phase' ],
    methods: {
        changePage() {
            let data = {
                name: 'eidtFormPage',
                student: this.student
            }
            this.$emit('changePage', data)
        },
        deleteStudent() {
            this.$emit('deleteStudent', this.student.id)
        }
    }
}
</script>

<style>

</style>