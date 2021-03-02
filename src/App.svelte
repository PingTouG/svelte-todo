<script lang="ts">
  type Todo = {
    id: number
    content: string
    status: boolean
  }

  let todoList: Array<Todo> = []
  let value: string = ''
  let input

  function onAdd() {
    if (!value) {
      alert('请输入待办事项')
      return false
    }

    const isExist =
      todoList.findIndex((item: Todo) => item.content === value) > -1

    if (isExist) {
      alert('要添加的待办事项已存在')
      return false
    }

    const todo: Todo = {
      id: Date.now(),
      content: value,
      status: false,
    }

    todoList = [todo, ...todoList]
    value = ''
    input.focus()
  }

  function onStatusChange(target: Todo) {
    let status = true
    if (target.status) {
      status = confirm('确定已经完成了此待办事项？')
    }

    if (!status) {
      target.status = false
      todoList = todoList.map((item: Todo) =>
        item.id === target.id ? target : item
      )
    }
  }
</script>

<main>
  <h1 class="title">Todo List</h1>
  <p class="header">
    <input class="input" type="text" bind:this={input} bind:value />
    <button class="button" on:click={onAdd}>添加</button>
  </p>
  <ul class="list">
    {#each todoList as item (item.id)}
      <li class="item">
        <input
          class="checkbox"
          id={item.id.toString()}
          type="checkbox"
          bind:checked={item.status}
          on:change={() => onStatusChange(item)}
        />
        <label
          class="content"
          class:active={item.status}
          for={item.id.toString()}>{item.content}</label
        >
      </li>
    {/each}
  </ul>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 480px;
    margin: 0 auto;
  }

  .header {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .title {
    color: #ff3e00;
    text-transform: uppercase;
    font-weight: 500;
  }

  .input {
    height: 32px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .button {
    height: 32px;
    width: 72px;
    border-radius: 4px;
    border: none;
    outline: none;
    background-color: #ff3e00;
    color: #fff;
    font-size: 14px;
    margin-left: 12px;
  }

  .list {
    list-style: none;
    max-width: 480px;
    margin: 0 auto;
  }

  .item {
    display: flex;
    align-items: center;
  }

  .checkbox {
    margin: 0;
  }

  .content {
    margin-left: 8px;
    font-size: 18px;
    font-weight: 700;
  }

  .active {
    text-decoration: line-through;
    color: #999;
  }

  @media (min-width: 640px) {
    main .list {
      max-width: none;
    }
  }
</style>
