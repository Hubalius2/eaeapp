<template>
    <div class="bg-white shadow-md border border-gray-200 py-4 px-4 rounded-lg">
        <h3 class="pb-3">Salario / Experiencia</h3>
        <BarChart :chartData="chartData" :options="options"  />
    </div>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue';
import { BarChart } from 'vue-chart-3';
import { Chart, registerables } from "chart.js";
import axios from 'axios';
Chart.register(...registerables);

export default defineComponent({
    name: 'RemoteRatio',
    components: { BarChart },
    setup() {

        const chartData = ref({});

        const numberToColor = function(number, min, max) {
            let normalized = (number - min) / (max - min);
            let hue = (250 + normalized * (-60)).toFixed(1);
            return `hsl(${hue}, 70%, 50%)`;
        }

        const fetchData = async () => {
            try {
                const response = await axios.get('https://h77v0f7om6.execute-api.us-east-1.amazonaws.com/dev/salaries/level');
                const data = JSON.parse(response.data.data);
                const labels = data.map(item => item.experience_level);
                const values = data.map(item => item.average_salary);
                const min = Math.min(...values);
                const max = Math.max(...values);
                const colors = values.map(item => numberToColor(item, min, max));
                const sizes = values.map(item => Math.sqrt(item));

                chartData.value = {
                    labels: labels,
                    datasets: [
                        {
                            label: 'Salarios por Año',
                            data: values.map((value, index) => ({ x: index, y: value, r: sizes[index] })), // Cada burbuja tiene una coordenada x (index), coordenada y (valor), y tamaño (r)
                            //backgroundColor: colors
                            //borderColor:colors,
                            backgroundColor: colors,
                            borderWidth: 2,
                            borderRadius: Number.MAX_VALUE,
                            borderSkipped: false,
                        },
                    ],
                }

            } catch (error) {
                console.log(error);
            }
        }

        onMounted(() => {
            fetchData();
        });

        const options = {
            indexAxis: 'x',
            plugins: {
                legend: {
                    display: false
                }
            }
        }

        return { chartData, options };
    },
});
</script>
