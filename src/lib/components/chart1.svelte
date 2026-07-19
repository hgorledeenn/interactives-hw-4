<script>
	import * as d3 from 'd3';

	const parseTime = d3.timeParse('%I:%M%p');

	// for controls
	let selected = $state('None');
	let opacity = $state(0.2);

	// for moveable/reactive charts
	// I don't like how it looks when the chart stretches all the way across my browser, so I incorporated the binded ClientWidth but used Math.min to set a max width for my chart of 1200px
	let containerWidth = $state(0);
	let width = $derived(Math.min(1200, containerWidth));

	// defining my data
	const data = [
		{
			city: 'New York City',
			aqi: [
				{ time: '12:00AM', value: 187 },
				{ time: '1:00AM', value: 183 },
				{ time: '2:00AM', value: 181 },
				{ time: '3:00AM', value: 180 },
				{ time: '4:00AM', value: 179 },
				{ time: '5:00AM', value: 176 },
				{ time: '6:00AM', value: 163 },
				{ time: '7:00AM', value: 152 },
				{ time: '8:00AM', value: 135 },
				{ time: '9:00AM', value: 121 },
				{ time: '10:00AM', value: 126 },
				{ time: '11:00AM', value: 97 },
				{ time: '12:00PM', value: 82 },
				{ time: '1:00PM', value: 75 },
				{ time: '2:00PM', value: 64 },
				{ time: '3:00PM', value: 60 },
				{ time: '4:00PM', value: 59 },
				{ time: '5:00PM', value: 61 },
				{ time: '6:00PM', value: 79 },
				{ time: '7:00PM', value: 106 },
				{ time: '8:00PM', value: 116 },
				{ time: '9:00PM', value: 125 },
				{ time: '10:00PM', value: 137 },
				{ time: '11:00PM', value: 153 }
			]
		},
		{
			city: 'Chicago',
			aqi: [
				{ time: '12:00AM', value: 376 },
				{ time: '1:00AM', value: 371 },
				{ time: '2:00AM', value: 404 },
				{ time: '3:00AM', value: 404 },
				{ time: '4:00AM', value: 398 },
				{ time: '5:00AM', value: 380 },
				{ time: '6:00AM', value: 357 },
				{ time: '7:00AM', value: 291 },
				{ time: '8:00AM', value: 263 },
				{ time: '9:00AM', value: 224 },
				{ time: '10:00AM', value: 192 },
				{ time: '11:00AM', value: 167 },
				{ time: '12:00PM', value: 129 },
				{ time: '1:00PM', value: 112 },
				{ time: '2:00PM', value: 99 },
				{ time: '3:00PM', value: 88 },
				{ time: '4:00PM', value: 71 },
				{ time: '5:00PM', value: 74 },
				{ time: '6:00PM', value: 70 },
				{ time: '7:00PM', value: 66 },
				{ time: '8:00PM', value: 61 },
				{ time: '9:00PM', value: 58 },
				{ time: '10:00PM', value: 58 },
				{ time: '11:00PM', value: 58 }
			]
		},
		{
			city: 'Detroit',
			aqi: [
				{ time: '12:00AM', value: 408 },
				{ time: '1:00AM', value: 398 },
				{ time: '2:00AM', value: 399 },
				{ time: '3:00AM', value: 390 },
				{ time: '4:00AM', value: 381 },
				{ time: '5:00AM', value: 378 },
				{ time: '6:00AM', value: 382 },
				{ time: '7:00AM', value: 370 },
				{ time: '8:00AM', value: 356 },
				{ time: '9:00AM', value: 316 },
				{ time: '10:00AM', value: 272 },
				{ time: '11:00AM', value: 238 },
				{ time: '12:00PM', value: 229 },
				{ time: '1:00PM', value: 224 },
				{ time: '2:00PM', value: 221 },
				{ time: '3:00PM', value: 224 },
				{ time: '4:00PM', value: 232 },
				{ time: '5:00PM', value: 243 },
				{ time: '6:00PM', value: 261 },
				{ time: '7:00PM', value: 245 },
				{ time: '8:00PM', value: 227 },
				{ time: '9:00PM', value: 220 },
				{ time: '10:00PM', value: 218 },
				{ time: '11:00PM', value: 208 }
			]
		},
		{
			city: 'Pittsburgh',
			aqi: [
				{ time: '12:00AM', value: 273 },
				{ time: '1:00AM', value: 262 },
				{ time: '2:00AM', value: 274 },
				{ time: '3:00AM', value: 306 },
				{ time: '4:00AM', value: 296 },
				{ time: '5:00AM', value: 241 },
				{ time: '6:00AM', value: 230 },
				{ time: '7:00AM', value: 226 },
				{ time: '8:00AM', value: 221 },
				{ time: '9:00AM', value: 212 },
				{ time: '10:00AM', value: 212 },
				{ time: '11:00AM', value: 214 },
				{ time: '12:00PM', value: 212 },
				{ time: '1:00PM', value: 211 },
				{ time: '2:00PM', value: 213 },
				{ time: '3:00PM', value: 220 },
				{ time: '4:00PM', value: 227 },
				{ time: '5:00PM', value: 238 },
				{ time: '6:00PM', value: 248 },
				{ time: '7:00PM', value: 241 },
				{ time: '8:00PM', value: 214 },
				{ time: '9:00PM', value: 210 },
				{ time: '10:00PM', value: 208 },
				{ time: '11:00PM', value: 214 }
			]
		},
		{
			city: 'Washington DC',
			aqi: [
				{ time: '12:00AM', value: 223 },
				{ time: '1:00AM', value: 236 },
				{ time: '2:00AM', value: 237 },
				{ time: '3:00AM', value: 245 },
				{ time: '4:00AM', value: 249 },
				{ time: '5:00AM', value: 250 },
				{ time: '6:00AM', value: 247 },
				{ time: '7:00AM', value: 238 },
				{ time: '8:00AM', value: 209 },
				{ time: '9:00AM', value: 179 },
				{ time: '10:00AM', value: 171 },
				{ time: '11:00AM', value: 173 },
				{ time: '12:00PM', value: 174 },
				{ time: '1:00PM', value: 177 },
				{ time: '2:00PM', value: 177 },
				{ time: '3:00PM', value: 178 },
				{ time: '4:00PM', value: 176 },
				{ time: '5:00PM', value: 174 },
				{ time: '6:00PM', value: 168 },
				{ time: '7:00PM', value: 167 },
				{ time: '8:00PM', value: 168 },
				{ time: '9:00PM', value: 167 },
				{ time: '10:00PM', value: 168 },
				{ time: '11:00PM', value: 171 }
			]
		}
	];

	// setting svg specs
	const height = 600;
	const margin = {
		top: 30,
		right: 30,
		bottom: 40,
		left: 70
	};

	// making the xScale
	let xScale = $derived(
		d3
			.scaleTime()
			.domain([parseTime('12:00AM'), parseTime('11:00PM')])
			.range([margin.left, width - margin.right])
	);

	// creating maxAqi variable to clean up yScale .domain() chunk
	let maxAqi = d3.max(data, (city) => d3.max(city.aqi, (d) => d.value));

	// making yScale
	let yScale = $derived(
		d3
			.scaleLinear()
			.domain([0, maxAqi])
			.range([height - margin.bottom, margin.top])
	);

	// making lineGenerator
	let lineGenerator = $derived(
		d3
			.line()
			.x((d) => xScale(parseTime(d.time)))
			.y((d) => yScale(d.value))
	);

	// defining colors for the cities' lines
	const cityColors = {
		Detroit: '#2ECC71',
		Chicago: '#33A8FF',
		Pittsburgh: '#F1C40F',
		'Washington DC': '#9B59B6',
		'New York City': '#FF5733'
	};

	// and creating colorScale to apply those colors to each city
	let colorScale = $derived(
		d3.scaleOrdinal().domain(Object.keys(cityColors)).range(Object.values(cityColors))
	);

	// maxing yTicks and xTicks variables for my chart
	const yTicks = d3.range(0, Math.floor(maxAqi / 50) * 50 + 1, 50);
	const xTicks = ['12:00AM', '6:00AM', '12:00PM', '6:00PM', '11:00PM'];

	// defining my ranges of AQI scale
	const aqiScaleRange = [
		{ rating: 'Good', min: 0, max: 50, color: '#01e400' },
		{ rating: 'Moderate', min: 50, max: 100, color: '#ffff00' },
		{ rating: 'Unhealthy for Sensitive Groups', min: 100, max: 150, color: '#ff7e00' },
		{ rating: 'Unhealthy', min: 150, max: 200, color: '#ff0000' },
		{ rating: 'Very Unhealthy', min: 200, max: 300, color: '#8f3f97' },
		{ rating: 'Hazardous', min: 300, max: 500, color: '#7e0122' }
	];

	// for tooltips
	let mouseX = $state(0);
	let mouseY = $state(0);
	let tooltipX = $derived(xScale.invert(mouseX));
	const times = data[0].aqi.map(d => parseTime(d.time));
	const bisect = d3.bisector(d => d).center;
	let hourIndex = $derived(
    	bisect(times, tooltipX)
	);

</script>

<div bind:clientWidth={containerWidth}>
	<div class="controls">
		{#each Object.entries(cityColors) as [city, color]}
			<button
				class="city-button"
				style="background-color: {color}"
				onclick={() => (selected = city)}
			>
				{city}
			</button>
		{/each}

		<!-- None button -->
		<label>
			<input type="range" bind:value={opacity} min="0" max="1" step="0.05" />
		</label>
		<button
			onclick={() => {
				selected = 'None';
				opacity = '0.2';
			}}
		>
			Reset
		</button>
		<p>{data[0].aqi[hourIndex].time}</p>
	</div>

	{#if containerWidth > 0}
		<svg {width} {height}>
			{#each aqiScaleRange as level}
				<rect
					x={margin.left}
					y={Math.max(yScale(level.max), yScale(maxAqi))}
					width={width - margin.left - margin.right}
					height={Math.abs(yScale(level.min) - yScale(Math.min(level.max, maxAqi)))}
					fill={level.color}
					{opacity}
				/>
			{/each}

			{#each yTicks as yTick}
				<line
					x1={margin.left - 40}
					x2={width - margin.right}
					y1={yScale(yTick)}
					y2={yScale(yTick)}
					fill="none"
					stroke="#c9c9c9"
					stroke-width="0.75"
				/>

				<text
					x={margin.left - 40}
					y={yScale(yTick) - 8}
					dominant-baseline="end"
					text-anchor="start"
					font-family="sans-serif"
					font-size="15"
				>
					{yTick}
				</text>
			{/each}

			{#each xTicks as xTick}
				<text
					x={xScale(parseTime(xTick))}
					y={height - margin.bottom + 10}
					dominant-baseline="hanging"
					text-anchor="middle"
					font-family="sans-serif"
					font-size="15"
				>
					{xTick.replace(':00', '')}
				</text>
			{/each}

			{#each data as cityData}
				<path
					d={lineGenerator(cityData.aqi)}
					fill="none"
					stroke={colorScale(cityData.city)}
					stroke-width={selected === 'None' || selected === cityData.city ? '3' : '2'}
					opacity={selected === 'None' || selected === cityData.city ? '1' : '0.3'}
				/>
			{/each}

			<rect
			    x={margin.left}
    			y={margin.top}
    			width={width - margin.left - margin.right}
    			height={height - margin.top - margin.bottom}
    			fill="transparent"
    			onmousemove={(event) => {
        			const [x, y] = d3.pointer(event);
        			mouseX = x;
        			mouseY = y;
    			}}
			/>

			{#if mouseX>0}
				<line
					x1={xScale(parseTime((data[0].aqi[hourIndex].time)))}
					x2={xScale(parseTime((data[0].aqi[hourIndex].time)))}
					y1={margin.top}
					y2={height - margin.bottom}
					stroke="gray"
					stroke-width="0.5"
				/>
			{/if}
		</svg>
	{/if}
</div>

<style>
	.city-button {
		padding: 4px 10px;
		font-size: 16px;
		font-family: sans-serif;
	}
</style>
