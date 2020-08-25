<template>
    <div>
        <h1>Edit Todo</h1>

        <div v-if="todo">
            <div>
                <div v-if="isChangingTitle">
                    <input type="text" v-model="todo.title">
                    <button @click="isChangingTitle = false">Save</button>
                </div>

                <div v-else>
                    <div>{{ todo.title }}</div>
                    <button @click="isChangingTitle = true">Edit</button>
                </div>
            </div>


            <ul>
                <EditTaskComponent
                    v-for="(task, index) in todo.tasksList"
                    :key="task.id"
                    :task="task"
                    @update-task="task = $event"
                    @remove-task="removeTask(index)"
                />
            </ul>
        </div>

        <CreateTaskComponent
            @add-task="todo.tasksList.push($event)"
        />

        <button
            @click="save"
            :disabled="!todos.length"
        >
            Save
        </button>

        <button @click="isShowedNotification = true">Remove Todo</button>

        <div v-if="isShowedNotification">
            <div>Are you sure?</div>

            <button @click="removeTodo">Yes</button>
            <button @click="isShowedNotification = false">No</button>
        </div>
    </div>
</template>

<script>
import EditTaskComponent from '../components/EditTaskComponent'
import CreateTaskComponent from '../components/CreateTaskComponent'

export default {
    components: {
        EditTaskComponent,
        CreateTaskComponent
    },

    data () {
        return {
            todos: JSON.parse(localStorage.getItem('todos')) || [],
            todo: null,
            isShowedNotification: false,
            isChangingTitle: false,
        }
    },

    created () {
        if (this.todos.length) this.getTodo(this.$route.params.id)
    },

    methods: {
        getTodo (id) {
            const todo = this.todos.find((item) => item.id === id)

            this.todo = todo
        },

        removeTodo () {
            const indexTodo = this.todos.findIndex(item => item.id === this.todo.id)

            this.todos.splice(indexTodo, 1)

            localStorage.setItem('todos', JSON.stringify(this.todos))

            this.$router.push({ path: '/' })
        },

        removeTask (index) {
            this.todo.tasksList.splice(index, 1)
        },

        save () {
            const todos = this.todos.map(todo => todo.id === this.todo.id ? this.todo : todo)

            this.todos = todos

            localStorage.setItem('todos', JSON.stringify(todos))

            this.$router.push({ path: '/' })
        },
    }
}
</script>