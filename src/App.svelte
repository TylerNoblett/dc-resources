<script>
	//import * as data from './info.json'
	let data = [
		{"type": "breakfast", "name": "National Community Church"},
		{"type": "dinner", "name": "Miriam's Kitchen (Breakfast)"},
		{"type": "groceries", "name": "Miriam's Kitchen (Dinner)"},
		{"type": "groceries", "name": "Spanish Catholic Center"}
	]

	let options = [];

	const type = [
		'breakfast',
		'dinner',
		'groceries',
		'weekly',
	];

	const dayOfweek = [
		'sunday',
		'monday',
		'tuesday',
		'wednesday',
		'thursday',
		'friday',
		'saturday'
	]

	// const regularity = [
	// 	"weekly",
	// 	"biweekly",
	// 	"1st and 3rd Sat of the month",
	// 	"2nd Tue of the month",
	// 	"2nd and 4th Wed of the month",
	// 	"2nd Tue of the month",
	// ]; 

	// const quadrant = [
	// 	"NW",
	// 	"NE",
	// 	"SE",
	// 	"SW"
	// ]

	// const startTime = []
	// const endTime = []
	// const ward = [1, 2, 3, 4, 6]
	
	function join(options) {
		if (options.length === 0) return data.map(resource => ` ${resource.name}`)
		return data
			.filter(resource => 
				options.includes(resource.type)
				&& options.includes(resource.dayOfweek)
			)
			.map(resource => ` ${resource.name}`)
	}
</script>


<h2>options</h2>

<select multiple bind:value={options}>
	{#each type as option}
		<option value={option}>
			{option}
		</option>
	{/each}
</select>

	<p>
		{join(options)}
	</p>
