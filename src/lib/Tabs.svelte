<script>
    import { Tabs, Tab, TabContent } from 'carbon-components-svelte';
    import NeoChart from './NeoChart.svelte';
    import NeoChart2 from './NeoChart2.svelte';
    import NeoChart3 from './NeoChart3.svelte';
</script>

<Tabs autoWidth>
    <Tab label="NEOs - Size and Proximity" />
    <Tab label="NEOs - Velocity by Day" />
    <Tab label="Texas Chem" />
    <svelte:fragment slot="content">
        <TabContent><NeoChart /></TabContent>
        <TabContent><NeoChart2 /></TabContent>
        <TabContent><NeoChart3 /></TabContent>
    </svelte:fragment>
</Tabs>
