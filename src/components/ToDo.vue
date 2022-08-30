<template>
    <div @click="toggleTodo()" class="todoItem"
     :class="{'done': this.todo.done}">
        <h2>{{ todo.todoName }}</h2>
        <button @click="deleteTodo"><img src="../assets/bin.png" alt="Löschen"></button>
    </div>
</template>

<script>
export default {
    name: "ToDo",
    props: ["todoprop", "todoindex"],
    data() {
        return {
            todo: this.todoprop,
        }
    },
    methods: {
        toggleTodo() {
            this.$emit("toggledone-index", this.todoindex);
        },
        deleteTodo() {
            this.$emit("deletetodo-index", this.todoindex);
        }
    },
}
</script>

<style>
.todoItem {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 15px auto;
    margin-bottom: 25px;
    padding: 25px;
    background-color: rgb(37, 37, 37);
    border-radius: 10px;
    box-shadow: 0 10px rgb(15, 15, 15);

    cursor: pointer;

    user-select: none;
}

.todoItem button {
    all: unset;

    background-color: rgba(0, 0, 0, 0.25);
    color: white;
    scale: 1/1;
    padding: 10px;
    border-radius: 5px;
    transition-duration: 150ms;
}

.todoItem button:hover {
    background-color: rgba(0, 0, 0, 0.5);
}

.todoItem img {
    width: 50px;
}

.done {
    background-color: var(--active);
    color: black;
    box-shadow: 0 10px rgb(35, 94, 70);
}

</style>