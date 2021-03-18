<script>
	import { onMount } from 'svelte';

	//let data = [
  //{"type": "breakfast", "regularity": "weekly", "name": "National Community Church", "quadrant": "NW", "ward": 3, "days":["Mon", "Tues", "Wed", "Thurs", "Fri", "Sat", "Sun"]},
  //{"type": "breakfast", "regularity": "1st and 3rd Sat of the month", "name": "Miriam's Kitchen (Breakfast)", "quadrant": "SE", "ward": 4, "days":["Mon", "Tues", "Wed", "Thurs", "Fri"]},
  //{"type": "dinner", "regularity": "2nd Tue of the month", "name": "Miriam's Kitchen (Dinner)", "quadrant": "SE", "ward": 6, "days":["Mon", "Wed", "Fri"]},
  //{"type": "groceries", "regularity": "weekly", "name": "Spanish Catholic Center", "quadrant": "SW", "ward": 1,  "days":["Fri"]}
	//]
	let displayData = [];
	let JSONifiedData;
	onMount(async () => {
		console.log("HERE")
		const res = await fetch('https://spreadsheets.google.com/feeds/list/1FF3R6I3sbzWukdiBP9U6XYO1qXthMer8Lz0GObAyKsc/od6/public/values?alt=json');
		JSONifiedData = await res.json();
		displayData = JSONifiedData.feed.entry.map(entry => {
			return { 
				name: entry.gsx$name.$t,
				type: entry.gsx$type.$t,
				start: entry.gsx$start.$t,
				end: entry.gsx$end.$t,
				regularity: entry.gsx$regularity.$t,
				quadrant: entry.gsx$quadrant.$t,
				ward: entry.gsx$ward.$t,
				region: entry.gsx$region.$t,
				location: entry.gsx$location.$t,
				restrictions: entry.gsx$restrictions.$t,
				eligibilitylimitations: entry.gsx$eligibilitylimitations.$t,
				website: entry.gsx$website.$t,
				contact: entry.gsx$contact.$t,
				contact2: entry.gsx$contact2.$t,
				//notes: entry.gsx$notese.gquality.$t
			}
		});
	});
  
	
	displayData = displayData;
  
let types = ['all'];
let regularity = ['all'];
let quadrants = ['all'];
let wards = ['all'];
let days = ['all'];

let typeOptions = [
	'all',
  'groceries',
  'dinner',
  'breakfast',
	'delivery'
];

const regularityOptions = [
	"all",
  "weekly",
  "biweekly",
  "1st and 3rd Sat of the month",
  "2nd Tue of the month",
  "2nd and 4th Wed of the month",
]; 

const quadrantOptions = [
	"all",
  "NW",
  "NE",
  "SE",
  "SW"
];
	
const wardOptions = [
	"all",
	1,
	2,
	3,
	4,
	6
]

const dayOptions =[
	"all",
	"Mon",
	"Tues",
	"Wed",
	"Thurs",
	"Fri",
	"Sat",
	"Sun"
]

function join(types, regularity, quadrants, wards, days) {
    displayData = data
      .filter(item => {
								if(types.includes('all')){return item}
                if(types.includes(item.type)){return item}
    		})
			.filter(item => {
								if(regularity.includes('all')){return item}
                if(regularity.includes(item.regularity)){return item}
		})
		.filter(item => {
								if(quadrants.includes('all')){return item}
                if(quadrants.includes(item.quadrant)){return item}
		})
	.filter(item => {
								if(wards.includes('all')){return item}
                if(wards.includes(item.ward)){return item}
		})
	.filter(item => {
					const sharedDates = days.map(day => item.days.find(d => d === day))
								if(days.includes('all')){return item}
								// fix this hack
                if(sharedDates[0] !== undefined){return item}
		})
}

</script>


<h2>Resources</h2>

<select multiple bind:value={types} on:click={join(types, regularity, quadrants, wards, days)}>
{#each typeOptions as type}
  <option selected value={type}>
    {type}
  </option>
{/each}
</select>

<select multiple bind:value={regularity} on:click={join(types, regularity, quadrants, wards, days)}>
{#each regularityOptions as time}
  <option selected value={time}>
    {time}
  </option>
{/each}
</select>

<select multiple bind:value={days} on:click={join(types, regularity, quadrants, wards, days)}>
{#each dayOptions as day}
  <option value={day}>
    {day}
  </option>
{/each}
</select>

<select multiple bind:value={quadrants} on:click={join(types, regularity, quadrants, wards, days)}>
{#each quadrantOptions as quad}
  <option value={quad}>
    {quad}
  </option>
{/each}
</select>

<select multiple bind:value={wards} on:click={join(types, regularity, quadrants, wards, days)}>
{#each wardOptions as ward}
  <option value={ward}>
    {ward}
  </option>
{/each}
</select>

<br>
<h1>
	Results
</h1>
<h3>{displayData.length ? displayData[0].name: "something else"}</h3>
<ul>
	{#each displayData as datum}
	<li>{datum.name}</li>
	<p>{datum.type}</p>
	<p>{datum.regularity}</p>
<!-- 		{#each datum.dates as date} -->
	<p>{datum.days}</p>
<!-- 		{/each} -->
	<p>{datum.quadrant} DC</p>
	<p>Ward {datum.ward}</p>
{/each}
</ul>

