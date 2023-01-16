<script>
    import { onMount } from "svelte";

    let customer_id;

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
            console.log(data);
        })
    }
      


    import FusionCharts from 'fusioncharts';
    import Charts from 'fusioncharts/fusioncharts.charts';
    import FusionTheme from 'fusioncharts/themes/fusioncharts.theme.fusion';
    import SvelteFC, { fcRoot } from 'svelte-fusioncharts';

    // Always set FusionCharts as the first parameter
    fcRoot(FusionCharts, Charts, FusionTheme);

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
        "category": [
            {
            "label": "1994"
            },
            {
            "label": "1995"
            }            
        ]
        }
    ],
    "dataset": [
        {
        "seriesname": "Diptera",
        "data": [
            {
            "value": "3622"
            },
            {
            "value": "2612"
            }            
        ]
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
</script>

<div id="container">
    <SvelteFC {...chartConfigs} />
  </div>
