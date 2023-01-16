<script>

    import FusionCharts from 'fusioncharts';
    import Charts from 'fusioncharts/fusioncharts.charts';
    import FusionTheme from 'fusioncharts/themes/fusioncharts.theme.fusion';
    import SvelteFC, { fcRoot } from 'svelte-fusioncharts';

    export let labelSeries = [];
    export let gasSeries = [];
    export let daySeries = [];
    export let nightSeries = [];

    fcRoot(FusionCharts, Charts, FusionTheme);
    const dataSource = {
    "chart": {
        "caption": "Electricity and Gas consumption",
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
            "seriesname": "Electricity (Day)",
            "data": daySeries
        },
        {
            "seriesname": "Electricity (Night)",
            "data": nightSeries
        },
        {
            "seriesname": "Gas",
            "data": gasSeries
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

    // console.log(labelSeries);
    // console.log(gasSeries)
</script>

<div class="container flex justify-center mt-1 mb-1">
    <SvelteFC {...chartConfigs} />
</div>