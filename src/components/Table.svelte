<script>
    export let data = [];

    // LIBS
    import Grid from 'gridjs-svelte'
	import { html } from 'gridjs';

    // COMPONENTS
    // import Tooltip from '$components/Tooltip.svelte';

    // VARS
    let tooltipData;
    let width = 500;
    let maxColWidth = '100px';
    
    // AQHI COLOUR VALUES
    const colourLookup = ['#00CCFF','#0099CC','#006699','#FFFF00','#FFCC00','#FF9933','#FF6666','#FF0000','#CC0000','#990000','#660000'];
    // AQHI Text
    const textLookup = ['Low','Low','Low','Moderate','Moderate','Moderate','Moderate', 'High','High','High','Very high'];
   
    // REACTIVE VARS
    $: columns = [
        {   
            name: 'Region',
            id: 'name',
            maxWidth: maxColWidth,
            formatter: cell => html(`<b>${cell}</b>`)
        },
        {
            name: 'Today', id: 'today', 
            // maxWidth: maxColWidth,
            attributes: cell => assignColour(cell),
            // data: cell => cellText(cell, 'today'),
            formatter: cell => formatCell(cell),
        },
        {
            name: 'Tonight', id: 'tonight', 
            // maxWidth: maxColWidth,
            attributes: cell => assignColour(cell),
            formatter: cell => formatCell(cell)
        },
        {
            name: 'Tmrw.', id: 'tomorrow', 
            // maxWidth: maxColWidth,
            attributes: cell => assignColour(cell),
            formatter: cell => formatCell(cell)
        // },
        // {
        //     name: 'Tmrw night', id: 'tomorrow_night', maxWidth: '90px',
        //     attributes: cell => assignColour(cell),
        //     formatter: cell => formatCell(cell)
        }
    ];

    // FUNCTIONS
    function formatCell(cell) {
        const text = textLookup[parseInt(cell) - 1];
        const textColor = parseInt(cell) > 3 && parseInt(cell) < 7 ? '#000' : '#FFF';

        const riskScore = `<h2 style="color:${textColor}">${cell}</h2>`;
        const riskText = text !== undefined ? `<p style="color:${textColor}">${text}</p>` : '';

        return html(`${riskScore}${riskText}`)

        // return html(`
        //     <h2 style="color:${textColor}">${cell}</h2>
        //     <p style="color:${textColor}">${text}</p>
        // `);
    }
    function assignColour(cell) {
        let style;
        if (cell && cell !== 'NA') {
            style = {
                'style': 
                    `background-color: ${lookupColour(cell)};
                    text-align:center;`
            }
        } else if (cell === 'NA') {
            style = {
                'style': 
                    `background-color: #d1d2d4;
                    text-align:center;`
            }
        }

        return style;
    }
    function lookupColour(aqhi) {
        const score = aqhi === '+' ? 11 : parseInt(aqhi);
        return colourLookup[score - 1];
    }
</script>

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

<div class="chart-container" bind:clientWidth={width}>
    <Grid 
        {columns}
        {data}
        search=true
    />
</div>

<style global>
    /* @import "https://cdn.jsdelivr.net/npm/gridjs/dist/theme/mermaid.min.css"; */
    @import '$css/aqhi-legend.css';
    @import '$css/gridjs.css';
</style>