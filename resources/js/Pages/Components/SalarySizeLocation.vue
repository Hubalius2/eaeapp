<template>
    <div class="bg-white shadow-md border border-gray-200 py-4 px-4 rounded-lg">
        <h3 class="pb-3">Salario / Tamaño Empresa</h3>
        <PolarAreaChart :chartData="chartData" :options="options"  />
    </div>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue';
import { PolarAreaChart } from 'vue-chart-3';
import { Chart, registerables } from "chart.js";
import axios from 'axios';
import Utils from "Utils";

Chart.register(...registerables);

export default defineComponent({
    name: 'ScatterChart',
    components: { PolarAreaChart },
    setup() {

        const chartData = ref({});


        const fetchData = async () => {
            try {
                const response = await axios.get('https://h77v0f7om6.execute-api.us-east-1.amazonaws.com/dev/salaries/country_size');
                const data = JSON.parse(response.data.data);
                const salaries = data.map(item => item.average_salary);
                const size = data.map(item => item.company_size);

                chartData.value = {
                    labels: size,
                    datasets: [
                        {
                            data: salaries,
                            backgroundColor: [             
                                'rgb(119, 206, 255, 0.5)',
                                'rgb(0, 121, 175, 0.5)',
                                'rgb(18, 62, 107,  0.5)',
                            ]
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
            plugins: {
                legend: {
                    display: true
                }
            }
        }

        return { chartData, options };
    },
});
</script>
