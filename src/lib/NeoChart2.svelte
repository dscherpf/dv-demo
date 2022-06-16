<script type="ts">
    import '@carbon/charts/styles.min.css';
    import { BoxplotChart } from '@carbon/charts-svelte';

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

        chartData = neoData.map((neo) => {
            return {
                group: neo.close_approach_data[0].close_approach_date,
                relative_velocity: parseInt(
                    neo.close_approach_data[0].relative_velocity
                        .kilometers_per_hour
                ),
                name: neo.name,
            };
        });
    }

    fetchData();
</script>

{#if chartData.length}
    <BoxplotChart
        data={chartData}
        options={{
            title: 'Near Earth Objects - Close approaches this week',
            axes: {
                bottom: {
                    title: 'Relative Velocity (Km/h)',
                    mapsTo: 'relative_velocity',
                },
                left: {
                    title: 'Date',
                    mapsTo: 'group',
                    scaleType: 'labels',
                },
            },
            height: '600px',
        }}
    />
{/if}

<!-- markup  -->
<style>
</style>
