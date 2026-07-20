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
		top: 10,
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
		Pittsburgh: '#FF5733',
		'Washington DC': '#F1C40F',
		'New York City': '#9B59B6'
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

	// to make the opacity of my subhead text background match the opacity of the chart background
	// this code came from chatGPT to help me solve the problem as I was having trouble on my own
	function hexToRgba(hex, alpha) {
		const r = parseInt(hex.slice(1, 3), 16);
		const g = parseInt(hex.slice(3, 5), 16);
		const b = parseInt(hex.slice(5, 7), 16);

		return `rgba(${r}, ${g}, ${b}, ${alpha})`;
	}

	// for tooltips
	let mouseX = $state(null);
	let mouseY = $state(null);
	let mouseXData = $derived(xScale.invert(mouseX));
	const times = data[0].aqi.map((d) => parseTime(d.time));
	const bisect = d3.bisector((d) => d).center;
	let hourIndex = $derived(bisect(times, mouseXData));
	let displayHourIndex = $derived(
		mouseX === null ? data[0].aqi.length - 1 : bisect(times, xScale.invert(mouseX))
	);
	let tooltipX = $derived(xScale(parseTime(data[0].aqi[hourIndex].time)));
	let tooltipY = $derived(mouseY - 50);

	let currentAQI = $derived(
		data.map((city) => ({
			city: city.city,
			color: colorScale(city.city),
			value: city.aqi[displayHourIndex].value,
			hour: city.aqi[displayHourIndex].time
		}))
	);

	let selectedCityClass = $state(null);

	// for testing tooltips
	const city = $derived(data.find((item) => item.city === selected));
</script>

<div bind:clientWidth={containerWidth}>
	<div
		class="story-head"
		style="width: {width - margin.left - margin.right}px; margin-left: {margin.left}px;"
	>
		<h1>Smoke from Canadian Wildfires Fills US Skies</h1>
		<p>
			The Air Quality Index in 5 US Cities ranged drastically on July 17, as smoke from hundreds of
			active wildfires in Canada was funneled over the Midwest and Northeast. Both Chicago and
			Detroit had sustained AQI values above 300, considered <span
				style="background-color: {hexToRgba('#7e0122', opacity)}; color: {opacity < 0.7
					? '#000'
					: '#fff'};">"Hazardous"</span
			>, while Pittsburgh and Washington D.C. peaked in the next-highest range, considered
			<span
				style="background-color: {hexToRgba('#8f3f97', opacity)}; color: {opacity < 0.7
					? '#000'
					: '#fff'}; fill-opacity: {opacity};">"Very Unhealthy"</span
			>. Hover over the chart below to see how the AQI changed in each of the cities in this 24-hour span.
		</p>
	</div>
	<div class="legend-container">
		<div class="city-section">
			<div class="legend-header">
				<h3>AQI at {currentAQI[0].hour}</h3>
				<p>Click a city to highlight its line</p>
			</div>

			<div class="aqi-summary">
				{#each currentAQI as city}
					<div
						class="city-summary"
						onclick={() => (selected = city.city)}
						class:highlight-box={selected === city.city}
					>
						<div class="dot" style="background-color:{city.color}"></div>

						<div>
							<strong>{city.city}</strong>
							<div>AQI: {city.value}</div>
						</div>
					</div>
				{/each}
			</div>
		</div>

		<div class="controls">
			<label>
				<p class="helper-text"><i>Background opacity:</i></p>
				<input type="range" bind:value={opacity} min="0" max="1" step="0.05" />
			</label>

			<button
				class="reset-button"
				onclick={() => {
					selected = 'None';
					opacity = 0.2;
				}}
			>
				Reset
			</button>
		</div>
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
					stroke-width={selected === 'None' || selected === cityData.city ? '4' : '3'}
					opacity={selected === 'None' || selected === cityData.city ? '1' : '0.3'}
				/>
			{/each}

			<!-- tooltip -->
			{#if mouseX > 0}
				<g>
					<line
						x1={xScale(parseTime(data[0].aqi[hourIndex].time))}
						x2={xScale(parseTime(data[0].aqi[hourIndex].time))}
						y1={margin.top}
						y2={height - margin.bottom}
						stroke="gray"
						stroke-width="0.5"
					/>

					{#each data as city}
						<circle
							cx={xScale(parseTime(data[0].aqi[hourIndex].time))}
							cy={yScale(city.aqi[hourIndex].value)}
							r="6"
							fill={colorScale(city.city)}
							opacity={selected === 'None' || selected === city.city ? '1' : '0.3'}
						/>
					{/each}

					<!-- my first tooltip attempt that shows the data where the mouse is-->

					<!-- <rect x={tooltipX} y={tooltipY} width="100" height="100" fill="#cacaca" opacity="0.8" />

					<foreignObject
						x={tooltipX + 10}
						y={tooltipY}
						width="220"
						height="180"
					>
						<table>
							<thead>
								<tr>
									<th>City</th>
									<th>AQI</th>
								</tr>
							</thead>

							<tbody>
								{#each data as city}
									<tr>
										<td>{city.city}</td>
										<td>{city.aqi[hourIndex].value}</td>
									</tr>
								{/each}
							</tbody>
						</table>
					</foreignObject> -->
				</g>
			{/if}

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
				onmouseleave={() => {
					mouseX = null;
					mouseY = null;
				}}
			/>
		</svg>
	{/if}
</div>

<style>
	.story-head {
		margin-top: 40px;
		margin-bottom: 20px;
	}

	.story-head h1 {
		font-size: 50px;
		font-family: sans-serif;
		margin-bottom: 0px;
	}

	.story-head p {
		margin-top: 10px;
		margin-bottom: 0px;
		font-size: 25px;
		font-family: sans-serif;
	}

	.highlight-box {
		border: 2px solid #333333;
		border-radius: 12px;
		background-color: lightgray;
	}

	.helper-text {
		font-size: 12px;
		font-family: sans-serif;
		color: #555;
		text-align: center;
		/* margin-top: -5px; */
		margin-bottom: 5px;
	}
	.reset-button {
		font-size: 15px;
		font-family: sans-serif;
	}

	.legend-container {
		display: flex;
		align-items: center;
		flex-wrap: wrap;

		gap: 30px;
		margin-top: 30px;
		margin-left: 100px;
		margin-bottom: 20px;
		margin-right: 30px;
	}

	.city-section {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.legend-header {
		text-align: center;
		margin-bottom: 10px;
	}

	.legend-header h3 {
		margin: 0;
		font-size: 18px;
		font-family: sans-serif;
	}

	.legend-header p {
		margin: 4px 0 0;
		font-size: 12px;
		font-style: italic;
		color: #555;
		font-family: sans-serif;
	}

	.aqi-summary {
		display: flex;
		flex-wrap: wrap;
		gap: 20px;
	}

	.controls {
		display: flex;
		align-items: center;
		gap: 15px;
	}

	.city-summary {
		padding-left: 10px;
		padding-right: 10px;
		padding-top: 5px;
		padding-bottom: 5px;
		display: flex;
		align-items: center;
		gap: 8px;
		font-family: sans-serif;
		cursor: pointer;
	}

	.dot {
		width: 20px;
		height: 20px;
		border-radius: 50%;
		flex-shrink: 0;
	}
</style>
