<template>
    <div>
        <input type="text" placeholder="Buy milk" v-model="task.value">

        <button @click="addTask">Add task</button>
    </div>
</template>

<script>
export default {
    data () {
        return {
            task :{
                id: null,
                value: '',
                isChecked: false,
            }
        }
    },

    methods: {
        addTask () {
            if (!this.task.value) return

            const task = { ...this.task }

            task.id = Date.now().toString()

            this.$emit('add-task', task)

            this.task.id = null
            this.task.value = ''
        }
    }
}
</script>