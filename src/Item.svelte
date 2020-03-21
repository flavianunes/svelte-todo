<style>
    .todo-item {
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 0.3s;
    }

    .remove-item {
        cursor: pointer;
        margin-left: 15px;
    }

    .remove-item:hover {
        color: lightseagreen;
    }

    .todo-item-right {
        margin-left: 15px;
    }

    .todo-item-left {
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        border: 1px solid white;
        margin-left: 12px;
    }

    .completed {
        text-decoration: line-through;
        color: #333;
    }

    .todo-item-category {
        color: #333;
        padding: .2em .5em;
    }

    .work {
        background-color: #e7e3a2;
    }

    .personal {
        background-color: #bcabca;
    }

    .other {
        background-color: #abcabc;
    }

    :global(.container-checkbox) {
        display: block;
        position: relative;
        padding-left: 35px;
        margin-bottom: 12px;
        cursor: pointer;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }

    :global(.container-checkbox input) {
        position: absolute;
        opacity: 0;
        cursor: pointer;
        height: 0;
        width: 0;
    }

    :global(.checkmark) {
        position: absolute;
        top: 0;
        left: 0;
        height: 20px;
        width: 20px;
        background-color: #eee;
    }

    :global(.container-checkbox:hover input~.checkmark) {
        background-color: #ccc;
    }


    :global(.container-checkbox input:checked~.checkmark) {
        background-color: lightgreen;
    }


    :global(.checkmark:after) {
        content: "";
        position: absolute;
        display: none;
    }


    :global(.container-checkbox input:checked~.checkmark:after) {
        display: block;
    }

    :global(.container-checkbox .checkmark:after) {
        left: 5px;
        top: 2px;
        width: 5px;
        height: 10px;
        border: solid white;
        border-width: 0 3px 3px 0;
        -webkit-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
        transform: rotate(45deg);
    }
</style>


<script>
    import {
        createEventDispatcher
    } from 'svelte';
    import {
        fly
    } from 'svelte/transition';
    export let id;
    export let title;
    export let category;
    export let completed;
    const dispatch = createEventDispatcher();

    function deleteTodo() {
        dispatch('deleteTodo', {
            id: id
        });
    }

    function toggleComplete() {
        dispatch('toggleComplete', {
            id: id
        });
    }
</script>



<li class="todo-item">
    <div class="todo-item-left" transition:fly="{{ y: 20, duration: 300 }}">
        <label class="container-checkbox">
            <span class="todo-item-label" class:completed>{title}</span>
            <input type="checkbox" bind:checked={completed} on:change={toggleComplete}>
            <span class="checkmark"></span>
        </label>
    </div>
    <div class="todo-item-right">
        <span class="todo-item-category {category}">{category}</span>
        <span class="remove-item" on:click={deleteTodo}>
            ❌
        </span>
    </div>

</li>