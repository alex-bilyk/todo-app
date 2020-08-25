<template>
    <li>
        <input
            type="checkbox"
            v-model="task.isChecked"
            :value="task.isChecked"
        >

        <span v-if="!isEdited">{{ task.value }}</span>

        <input
            v-else
            type="text"
            v-model="task.value"
        />

        <button
            v-if="!isEdited"
            @click="isEdited = !isEdited"
        >
            Edit text
        </button>

        <button
            v-else
            @click="saveValue"
        >
            Save changes
        </button>

        <button @click="removeTask">Remove task</button>
    </li>
</template>

<script>
export default {
    props: [
        'task'
    ],

    data () {
        return {
            isEdited: false
        }
    },

    methods: {
        saveValue () {
            this.isEdited = false

            this.$emit('update-task', this.task)
        },

        removeTask () {
            this.$emit('remove-task', true)
        },
    }
}
</script>