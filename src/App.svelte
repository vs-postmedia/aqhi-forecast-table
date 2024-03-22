<script>
    // COMPONENTS
    import { onMount } from 'svelte';
    import { csvParse } from 'd3-dsv';
    import Table from "$components/Table.svelte";

    // DATA
    const dataUrl = 'https://raw.githubusercontent.com/vs-postmedia/bc-wildfire-scraper/master/data/aqhi-data.csv';

    // VARIABLES
    let data;

    // REACTIVE VARIABLES

    async function fetchData(url) {
        const resp = await fetch(url);
        data = await resp.text();
        return csvParse(data);
    }

    async function init() {
        // fetch remote data
        data = await fetchData(dataUrl);
        data = data.filter(d => d.name !== '');
    }

    onMount(init);
</script>

<main>
    <Table 
        data={data}
    />
</main>

<footer>
    <p class="source">Source:  <a href="https://envistaweb.env.gov.bc.ca/" target="_blank">B.C. government</a></p>
</footer>
  
<style>
    @import '$css/normalize.css';
    @import '$css/fonts.css';
    @import '$css/colors.css';
    @import '$css/app.css';
    
    header {
		margin-bottom: 2rem;
	}
	header > h1 {
		text-align: center;
	}
	header .subhead {
		margin: 0 auto;
		max-width: 525px;
		text-align: center;
	}
    .legend-table {
        margin: 10px 0;
    }
    .legend-table h3 {
        margin-bottom: 5px;
    }
</style>
