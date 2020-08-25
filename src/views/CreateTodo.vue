<template>
    <div class="about">
        <h1>Create Todo</h1>

        <div>
            <div>
                <div>Todo Title</div>
                <input type="text" placeholder="Weekday" v-model="todo.title">
            </div>

            <ul
                v-for="item in todo.tasksList"
                :key="item.id"
            >
                <li>
                    {{ item.value }}
                </li>
            </ul>

            <CreateTaskComponent
                @add-task="todo.tasksList.push($event)"
            />
        </div>

        <button @click="save">Save</button>
    </div>
</template>

<script>
import CreateTaskComponent from '../components/CreateTaskComponent'

export default {
    components: {
        CreateTaskComponent
    },

    data () {
        return {
            todo: {
                id: Date.now().toString(),
                title: '',
                tasksList: []
            }
        }
    },

    methods: {
        save () {
            if (!(this.todo.title && this.todo.tasksList.length)) return

            const todos = JSON.parse(localStorage.getItem('todos')) || []

            todos.push(this.todo)

            localStorage.setItem('todos', JSON.stringify(todos))

            this.todo.id = Date.now().toString()
            this.todo.title = ''
            this.todo.tasksList = []

            this.$router.push({ path: '/' })
        }
    }
}
</script>