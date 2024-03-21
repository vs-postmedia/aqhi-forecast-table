<script>
    export let data = [];

    // LIBS
    import { onMount } from 'svelte';
    import Grid from 'gridjs-svelte'
	import { html } from 'gridjs';

    // COMPONENTS
    // import Tooltip from '$components/Tooltip.svelte';

    // VARS
    let tooltipData;
    let width = 500;
    let tableHeight = '600px';
    const colourLookup = ['#00CCFF','#0099CC','#006699','#FFFF00','#FFCC00','#FF9933','#FF6666','#FF0000','#CC0000','#990000','#660000']; // AQHI COLOUR VALUES
    const textLookup = ['Low','Low','Low','Moderate','Moderate','Moderate','Moderate', 'High','High','High','Very high'];
   
    $: columns = [
        {   
            name: 'Name',
            maxWidth: '110px',
            formatter: cell => html(`<b>${cell}</b>`)
        },
        {
            name: 'Today', id: 'today', maxWidth: '90px',
            attributes: cell => assignColour(cell),
            // data: cell => cellText(cell, 'today'),
            formatter: cell => formatCell(cell),
        },
        {
            name: 'Tonight', id: 'tonight', maxWidth: '90px',
            attributes: cell => assignColour(cell),
            formatter: cell => formatCell(cell)
        },
        {
            name: 'Tmrw', id: 'tomorrow', maxWidth: '90px',
            attributes: cell => assignColour(cell),
            formatter: cell => formatCell(cell)
        },
        {
            name: 'Tmrw night', id: 'tomorrow_night', maxWidth: '90px',
            attributes: cell => assignColour(cell),
            formatter: cell => formatCell(cell)
        }
    ];

    // FUNCTIONS
    function formatCell(cell) {
        // console.log(data, forecast)
        const text = textLookup[parseInt(cell) - 1];
        const textColor = parseInt(cell) > 3 && parseInt(cell) < 7 ? '#000' : '#FFF';

        return html(`
            <h2 style="color:${textColor}">${cell}</h2>
            <p style="color:${textColor}">${text}</p>
        `);
    }
    function assignColour(cell) {
        let style;
            if (cell) {
                style = {
                    'style': 
                        `background-color: ${lookupColour(cell)};
                        text-align:center;`
                }
            }

        return style;
    }
    function lookupColour(aqhi) {
        // console.log(aqhi)
        const score = aqhi === '+' ? 11 : parseInt(aqhi);

        return colourLookup[score - 1];
    }

    function init() {
        console.log('CHART INIT!')
    }

    // LIGHTS! CAMERA! ACTION!
    onMount(init);
</script>


<div class="chart-container" bind:clientWidth={width}>
    <Grid 
        {columns}
        {data}
        search=true
        width='100%'
    />
</div>

<style global>
    /* @import "https://cdn.jsdelivr.net/npm/gridjs/dist/theme/mermaid.min.css"; */
    @import '$css/gridjs.css';

    .gridjs .gridjs-td h2 {
        font-size: 3rem;
    }
</style>