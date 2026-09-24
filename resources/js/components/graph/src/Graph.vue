<template>
    <div class="chart-container">
        <canvas ref="canvas"></canvas>
    </div>
</template>

<style>
    .chart-container {
        position: relative;
        margin: auto;
        height: 260px;
        width: 190px;
    }
    .chart-container .chartjs-render-monitor{
        height: inherit!important;
    }
</style>

<script>
    import Chart from 'chart.js';

    export default {
        props: ['type', 'allData'],
        data() {
            return {
                chart: null,
                options: {
                    maintainAspectRatio: false,
                    lineTension: 0,
                    legend: {
                        display: false,
                    }
                },
            }
        },
        created() {
            this.title = 'Comprobantes';
        },
        mounted() {
        },
        watch: {
            allData() {
                this.createChart();
            }
        },
        methods: {
            getChartDatasets() {
                const datasets = (this.allData && this.allData.datasets) || [];
                const root = document.documentElement;
                if (!root.classList.contains('skin-bocado')) return datasets;

                const styles = window.getComputedStyle(root);
                const palette = [
                    styles.getPropertyValue('--bocado-primary').trim() || '#cc0033',
                    styles.getPropertyValue('--bocado-primary-dark').trim() || '#c10016',
                    '#d39a45',
                    '#7b1f3d',
                    '#a84b63'
                ];
                const colorAt = index => palette[index % palette.length];
                // "Total cobrado" comparte el color de la serie "Total".
                const totalColor = palette[2];

                return datasets.map((dataset, index) => {
                    const themed = Object.assign({}, dataset);
                    const isSaleNotes = dataset.label === 'Notas de venta';
                    const isTotal = dataset.label === 'Total';
                    if (Array.isArray(themed.backgroundColor)) {
                        themed.backgroundColor = themed.backgroundColor.map((color, colorIndex) =>
                            isSaleNotes && colorIndex === 0 ? totalColor : colorAt(colorIndex)
                        );
                    } else if (themed.backgroundColor) {
                        themed.backgroundColor = isTotal ? totalColor : colorAt(index);
                    }
                    if (Array.isArray(themed.borderColor)) {
                        themed.borderColor = themed.borderColor.map((color, colorIndex) => colorAt(colorIndex));
                    } else if (themed.borderColor) {
                        themed.borderColor = isTotal ? totalColor : colorAt(index);
                    }
                    return themed;
                });
            },
            createChart() {
                if (this.chart) {
                    this.chart.destroy();
                }
                this.chart = new Chart(this.$refs.canvas.getContext('2d'), {
                    type: this.type,
                    data: {
                        labels: this.allData.labels,
                        datasets: this.getChartDatasets(),
                    },
                    options: this.options,
                });

            }
        }
    }
</script>
