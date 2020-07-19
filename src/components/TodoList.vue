<template>
    <div>
        <input type="text" class="todo-input" placeholder="想要做的事情" v-model="newTodo" @keyup.enter="addTodo"> 
        <transition-group name="fade"
            enter-active-class="animate__animated animate__fadeInUp" 
            leave-active-class="animate__animated animate__fadeOutDown">

            <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id"  :todo="todo" :index="index" 
            @removeTodo="removeTodo" @editTodo="editTodo"
            class="todo-item"></todo-item>
        </transition-group>
        <div class="extra-container">
            <div>
                <label><input type="checkbox" :checked="!anyRemaining"
                    @change="checkAllTodos"> 全选</label>
            </div>
            <div>{{ remaining}} items left</div>
        </div>
        <div class="extra-container">
            <div>
                <button :class="{active : filter == 'all'}" @click="filter = 'all'">all</button>
                <button :class="{active : filter == 'active'}" @click="filter = 'active'">Active</button>
                <button :class="{active : filter == 'completed'}" @click="filter = 'completed'">Completed</button>
            </div>
            <div>
                <transition name="fade">
                    <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
import TodoItem from "./TodoItem";
import 'animate.css'
    export default {
        name: 'todo-list',
        components: {
            TodoItem,
        },
        data() {
            return {
                newTodo:'',
                idForTodo: 3,
                beforeEditCachev: '',
                filter:'all',
                todos: [
                    {
                        "id": 1,
                        "title": '完成Vue to-do 学习',
                        "completed": false,
                        "editing": false,
                    },
                    {
                        "id": 2,
                        "title": '看完柔福公主',
                        "completed": false,
                        "editing": false,
                    },

                ]
            }
        },
        directives:{
            focus: {
                inserted: function(el) {
                    el.focus()
                }
            }
        },
        computed:{
            remaining(){
                return this.todos.filter( todo => !todo.completed).length
            },
            anyRemaining(){
                return this.remaining !== 0
            },
            todosFiltered(){
                if(this.filter == 'all') {
                    return this.todos
                } else if (this.filter == 'active') {
                    return this.todos.filter((todo) => !todo.completed)
                } else if (this.filter == 'completed') {
                    return this.todos.filter((todo) => todo.completed)
                }
                return this.todos
            },
            showClearCompletedButton(){
                return this.todos.filter(todo => todo.completed).length > 0
            }
        },
        methods: {
            addTodo() {
                if(this.newTodo.trim().length == 0) {
                    return
                }

                this.todos.push({
                    id: this.idForTodo,
                    title: this.newTodo,
                    completed: false,
                })

                this.newTodo = '',
                this.idForTodo++;
            },
            editTodo(todo){
                this.beforeEditCache = todo.title;
                todo.editing = true;
            },
            doneEdit(todo) {
                 if(todo.title.trim() == '') {
                    todo.title = this.beforeEditCache
                }
                todo.editing = false;
            },
            cancelEdit(todo) {
                todo.title = this.beforeEditCache
                todo.editing = false;
            },
            removeTodo(index) {
                this.todos.splice(index, 1)
            },
            checkAllTodos(){
                this.todos.forEach((todo) => todo.completed = event.target.checked)
                console.log(event.target.checked);
                // 
            },
            clearCompleted(){
                  this.todos = this.todos.filter( todo => !todo.completed)
            }
        }
    }
</script>

<style lang="scss">


// @import url('https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.0.0/animate.min.css');
    .todo-input {
        width:100%;
        padding:10px 18px;
        font-size:18px;
        margin-bottom: 16px;
        &:focus {
            outline: 0;
        }
    }
    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .remove-item {
        cursor: pointer;
        margin-left: 14px;
        &:hover {
            color: black;
        }
    }
    .todo-item-left {
        display: flex;
        align-items: center;
    }
    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }
    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solod #ccc;
        font-family:'微软雅黑';

        &:focus {
            outline: none;
        }
    }
    .completed {
        text-decoration: line-through;
        color: grey;
    }
    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgray;
        padding-top: 14px;
        margin-bottom: 14px;
    }
    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
        border: 1px solid #ccc;
        outline: none;
        margin-right: 10px;

        &:hover {
            background: lightgreen;
        }
        &:focus {
            outline: none;
        }
    }
    .active {
        background: lightgreen;
    }
    .fade-enter-active, .fade-leave-active {
        transition: opacity 2s;
    }
    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>