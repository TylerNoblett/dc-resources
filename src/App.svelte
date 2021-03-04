<script>
  let data = [
  {"type": "breakfast", "regularity": "weekly", "name": "National Community Church", "quadrant": "NW"},
  {"type": "breakfast", "regularity": "1st and 3rd Sat of the month", "name": "Miriam's Kitchen (Breakfast)", "quadrant": "SE"},
  {"type": "dinner", "regularity": "2nd Tue of the month", "name": "Miriam's Kitchen (Dinner)", "quadrant": "SE"},
  {"type": "groceries", "regularity": "weekly", "name": "Spanish Catholic Center", "quadrant": "SW"}
]
	// give selector objects "all" that is selected by default
  
let types = ['all'];
let regularity = ['all'];
let quadrants = ['all'];

let typeOptions = [
	'all',
  'groceries',
  'dinner',
  'breakfast'
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
]

function join(types, regularity, quadrants) {
    return data
      .filter(item => {
              if (types.length){
								if(types.includes('all')){return item}
                if(types.includes(item.type)){return item}
              }
    		})
			.filter(item => {
							if (regularity.length){
								if(regularity.includes('all')){return item}
                if(regularity.includes(item.regularity)){return item}
              }
		})
		.filter(item => {
							if (quadrants.length){
								if(quadrants.includes('all')){return item}
                if(quadrants.includes(item.quadrant)){return item}
              }
		})
			.map(item => item.name)

}

</script>


<h2>types</h2>

<select multiple bind:value={types} >//selected="all">
{#each typeOptions as type}
  <option selected value={type}>
    {type}
  </option>
{/each}
</select>

<select multiple bind:value={regularity}>
{#each regularityOptions as time}
  <option selected value={time}>
    {time}
  </option>
{/each}
</select>

<select multiple bind:value={quadrants}>
{#each quadrantOptions as quad}
  <option value={quad}>
    {quad}
  </option>
{/each}
</select>
{console.log("FLAV", types), console.log("TIME", regularity)}
<br>
{join(types, regularity, quadrants)}

