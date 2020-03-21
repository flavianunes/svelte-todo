<style>
    @import url('https://fonts.googleapis.com/css?family=Bellota&display=swap');
    * {
        font-family: 'Bellota', cursive;
    }

    header {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .container {
        max-width: 800px;
        margin: 0 auto;
    }
    .new-todo {
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 0.3s;
    } 
    .new-todo .left {
        background-color: moccasin;
        padding: .5em .8em;
    }
    .new-todo .left label {
        display : inline;
        padding-right : 4px;
    }


    .new-todo .left select {
        background-color: moccasin;
        border: none;
        margin: 0;
        padding: 0;
        color: #333;    
    }

    .new-todo .left select:focus {
        outline: none;
    }

    .todo-list {
        padding-left: 0;
    }

    .todo-input {
        width: 100%;
        padding: 10px, 20px;
        font-size: 18px;
        margin-bottom: 20px;
        border: none;
        border-bottom: 2px solid moccasin;
    }

    .todo-input:focus {
        outline: none;
        background-color: moccasin;
    }
    .inner-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 15px;
        margin-bottom: 13px;
    }
    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
        background-color: #eee;
        outline: none;
        border: none;
        padding: .5em .7em;
    }
    button:hover {
        cursor: pointer;
        background-color: #ccc;
    }
    button:focus {
        outline: none;
    }
    .active {
        background: lightgreen;
    }
    .active:hover {
        cursor: not-allowed;
        background: lightgreen;
    }
    .clear {
        background-color: moccasin;
    }
    .clear:hover {
        background-color: darksalmon;
    }
    
</style>

<script>


    import Item from './Item.svelte';
    import { time } from './stores.js';


    let newTodoTitle = '';
    let newTodoCategory = 'work';
    let currentFilter = 'all';
    let nextId = 4;
    let today = new Date().toLocaleDateString();
    let formatter = new Intl.DateTimeFormat('en', {
		hour12: true,
		hour: 'numeric',
		minute: '2-digit',
		second: '2-digit'
	});

    
    let todos = [
        {
            id: 1,
            title: 'Add your tasks here',
            category: 'work',
            completed: false
        },
        {
            id: 2,
            title: 'This is an example',
            category: 'personal',
            completed: true
        },
        {    id: 3,
            title: 'Choose from categories: work, personal & other',
            category: 'other',
            completed: false
        }

    ]; 

    let categories = [
        {
            id: 'work',
            title: 'Work',
            color: 'blue'
        }
    ];

    //computed properties
    $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;

    $: filteredTodos = currentFilter === 'all' ? todos : currentFilter === 'work' 
    ? todos.filter(todo => todo.category === 'work') : currentFilter === 'personal' 
    ? todos.filter(todo => todo.category === 'personal')
    : todos.filter(todo => todo.category === 'other')


    function addTodo(event) {
        if(event.key === 'Enter') {
            todos = [...todos, {
                id: nextId,
                title: newTodoTitle,
                category: newTodoCategory,
                completed: false
            }];

            newTodoTitle = '';
            newTodoCategory = 'work';
            nextId+=1;
        }
    }

    function checkAllTodos(event) {
        todos.forEach(todo => todo.completed = event.target.checked);
        todos = todos;
    }

    function updateFilter(newFilter) {
        currentFilter = newFilter;
    }

    function clearCompleted() {
        todos = todos.filter(todo => !todo.completed);
    }

    function handleDeleteTodo(event) {
        todos = todos.filter(todo => todo.id !== event.detail.id);
    }

    function handleToggleComplete(event) {
        const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
        const updatedTodo = { ...todos[todoIndex], completed: !todos[todoIndex].completed};
        todos = [
            ...todos.slice(0, todoIndex),
            updatedTodo,
            ...todos.slice(todoIndex + 1),
        ];
    }


</script>

<div class="container">
    <header>
        <h1>Fluvixx's Todo 📝</h1>
        <h2>{today} - {formatter.format($time)}</h2>
    </header>

    <div class="new-todo">
        <div class="left">
            <label for="category">Choose a category:</label>

            <select id="category" bind:value={newTodoCategory}>
                <option value="work">Work</option>
                <option value="personal">Personal</option>
                <option value="other">Other</option>
            </select>
        </div>
        <div class="right">
            <input type="text" class="todo-input" placeholder="What do you need to do? 📝" bind:value={newTodoTitle} on:keydown={addTodo}>
        </div>
    </div>

    <ul class="todo-list">
        {#each filteredTodos as todo}
            <Item {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} />
        {/each}
    </ul>

    <div class="inner-container">
        <label class="container-checkbox">
            <span>Complete them all ✔️</span>
            <input type="checkbox" on:change={checkAllTodos}>
            <span class="checkmark"></span>
        </label>
        <span>{todosRemaining} items left </span>
    </div>

    <div class="inner-container">
        <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}"> All </button> 
        <button on:click={() => updateFilter('work')} class:active="{currentFilter === 'work'}"> Work </button> 
        <button on:click={() => updateFilter('personal')} class:active="{currentFilter === 'personal'}">Personal</button> 
        <button on:click={() => updateFilter('other')} class:active="{currentFilter === 'other'}">Other</button> 
        <button on:click={clearCompleted} class="clear">Clear Completed 🧽</button>
        <button on:click={()=> console.log('todo')} class="clear">Customize categories 🖊️</button>
    </div>
</div>





<!-- https://svelte.dev/repl/033e824fad0a4e34907666e7196caec4?version=3.4.1 -->
