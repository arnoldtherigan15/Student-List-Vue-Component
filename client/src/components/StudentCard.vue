<template>
    <div 
        class="card"
        :class="phase.class">
        <div class="card-img">
            <img :src="`https://avatars.dicebear.com/api/human/${studentDetail.name}.svg`" alt="avatar">
        </div>
        <div class="card-body">
            <p>{{ studentDetail.name }}</p>
        </div>
        <div class="card-button">
            <i @click="editButton" class="fas fa-pen-square"></i>
            <i @click="deleteStudentCard" class="fas fa-minus-square"></i>
        </div>
    </div>
</template>

<script>
export default {
    name: 'StudentCard',
    props: [ 'studentDetail', 'phase' ],
    methods: {
        deleteStudentCard() {
            let payload = {
                id: this.studentDetail.id
            }
            this.$emit('deleteStudentCard' ,payload)
        },
        editButton() {
            let payload = {
                data: this.studentDetail,
                pageName: 'editForm'
            }
            this.$emit('editFormButtonFromCard', payload)
        }
    }
}
</script>

<style>

</style>