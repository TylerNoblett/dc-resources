<script>
  let data = [
  {"type": "breakfast", "regularity": "weekly", "name": "National Community Church", "quadrant": "NW"},
  {"type": "breakfast", "regularity": "1st and 3rd Sat of the month", "name": "Miriam's Kitchen (Breakfast)", "quadrant": "SE"},
  {"type": "dinner", "regularity": "2nd Tue of the month", "name": "Miriam's Kitchen (Dinner)", "quadrant": "SE"},
  {"type": "groceries", "regularity": "weekly", "name": "Spanish Catholic Center", "quadrant": "SW"}
]
	// give selector objects "all" that is selected by default
  
let flavours = [];
let stimes = [];
let chosenQuadrants = [];

let menu = [
	'all',
  'groceries',
  'dinner',
  'breakfast'
];

const regularity = [
  "weekly",
  "biweekly",
  "1st and 3rd Sat of the month",
  "2nd Tue of the month",
  "2nd and 4th Wed of the month",
]; 

const quadrant = [
  "NW",
  "NE",
  "SE",
  "SW"
]

function join(flavours, stimes, chosenQuadrants) {
  if (!flavours.length && !stimes.length && !chosenQuadrants){
    return data.map(item => item.name)
  } else {
    return data
// 				.filter(item => flavours.includes(item.type))
// 				.filter(item => stimes.includes(item.regularity))
// 				.filter(item => chosenQuadrants.includes(item.quadrant))
      .filter(item => {
              if (flavours.length){
								if(flavours.includes('all')){return item}
                if(flavours.includes(item.type)){return item}
              } else {return item}
              //&& stimes.includes(item.regularity)
              //&& chosenQuadrants.includes(item.quadrant)
    		})
			.filter(item => {
							if (stimes.length){
								if(stimes.includes('all')){return item}
                if(stimes.includes(item.regularity)){return item}
              } else {return item}
		})
		.filter(item => {
							if (chosenQuadrants.length){
								if(chosenQuadrants.includes('all')){return item}
                if(chosenQuadrants.includes(item.quadrant)){return item}
              } else {return item}
		})
			.map(item => item.name)
  }
}

</script>


<h2>Flavours</h2>

<select multiple bind:value={flavours} >//selected="all">
{#each menu as flavour}
  <option value={flavour}>
    {flavour}
  </option>
{/each}
</select>

<select multiple bind:value={stimes}>
{#each regularity as time}
  <option value={time}>
    {time}
  </option>
{/each}
</select>

<select multiple bind:value={chosenQuadrants}>
{#each quadrant as thing}
  <option value={thing}>
    {thing}
  </option>
{/each}
</select>
<p>
{console.log("FLAV", flavours), console.log("TIME", stimes)}
</p>
<br>
{join(flavours, stimes, chosenQuadrants)}

