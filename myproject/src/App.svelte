<script>
	// export let name;
	import { createEventDispatcher } from 'svelte';
  import { onMount } from 'svelte';
  import "../public/tailwind.css"

  let sidebarData = [
    { id: 1, title: 'Card 1', content: 'This is the content of Card 1.' },
    { id: 2, title: 'Card 2', content: 'This is the content of Card 2.' },
    { id: 3, title: 'Card 3', content: 'This is the content of Card 3.' }
  ];
  let searchQuery = '';
  let newCard = { title: '', content: '' };
  let showModal = false;

  const dispatch = createEventDispatcher();

  function search(event) {
    // Perform search logic here
    searchQuery=event.target.value;
   
  }

  function addCard() {
    showModal = true;
  
  }

  function saveCard() {
    sidebarData.push({ id: sidebarData.length + 1, title: newCard.title, content: newCard.content });
    newCard = { title: '', content: '' };
    localStorage.setItem("newCard", JSON.stringify(sidebarData));
    showModal = false;
  }
  if(localStorage.getItem("newCard")){
    sidebarData=JSON.parse(localStorage.getItem("newCard"));
  }

  function closeModal() {
    showModal = false;
  }

  onMount(() => {
    function handleKeyPress(e) {
      if (e.key === 'Enter') {
        search();
      }
    }
    window.addEventListener('keypress', handleKeyPress);
    return () => {
      window.removeEventListener('keypress', handleKeyPress);
    };
  });
</script>

<main>
	<div class="container flex flex-wrap">
  <div class="writing-space flex-grow"> 
    {#if showModal}
		<div class="modal" on:click="{closeModal}">

		  <div class="modal-content" on:click="{e => e.stopPropagation()}">
			<h2>Add New Card</h2>
			<input type="text" bind:value="{newCard.title}" placeholder="Title" />
			<textarea bind:value="{newCard.content}" placeholder="Content"></textarea><br>
			<button  on:click="{saveCard}">Save</button>
		  </div>
      
		</div>
	  {/if}
  </div>

	<div class="sidebar flex-grow">
		<input class="search" type="text" bind:value="{searchQuery}" placeholder="Search" />
    
  <ul>
    {#each sidebarData as card}
      {#if card.title.toLowerCase().includes(searchQuery)}
        <li>
          <div class="card">
            <h3>{card.title}</h3>
            <p>{card.content}</p>
          </div>
        </li>
      {/if}
    {/each}
  </ul>

		<!-- <button on:click="{search}">Search</button>
		<hr />
		{#each sidebarData as item} 
		  <div class="card">
			<h3>{item.title}</h3>
			<p>{item.content}</p>
		  </div>
		{/each} -->
		<button class="addcard" on:click="{addCard}">Add Card</button>
	  </div>
	
</main>

<style >
 

	main {
		/* text-align: center; */
		/* padding: 1em; */
		/* max-width: 240px; */
		margin: 0 auto;
   
    
	}
  .container{
    display: flex;
    background-color: rgb(172, 172, 209);
  }
  .writing-space{
    width: 70vw;
    height: 100vh;
  }
  
  .side-bar{
     width: 30vw;
    height: 30vh; 
    margin-left: 800px;
    margin-top: 10%;
    
  }
  .card{
    width: 30vw;
    height: 30vh;
    background-color:black; 
  }
  .addcard{
    width: 200px;
    position: relative;
    margin-left: 35%;  
  }
  .search{
    width: 200px;
    position: relative;
    margin-left: 35%;  
  }
  h3{
    color: beige;
  }
  p{
    color: rgb(175, 170, 170);
  }

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
      background-color: rgb(172, 172, 209);
		}
	}
	.card {
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .modal {
    /* position: fixed;  */
     /* top: 0;
    left: 0; */
    width: 75%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
  }


  .modal-content {
    background-color: bisque;
    /* padding: 20px; */
    /* border-radius: 4px; */
    position: relative;
    width: 200%;
    height: 100%;
    /* margin-left: 20%;
    margin-top: 20%; */ 
  }

</style>