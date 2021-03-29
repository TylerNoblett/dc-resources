<script>
	import { onMount } from 'svelte';

	//let data = [
  //{"type": "breakfast", "regularity": "weekly", "name": "National Community Church", "quadrant": "NW", "ward": 3, "days":["Mon", "Tues", "Wed", "Thurs", "Fri", "Sat", "Sun"]},
  //{"type": "breakfast", "regularity": "1st and 3rd Sat of the month", "name": "Miriam's Kitchen (Breakfast)", "quadrant": "SE", "ward": 4, "days":["Mon", "Tues", "Wed", "Thurs", "Fri"]},
  //{"type": "dinner", "regularity": "2nd Tue of the month", "name": "Miriam's Kitchen (Dinner)", "quadrant": "SE", "ward": 6, "days":["Mon", "Wed", "Fri"]},
  //{"type": "groceries", "regularity": "weekly", "name": "Spanish Catholic Center", "quadrant": "SW", "ward": 1,  "days":["Fri"]}
	//]
	let data = [];
	let displayData = [];
	let JSONifiedData;

	const dayMap ={
				'sun': "Sun",
				'm': "Mon", 
				't': "Tues",
				'w': "Wed", 
				'th': "Thurs",
				'f': "Fri",
				'sat': "Sat"
	}
	const dayKeys = Object.keys(dayMap);
	const baseName = "gsx$";

	function capitalizeFirstLetter(string) {
  return string.charAt(0).toUpperCase() + string.slice(1);
}

	onMount(async () => {
		const res = await fetch('https://spreadsheets.google.com/feeds/list/1e022V6e8t_YLS_6tkaDjSQMihlt3nGu1flr887wIBWg/od6/public/values?alt=json');
		JSONifiedData = await res.json();
		data = JSONifiedData.feed.entry.map(entry => {
			//caculate days
			
			let entryDays = dayKeys.map(day => {
				const att = baseName + day
				if (!!entry[att].$t) { 
					return dayMap[day] 
				}
			})

			

			entryDays = entryDays.filter(day => day)

			return { 
				name: entry.gsx$name.$t,
				type: entry.gsx$type.$t,
				days: entryDays,
				start: entry.gsx$start.$t,
				end: entry.gsx$end.$t,
				regularity: capitalizeFirstLetter(entry.gsx$regularity.$t),
				quadrant: entry.gsx$quadrant.$t,
				ward: entry.gsx$ward.$t,
				region: entry.gsx$region.$t,
				location: entry.gsx$location.$t,
				restrictions: entry.gsx$restrictions.$t,
				eligibility: entry.gsx$eligibilitylimitations.$t,
				website: entry.gsx$website.$t,
				contact: entry.gsx$contact.$t,
				contact2: entry.gsx$contact2.$t,
				//notes: entry.gsx$notese.gquality.$t
			}
		});
		displayData = data;
	});
  
	
	
	// displayData = displayData;
  
let types = ['All'];
let regularity = ['All'];
let quadrants = ['All'];
let wards = ['All'];
let days = ['All'];

let typeOptions = [
	'All',
  'Groceries',
	'Dinner',
	'Lunch',
  'Breakfast',
	'Delivery'
];

const regularityOptions = [
	"All",
  "Weekly",
  "Biweekly",
  "1st and 3rd Sat of the month",
  "2nd Tue of the month",
  "2nd and 4th Wed of the month",
]; 

const quadrantOptions = [
	"All",
  "NW",
  "NE",
  "SE",
  "SW"
];
	
const wardOptions = [
	"All",
	1,
	2,
	3,
	4,
	6
]

const dayOptions =[
	"All",
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
								if(types.includes('All')){return item}
                if(types.includes(item.type)){return item}
    		})
			.filter(item => {
								if(regularity.includes('All')){return item}
                if(regularity.includes(item.regularity)){return item}
		})
		.filter(item => {
								if(quadrants.includes('All')){return item}
                if(quadrants.includes(item.quadrant)){return item}
		})
	.filter(item => {
								if(wards.includes('All')){return item}
                if(wards.includes(item.ward)){return item}
		})
	.filter(item => {
		const sharedDates = days.map(day => item.days.find(d => d === day))
								if(days.includes('All')){return item}
               if(sharedDates[0] !== undefined){return item}
		})
}

</script>

<div id="inner">
<h1 class="centered">DC Resources</h1>
<select multiple bind:value={types} on:select={join(types, regularity, quadrants, wards, days)}>
{#each typeOptions as type}
  <option selected value={type}>
    {type}
  </option>
{/each}
</select>

<select multiple bind:value={regularity} on:select={join(types, regularity, quadrants, wards, days)}>
{#each regularityOptions as time}
  <option selected value={time}>
    {time}
  </option>
{/each}
</select>

<select multiple bind:value={days} on:select={join(types, regularity, quadrants, wards, days)}>
{#each dayOptions as day}
  <option value={day}>
    {day}
  </option>
{/each}
</select>

<select multiple bind:value={quadrants} on:select={join(types, regularity, quadrants, wards, days)}>
{#each quadrantOptions as quad}
  <option value={quad}>
    {quad}
  </option>
{/each}
</select>

<select multiple bind:value={wards} on:select={join(types, regularity, quadrants, wards, days)}>
{#each wardOptions as ward}
  <option value={ward}>
    {ward}
  </option>
{/each}
</select>

{#if data.length}
	<ul>
		{#each displayData as datum}
		<h3>{datum.name}</h3>
		<div class="resource">
			{#if datum.regularity[0] >= '0' && datum.regularity[0] <= '9'}
				<p>📆 {datum.regularity} {datum.type}</p>
			{:else}
				<p>📆 {datum.regularity} {datum.type} ({datum.days})</p>
			{/if}
			{#if datum.start && datum.end}
				<p>🕒 {datum.start} to {datum.end}</p>
			{:else if datum.start && !datum.end}
				<p>🕒 Starts at {datum.start}</p>
			{:else if !datum.start && datum.end}
				<p>🕒 Ends at {datum.end}</p>
			{/if}
			{#if datum.ward}
				<p>🏛 {datum.quadrant} DC - Ward {datum.ward}</p>
			{:else}
				<p>🏛 {datum.quadrant} DC</p>
			{/if}
			{#if datum.region}
				<p>🏢 {datum.location} ({datum.region})</p>
			{:else}
				<p>🏢 {datum.location}</p>
			{/if}
			<!-- only show restrcitnos and eligibility if not noo -->
			{#if datum.restrictions === 'yes'}
				<p>🚫 Restrictions? {datum.eligibility}</p>
			{/if}
			{#if datum.website || datum.contact || datum.contact2}
				<p>📞 {datum.website} {datum.contact} {datum.contact2}</p>
			{/if}
		</div>
	{/each}
	</ul>
{:else}
	<p>No Results</p>
{/if}
</div>

