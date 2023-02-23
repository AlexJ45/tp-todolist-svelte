<script lang="ts">
  import { text } from "svelte/internal";

    interface Task {
      id: number;
      name: string;
      status: boolean;
    }
    let tasks: Task[] = [
      {
        id: 1,
        name: 'Courir',
        status: true,
      },
      {
        id: 2,
        name: 'Boire',
        status: false,
      },
      {
        id: 3,
        name: 'Dormir',
        status: false,
      }
    ]
    $: tasksLeftCount = tasks.filter(task => task.status == false);
    function checkAll(){
      tasks = tasks.map(task => ({...task, status: true}))
    }
    function add(new_task) {
      tasks = tasks.concat({id: Date.now(),
        name: new_task, status: false});
	  }
    </script>
    
    <main>
      <form action=''>
        <input type='text' name='new_task'>
        <button type="submit" on:click={add(input.value)}>Ajouter</button>
      </form>
      
      {#each tasks as task}
        <ul>
          <input type='checkbox' name="{task.name}" bind:checked={task.status}>
          <label for="{task.name}">{task.name}</label>
        </ul>
      {/each}
      <button on:click={checkAll} disabled ={tasksLeftCount.length === 0}>Tout Compléter</button>
      {#if tasksLeftCount.length === 0}
      Félicitation, c'est terminé !
      {:else}
      {tasksLeftCount.length} tâche(s) restante(s)
      {/if}
    </main>
    
    <style>
    input:checked ~ label{text-decoration:line-through;}
    </style>