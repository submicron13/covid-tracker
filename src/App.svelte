<script>
	import { onMount } from 'svelte';
	import moment from 'moment';
	import * as Pancake from '@sveltejs/pancake';
	const CURRENT_API = 'https://covidtracking.com/api/v1/states/current.json';
	const DAILY_API = 'https://covidtracking.com/api/v1/states/daily.json'
	let state = 'AL'
	// console.log(myParam);
	let azData = ''
	let azDailyData = ''
	let checked;
	let selected;
	let days;
	let dayZero = ''
	let deathIncrease = 0;
	let positiveIncrease = 0;
	let negativeIncrease = 0;
	let deathPoints = [];
	let positivePoints = [];
	let dataLoaded = false
	let states = 
	// States collapsed
[
    {
        "id": 0,
        "state": "AL"
    },
    {
        "id": 1,
        "state": "AK"
    },
    {
        "id": 2,
        "state": "AZ"
    },
    {
        "id": 3,
        "state": "AR"
    },
    {
        "id": 4,
        "state": "CA"
    },
    {
        "id": 5,
        "state": "CO"
    },
    {
        "id": 6,
        "state": "CT"
    },
    {
        "id": 7,
        "state": "DE"
    },
    {
        "id": 8,
        "state": "DC"
    },
    {
        "id": 9,
        "state": "FL"
    },
    {
        "id": 10,
        "state": "GA"
    },
    {
        "id": 11,
        "state": "HI"
    },
    {
        "id": 12,
        "state": "ID"
    },
    {
        "id": 13,
        "state": "IL"
    },
    {
        "id": 14,
        "state": "IN"
    },
    {
        "id": 15,
        "state": "IA"
    },
    {
        "id": 16,
        "state": "KS"
    },
    {
        "id": 17,
        "state": "KY"
    },
    {
        "id": 18,
        "state": "LA"
    },
    {
        "id": 19,
        "state": "ME"
    },
    {
        "id": 20,
        "state": "MD"
    },
    {
        "id": 21,
        "state": "MA"
    },
    {
        "id": 22,
        "state": "MI"
    },
    {
        "id": 23,
        "state": "MN"
    },
    {
        "id": 24,
        "state": "MS"
    },
    {
        "id": 25,
        "state": "MO"
    },
    {
        "id": 26,
        "state": "MT"
    },
    {
        "id": 27,
        "state": "NE"
    },
    {
        "id": 28,
        "state": "NV"
    },
    {
        "id": 29,
        "state": "NH"
    },
    {
        "id": 30,
        "state": "NJ"
    },
    {
        "id": 31,
        "state": "NM"
    },
    {
        "id": 32,
        "state": "NY"
    },
    {
        "id": 33,
        "state": "NC"
    },
    {
        "id": 34,
        "state": "ND"
    },
    {
        "id": 35,
        "state": "OH"
    },
    {
        "id": 36,
        "state": "OK"
    },
    {
        "id": 37,
        "state": "OR"
    },
    {
        "id": 38,
        "state": "PA"
    },
    {
        "id": 39,
        "state": "RI"
    },
    {
        "id": 40,
        "state": "SC"
    },
    {
        "id": 41,
        "state": "SD"
    },
    {
        "id": 42,
        "state": "TN"
    },
    {
        "id": 43,
        "state": "TX"
    },
    {
        "id": 44,
        "state": "UT"
    },
    {
        "id": 45,
        "state": "VT"
    },
    {
        "id": 46,
        "state": "VA"
    },
    {
        "id": 47,
        "state": "WA"
    },
    {
        "id": 48,
        "state": "WV"
    },
    {
        "id": 49,
        "state": "WI"
    },
    {
        "id": 50,
        "state": "WY"
    }
]
		
function changedState() {
// 	var result = array.filter(function(item) {
//     return filterArray.indexOf(item.id) !== -1;
// });
	//console.log(selected)
	state = selected.state
	resetData()
	getData();
	//   state = states.filter( obj => {
	// 	  if (obj.id === selected ) {
	// 		  return obj.state;
	// 	  } 
	// 	}
	//   )
	//console.log(state)
	  //state = states[selected].state;
}
function resetData()  {
		deathPoints = [];
		positivePoints = [];
		azDailyData = null;
		azData = null;
		dataLoaded = false
}
function getData() {
	fetch(CURRENT_API)
	.then(response => response.json())
	.then(result => {
		
		azData = result.filter( obj => {
		return obj.state === state
		} )[0]
		//console.log(azData);
	}).then( () => {
	let timestamp = new Date(azData.checkTimeEt);
	checked = moment(timestamp).local().format('MM-DD HH:mm:ss')
	});
	fetch(DAILY_API)
	.then(response => response.json())
	.then(result => {
		azDailyData = result.filter( obj => {
		return obj.state === state
		} )
		//console.log(azData);
	}).then( () => {
		deathIncrease = azDailyData[0].deathIncrease
		negativeIncrease = azDailyData[0].negativeIncrease
		positiveIncrease = azDailyData[0].positiveIncrease
		days = azDailyData.length
		let count = 0;
		const revAZdaily = azDailyData.reverse();
		dayZero = moment(revAZdaily[0].date, ['YYYYMMDD']).format('MM-DD-YYYY')
									
		revAZdaily.forEach(element => {
			let deaths = element.death || 0;
			let pos = element.positive || 0;

			deathPoints.push( 
				{
				x: count,
				y: deaths });
			positivePoints.push( 
				{
				x: count,
				y: pos });
			count += 1;
		});
		dataLoaded = true;
		// console.log(points);
	});
}
getData()
	  //console.log(azData)
</script>

<style>
span {
	color: #EB6864;
}
	.chartWrapper {
		width:85vw;
		margin: auto;
	}
	.chart {
		height: 400px;
		padding: 3em 0 2em 2em;
		margin: 0 0 36px 0;
	}

	.grid-line {
		position: relative;
		display: block;
	}

	.grid-line.horizontal {
		width: calc(100% + 2em);
		left: -2em;
		border-bottom: 1px dashed #ccc;
	}

	.grid-line.vertical {
		height: 100%;
		border-left: 1px dashed #ccc;
	}

	.grid-line.first {
		border-bottom: 1px solid #333;
	}

	.grid-line span {
		position: absolute;
		left: 0;
		bottom: 2px;
		font-family: sans-serif;
		font-size: 14px;
		color: #999;
	}

	.year-label {
		position: absolute;
		width: 4em;
		left: -2em;
		bottom: -22px;
		font-family: sans-serif;
		font-size: 14px;
		color: #999;
		text-align: center;
	}

	path.data {
		stroke: rgb(171, 0, 214);
		stroke-linejoin: round;
		stroke-linecap: round;
		stroke-width: 6px;
		fill: none;
	}

	path.highlight {
		stroke: rgb(171, 0, 214);
		stroke-linejoin: round;
		stroke-linecap: round;
		stroke-width: 10px;
		fill: none;
	}
	.textCenter {
		text-align: center
	}
</style>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://bootswatch.com/4/journal/bootstrap.min.css" />
</svelte:head>

{#if dataLoaded}
	 <!-- content here -->

<h1 class="text-primary">{state} Covid Tracker</h1>
<label for="state">Select State:</label>
<select name="state" bind:value={selected} on:change={changedState}>
	{#each states as state}
		<option value={state}>
			{state.state}
		</option>
	{/each}
</select>
<h2 >Positive: {azData.positive} 		<span>+{ positiveIncrease }</span></h2>
<h2>Negative: {azData.negative} 		<span>+{ negativeIncrease }</span></h2>
<h2>Deaths: {azData.death} 				<span>+{ deathIncrease }</span></h2>
<!-- <h2>Positive Increase: { positiveIncrease}</h2>
<h2>Negative Increase: { negativeIncrease}</h2>
<h2>Death Increase: { deathIncrease}</h2> -->
<h6>Data Updated: {checked}</h6>
<br>
<div class="chartWrapper">


<div class="chart">

	<Pancake.Chart x1={0} x2={days} y1={0} y2={azData.death * 1.5}>
		<Pancake.Grid horizontal count={5} let:value let:first>
			<div class="grid-line horizontal" class:first><span>{value}</span></div>
		</Pancake.Grid>

		<Pancake.Grid vertical count={10} let:value>
			<div class="grid-line vertical"></div>
			<span class="year-label">{value}</span>
		</Pancake.Grid>

		<Pancake.Svg>
		<Pancake.SvgScatterplot data={deathPoints} x="{d => d.x}" y="{d => d.y}" let:d>
			<path class="data" {d}/>
		</Pancake.SvgScatterplot>

			<Pancake.Quadtree data={deathPoints} x="{d => d.x}" y="{d => d.y}" let:closest>
				{#if closest}
					<Pancake.SvgPoint x={closest.x} y={closest.y} let:d>
						<path class="highlight" d={d}/>
					</Pancake.SvgPoint>
				{/if}
			</Pancake.Quadtree>
		</Pancake.Svg>
	</Pancake.Chart>
</div>

<div class="textCenter">
<h1 >Total Deaths By Day</h1>
	<h2>Day 0: {dayZero}</h2>
</div>
<br>
<br>
<div class="chart">

	<Pancake.Chart x1={0} x2={days} y1={0} y2={azData.positive * 1.5}>
		<Pancake.Grid horizontal count={5} let:value let:first>
			<div class="grid-line horizontal" class:first><span>{value}</span></div>
		</Pancake.Grid>

		<Pancake.Grid vertical count={10} let:value>
			<div class="grid-line vertical"></div>
			<span class="year-label">{value}</span>
		</Pancake.Grid>

		<Pancake.Svg>
		<Pancake.SvgScatterplot data={positivePoints} x="{d => d.x}" y="{d => d.y}" let:d>
			<path class="data" {d}/>
		</Pancake.SvgScatterplot>

			<Pancake.Quadtree data={positivePoints} x="{d => d.x}" y="{d => d.y}" let:closest>
				{#if closest}
					<Pancake.SvgPoint x={closest.x} y={closest.y} let:d>
						<path class="highlight" d={d}/>
					</Pancake.SvgPoint>
				{/if}
			</Pancake.Quadtree>
		</Pancake.Svg>
	</Pancake.Chart>
</div>
<div class="textCenter">
<h1 >Total Positive By Day</h1>
	<h2>Day 0: {dayZero}</h2>
</div>

</div>
<h5>Data From: <a href="https://covidtracking.com/">The Covid Tracking Project</a></h5>
<h5>Github: <a href="https://github.com/submicron13/covid-tracker">Covid-Tracker</a></h5>
{/if}