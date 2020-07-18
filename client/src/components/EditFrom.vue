<template>
    <div class="addForm">
        <div class="addForm-container">
            <div class="addForm-head">
                <h3>Edit Student Data</h3>
            </div>
            <form @submit.prevent="editMethod" class="addForm-body">
                <div class="addForm-input">
                    <label for="name">
                        <i class="fas fa-user-graduate"></i>
                        Name 
                    </label>
                    <input v-model="name" type="text">
                </div>
                <div class="addForm-dropdown">
                    <label for="cars">
                        <i class="fas fa-layer-group"></i>
                        Choose a phase:
                    </label>
                    <select name="phase" id="phase" v-model="selected">
                        <option disabled value="">Please select one</option>
                        <option value="0">🍊 Phase 0</option>
                        <option value="1">🍎 Phase 1</option>
                        <option value="2">🍇 Phase 2</option>
                        <option value="3">🥑 Phase 3</option>
                    </select>
                </div>
                <div class="addForm-button">
                    <button>Save</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: 'EditForm',
    props: [ 'studentDetail' ],
    data() {
        return {
            selected: this.studentDetail.category,
            name: this.studentDetail.name
        }
    },
    methods: {
        editMethod() {
            let payload = {
                id: this.studentDetail.id,
                name: this.name,
                category: Number(this.selected)

            }
            this.$emit('editMethod', payload)
        }
    }
}
</script>

<style>

</style>