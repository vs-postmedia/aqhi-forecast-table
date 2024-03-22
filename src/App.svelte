<script>
    // COMPONENTS
    import { onMount } from 'svelte';
    import { csvParse } from 'd3-dsv';
    import Table from "$components/Table.svelte";

    // DATA
    // const dataUrl = 'https://raw.githubusercontent.com/vs-postmedia/bc-aqhi-scraper/master/data/aqhi-data.csv';
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
        console.log(data);
    }

    onMount(init);
</script>

<!-- <header>
    <h1>Maximum AQHI forecast</h1>
    <p class="subhead">Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
</header> -->

<main>
    <div class="legend-table">
        <h3>Risk level:</h3>
        <table class="table aqhiScale">
            <tbody>
                <tr>
                    <td class="aqhi1">1</td>
                    <td class="aqhi2">2</td>
                    <td class="aqhi3">3</td>
                    <td class="aqhi4">4</td>
                    <td class="aqhi5">5</td>
                    <td class="aqhi6">6</td>
                    <td class="aqhi7">7</td>
                    <td class="aqhi8">8</td>
                    <td class="aqhi9">9</td>
                    <td class="aqhi10">10</td>
                    <td class="aqhi11" colspan="2">+</td>
                </tr>
                <tr class="legend-text">
                    <td class="aqhiRisk" colspan="3">Low</td>
                    <td class="aqhiRisk" colspan="3">Moderate</td>
                    <td class="aqhiRisk" colspan="4">High</td>
                    <td class="aqhiRisk" colspan="2">Very high</td>
                </tr>
            </tbody>
        </table>
    </div>

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
    @import '$css/aqhi-legend.css';

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
