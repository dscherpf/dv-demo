<script type="ts">
    import { Toggle } from 'carbon-components-svelte';
    import { ScatterChart } from '@carbon/charts-svelte';
    import '@carbon/charts/styles.min.css';

    import dayjs from 'dayjs';

    var now = dayjs().format('YYYY-MM-DD');

    let chartData: any[] = [];
    let start = now;
    let end = '';
    const baseURL = 'https://api.nasa.gov/neo/rest/v1/feed';
    let startQuery = start ? '?start_date=' + start : '?start_date=' + now;
    let endQuery = end ? '&end_date=' + end : '';
    const apiKey = '&api_key=' + import.meta.env.VITE_NASA_API_KEY;

    async function fetchData() {
        let response = await fetch(baseURL + startQuery + endQuery + apiKey);
        let data = await response.json();

        let dataArray: any[] = [];
        let dataMap = Object.entries(data.near_earth_objects).map(
            (value) => value[1]
        );

        let neoData = dataArray.concat(...dataMap);
        console.log('NEODATA', neoData);

        chartData = neoData.map((neo) => {
            return {
                group: neo.is_potentially_hazardous_asteroid
                    ? 'Potential Hazard'
                    : 'Safe',
                estDiameter:
                    neo.estimated_diameter.meters.estimated_diameter_max,
                closeApproach:
                    neo.close_approach_data[0].miss_distance.kilometers,
                name: neo.name,
            };
        });
    }

    fetchData();

    let xToggle = true;
    let yToggle = true;
</script>

{#if chartData.length}
    <ScatterChart
        data={chartData}
        options={{
            title: 'Near Earth Objects - Close approaches this week',
            axes: {
                bottom: {
                    title: 'Diameter (Meters)',
                    mapsTo: 'estDiameter',
                    scaleType: xToggle ? 'linear' : 'labels',
                },
                left: {
                    title: 'Miss Distance (Kilometers)',
                    mapsTo: 'closeApproach',
                    scaleType: yToggle ? 'linear' : 'labels',
                },
            },
            height: '600px',
        }}
    />
    <div class="toggle-set">
        <Toggle
            labelText="X Scale Type"
            bind:toggled={xToggle}
            labelA="Labels"
            labelB="Linear"
        />
        <Toggle
            labelText="X Scale Type"
            bind:toggled={yToggle}
            labelA="Labels"
            labelB="Linear"
        />
    </div>
{/if}

<!-- markup  -->
<style>
    .toggle-set {
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        align-items: flex-start;
        padding-top: 1rem;
    }
    .toggle-set .bx--form-item {
        flex-grow: 0;
    }
</style>
