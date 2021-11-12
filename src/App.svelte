<script>
import { writable } from 'svelte/store';
import {shortcut} from './shortcut.js';
//  writable([]);
let itemText = "";
let searchText = 'Not Entered Yet';
// let searchResult = ["Kotlin List", "Java List", "Svelte List"];
let searchResult = writable([]);
let searchResultData = [];
searchResult.subscribe(value => {
	searchResultData = value;
});
let outerList = ["Kotlin List", "Java List", "Svelte List"];
let outerCollapseMap = {"Kotlin List": false, "Java List": false, "Svelte List": false};
let curolname = "Kotlin List"; 
let outerListStore = writable(outerList);
let lldata = {
	"Kotlin List": ["Kotlin Js", "classes", "Anroid", "examples"],
	"Java List": ["OPPs", "collections", "spring boot"],
	"Svelte List": ["Hello world", "Dyanmic attributes",
		"styling", "props", "events", "store"
	],
};
let buttonCount = 0;

const itemTextKeyHandler = e => {
	console.log("inside item text key handler");
	if(e.charCode === 13){
		//add itemText append 
		if(curolname in lldata){
			lldata[curolname].push(itemText);
			$outerListStore = [... $outerListStore]; // todo force to refresh
		}
		itemText = "";
	}
};
const addOuterList = () => {
	const name = prompt("enter outer list name");
	const li = outerList.indexOf(name);
	if(li >=0){
		alert(`duplicate outer list name: $name please try again`);
		return;
	}
	outerList.push(name);
	lldata[name] = [];
	console.log("lldata:", lldata, "; outerList:", outerList);
};

const searchHelper = (searchText) => {
	if(!searchText || searchText.trim() == "")
		return;
	// console.log("inside search helper st:", searchText);
	const result = outerList.filter(name => {
		
		// console.log("name:", name); 
		return name.toLocaleLowerCase().includes(searchText.toLocaleLowerCase())
	});
	// console.log("serach result found here: ", result);
	$searchResult = [ ...result];
};
const onSearchHandler = () => { 
	console.log("onsearch handler");
	buttonCount = buttonCount + 1;
	searchText= prompt("Please enter your name", "Harry Potter");
	searchHelper(searchText);
	console.log("search result: ", searchResult);
};
const toggleCollapse = (name) => {
	if(name in outerCollapseMap)
		outerCollapseMap[name] = ! outerCollapseMap[name];
};
const getCollapseText = (name) => {
	if(name in outerCollapseMap){
		return outerCollapseMap[name] ? '+': '-';
	}else{
		return "";
	}
}
</script>

<main >
  <div class="debug_container">
    buttonCount {buttonCount}
   </div>
   <button use:shortcut={{shift:true, code: 'Digit1'}} on:click={onSearchHandler}>
	Search	
	</button>
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
<button on:click={addOuterList}>Add Outer List</button><br/>
<input type="text" class="item_text" 
	placeholder="inner list's item" 
	bind:value={itemText}
	on:keypress={itemTextKeyHandler}
/> 
<ul>
	{#each $outerListStore as outerListName}
	<li class="outer_list">
		{#if curolname == outerListName}
		<span class="current_outer_list"> Current</span>
		{/if}
		<span class="collapse" on:click={toggleCollapse(outerListName)}>
			{getCollapseText(outerListName)}
		</span>
		<span class="outer_list_name" on:click={() => curolname=outerListName}>
			{outerListName}
		</span>
		{#if outerListName in outerCollapseMap && !outerCollapseMap[outerListName]}
		<ul>
			{#each lldata[outerListName] as item}
			<li>{item}</li>
			{/each}
		</ul>
		{/if}
	</li>
	{/each}
</ul>
</main>

<style>
li{
	list-style-type: none;
}
.outer_list {
	margin-bottom: 20px;
}
.current_outer_list {
 background-color: greenyellow;
}
.collapse{
 background-color: goldenrod;
 padding:5px;
 color: white;
}
</style>