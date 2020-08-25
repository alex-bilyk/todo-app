<template>
    <div class="home">
        <img alt="Vue logo" src="../assets/logo.png" />

        <h2>ToDo</h2>

        <router-link to="/create-todo">Create Todo</router-link>

        <div v-if="isShowedNotification">
            <div>Are you sure?</div>

            <button @click="removeTodo">Yes</button>
            <button @click="isShowedNotification = false; removingTodo = null">No</button>
        </div>

        <div
            v-for="todo in todos"
            :key="todo.id"
        >
            <div>
                <h3>{{ todo.title }}</h3>

                <router-link :to="`/edit/${todo.id}`">Edit Todo</router-link>

                <button @click="isShowedNotification = true; removingTodo = todo">Remove Todo</button>
            </div>

            <ul>
                <li
                    v-for="task in todo.tasksList.filter((_, index) => index < 3)"
                    :key="task.id"
                >
                    <input
                        type="checkbox"
                        :checked="task.isChecked"
                        disabled
                    />

                    {{ task.value }}
                </li>
            </ul>

            <div v-if="todo.tasksList.length > 3">Tasks more than 3...</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Home',

    data () {
        return {
            todos: JSON.parse(localStorage.getItem('todos')) || [],
            isShowedNotification: false,
            removingTodo: null
        }
    },

    methods: {
        removeTodo () {
            const indexTodo = this.todos.findIndex(item => item.id === this.removingTodo.id)

            this.todos.splice(indexTodo, 1)

            this.isShowedNotification = false

            localStorage.setItem('todos', JSON.stringify(this.todos))
        }
    }
}
</script>