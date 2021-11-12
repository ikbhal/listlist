<script>
import { writable } from 'svelte/store';
import {shortcut} from './shortcut.js';
//  writable([]);
let searchText = 'Not Entered Yet';
// let searchResult = ["Kotlin List", "Java List", "Svelte List"];
let searchResult = writable([]);
let searchResultData = [];
searchResult.subscribe(value => {
	searchResultData = value;
});
let buttonCount = 0;
const onSearchHandler = () => {
	console.log("onsearch handler");
	buttonCount = buttonCount + 1;
	searchText= prompt("Please enter your name", "Harry Potter");
	if(searchText == 'list'){
		$searchResult = ['Kotlin List', "Java List"]
	}else {
		$searchResult = [];
	}
	console.log("search result: ", searchResult);
};
</script>

<main use:shortcut={{shift: true, code: 'Digit1'}} on:click={onSearchHandler}>
  <div class="debug_container">
    buttonCount {buttonCount}
   </div>
   <div>
	   Search Text is : {searchText}
   </div>
   	{#if searchResultData && searchResultData.length >0  }
		<div class="searchList" >
		   <h3>Search List</h3>
			{#each $searchResult as item}
				<div class="searchItem">
					{item}
				</div>
			{/each}
	   </div>
	{:else}
	   <div class="search-result-empty">
		   No data found for search
	   </div>
	{/if}
<h1>List of List</h1>
<p>simplified version of workflowy.com or basic slack</p>
<ul>
	<li>
		Kotlin List
		<ul>
			<li>kotlin js</li>
			<li>kotliin classes</li>
			<li>Kotlin collections</li>
			<li>Kotlin Android</li>
			<li>kotlin examples</li>
			<li>Kotlin kones</li>
			<li>Kotlin ktor</li>
			<li>Kotlin kmongo</li>
		</ul>
	</li>
	<li>Java List
		<ul>
			<li>Java OOPS</li>
			<li>Java collections</li>
			<li>Java spring boot</li>
		</ul>
	</li>
	<li>Svelte List
		<ul>
			<li>Svelte hello world</li>
			<li>Dynamic Attributes</li>
			<li>Styling</li>
			<li>nested components</li>
			<li>Html tags</li>
			<li>Reactivity</li>
			<li>Props</li>
			<li>Logic</li>
			<li>Events</li>
			<li>Lifecycle</li>
			<li>Store</li>
		</ul>
	</li>
</ul>
</main>

<style>
</style>