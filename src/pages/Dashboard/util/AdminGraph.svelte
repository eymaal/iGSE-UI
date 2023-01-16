<script>
    import { onMount } from "svelte";
    import FusionCharts from 'fusioncharts';
    import Charts from 'fusioncharts/fusioncharts.charts';
    import FusionTheme from 'fusioncharts/themes/fusioncharts.theme.fusion';
    import SvelteFC, { fcRoot } from 'svelte-fusioncharts';
    import AdminGraphTotal from "./AdminGraphTotal.svelte";

    let customer_id;
    let averageSeries = [];
    let labelSeries = [];
    let daySeries = [];
    let nightSeries = [];
    let gasSeries = [];
    let counter=0;

    onMount(() => {
        customer_id = JSON.parse(localStorage.getItem('customer')).customer_id;
        getConsumptionData();
    })

    let getConsumptionData = async () => {
        const res = await fetch('http://localhost:8080/iGSE/admin/stats?customer_id='+customer_id,{
            method: 'GET',
            headers: {
            'Content-Type': 'application/json'
            }
        })
        .then((response) => {
            return response.json();
        })
        .then((data) => {
            // console.log(data);
            Object.keys(data).forEach(label => {
                let customer = {};
                customer['label'] = label;
                labelSeries.push(customer);
            })
            Object.values(data).forEach(v => {
                let avg = {};
                let day = {};
                let night = {};
                let gas = {};
                avg['value'] = v.averageUnits;
                day['value'] = v.dayUnits;
                night['value'] = v.nightUnits;
                gas['value'] = v.gasUnits;
                averageSeries.push(avg);
                daySeries.push(day);
                nightSeries.push(night);
                gasSeries.push(gas);
                counter+=1;
            })
            
        })
        // console.log(labelSeries);
        // console.log(averageSeries);
    }
      
    // Always set FusionCharts as the first parameter
    fcRoot(FusionCharts, Charts, FusionTheme);
    // alert(averageSeries);
    const dataSource = {
    "chart": {
        "caption": "Average Gas and Electricity Consumption per day",
        "yaxisname": "Consumption in kWh",
        "subcaption": "(in kWh)",
        "flatscrollbars": "0",
        "scrollheight": "12",
        "numvisibleplot": "8",
        "plottooltext": "<b>$dataValue</b> $seriesName consumption by $label",
        "theme": "fusion"
    },
    "categories": [
        {
            "category": labelSeries
        }
    ],
    "dataset": [
        {
            "seriesname": "Average Electricity and Gas Consumption",
            "data": averageSeries
        }
    ]
    };

    const chartConfigs = {
    type: 'scrollstackedcolumn2d',
    width: 600,
    height: 400,
    dataFormat: 'json',
    dataSource
    };

    // FusionCharts.ready(function (FusionCharts) {
    //     var Chart = new FusionCharts({
    //         type: 'scrollstackedcolumn2d',
    //         width: 600,
    //         height: 400,
    //         renderAt: 'Graphs',
    //         dataFormat: 'json',
    //         dataSource
    //     });
    //     Chart.render();
    // })        
    
</script>

<div id="Graphs">
    <!-- <button class = "btn btn-primary" on:click={getConsumptionData}>Load Graphs</button> -->
    {#key counter}
        <!-- {console.log("rendering")} -->
        <div class="container flex justify-center mt-1 mb-1">
            <SvelteFC {...chartConfigs} id="chart"/>
        </div>
        <AdminGraphTotal labelSeries={labelSeries} daySeries={daySeries} nightSeries={nightSeries} gasSeries={gasSeries}/>
    {/key}
</div>
