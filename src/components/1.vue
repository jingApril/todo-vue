<template>
    <div>
        <input type="text" class="todo-input" placeholder="想要做的事情" v-model="newTodo" @keyup.enter="addTodo">
        <transition-group name="fade" enter-active-class="animate__animated animate__fadeInUp" leave-active-class="animate__animated animate__fadeOutDown">
            <todo-item v-for="todo in todosFiltered" :key="todo.id" :todo="todo" :checkAll = "!anyRemaining"></todo-item>
        </transition-group>
        <div class="extra-container">
            <todo-check-all></todo-check-all>
            <todo-items-remaining></todo-items-remaining>
        </div>
        <div class="extra-container">
            <todo-filter></todo-filter>
            <div>
                <transition name="fade">
                   <todo-clear-completed></todo-clear-completed>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
import TodoItem from "./TodoItem";
import TodoItemsRemaining from "./TodoItemsRemaining";
import TodoCheckAll from "./TodoCheckAll";
import TodoFilter from "./TodoFilter";
import TodoClearCompleted from "./TodoClearCompleted";

import 'animate.css'
    export default {
        name: 'todo-list',
        components: {
            TodoItem,
            TodoItemsRemaining,
            TodoCheckAll,
            TodoFilter,
            TodoClearCompleted
        },
        data() {
            return {
                newTodo:'',
                idForTodo: 3,
            }
        },
        computed:{
            // remaining(){
            //     return this.$store.getters.remaining
            // },
            anyRemaining(){
                return this.$store.getters.anyRemaining
            },
            todosFiltered(){
                return this.$store.getters.todosFiltered
            },
            // showClearCompletedButton(){
            //     return this.$store.getters.showClearCompletedButton
            // }
        },
        methods: {
            addTodo() {
                if(this.newTodo.trim().length == 0) {
                    return;
                }
                this.$store.dispatch('addTodo', {
                    id: this.idForTodo,
                    title: this.newTodo,
                })
                this.newTodo = '',
                this.idForTodo++;
            },

        }
    }
</script>

<style lang="scss">
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
        transition: opacity .7s;
    }
    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>