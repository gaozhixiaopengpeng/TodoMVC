<template>
    <div>
        <BaseInputText
            v-model="newTodoText"
            placeholder="New todo"
            @keydown.enter="addTodo"
            v-focus
        ></BaseInputText>
        <ul v-if="todos.length">
            <TodoItem
                v-for="(todo, index) in todos" 
                :key="index"
                :todo="todo"
                :index="index"
                @remove="removeTodo"
            />
        </ul>
        <p v-else>Nothing left in the list. Add a new todo in the input above.</p>
    </div>
</template>

<script>
import BaseInputText from './BaseInputText'
import TodoItem from './TodoItem';

let todoList = localStorage.getItem('todoList');

if (!todoList) {
    todoList = [];
} else {
    todoList = JSON.parse(todoList);
}

export default {
    components: {
        BaseInputText,
        TodoItem
    },
    data () {
        return {
            newTodoText: "",
            todos: todoList
        }
    },
    methods: {
        removeTodo (index) {
            this.todos.splice(index, 1)
        },
        addTodo () {
            const trimmedText = this.newTodoText.trim();
            if (trimmedText) {
                this.todos.push({
                    id: this.todos.length+1,
                    text: trimmedText
                })
                this.newTodoText = ""
            } 
        }
    },
    watch: {
        todos: {
            deep: true,
            handler: function () {
                localStorage.setItem('todoList', JSON.stringify(this.todos));
            }
        }
    },
    directives: {
        focus: {
            inserted: function (el) {
                el.focus();
            }
        }
    }
}
</script>

<style lang="scss">
    
</style>
