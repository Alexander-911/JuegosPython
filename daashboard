<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard de Control de Gastos</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            color: #333;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            min-height: 100vh;
        }

        header {
            background-color: #007BFF;
            color: white;
            text-align: center;
            padding: 20px;
        }

        header h1 {
            margin: 0;
            font-size: 2em;
        }

        .charts {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            gap: 20px;
            margin-top: 30px;
        }

        .chart-container {
            width: 48%;
        }

        .update-form {
            margin-top: 30px;
            display: flex;
            gap: 20px;
            justify-content: center;
        }

        .update-form input, .update-form button {
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
            font-size: 1em;
        }

        .update-form button {
            background-color: #007BFF;
            color: white;
            cursor: pointer;
        }

        .update-form button:hover {
            background-color: #0056b3;
        }

        .notification {
            background-color: #28a745;
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            text-align: center;
            margin-top: 20px;
            display: none;
        }

        .saldo {
            margin-top: 20px;
            font-size: 1.5em;
            font-weight: bold;
            text-align: center;
        }

        .flow-inputs {
            display: flex;
            gap: 20px;
            justify-content: center;
            margin-top: 20px;
        }

        .flow-inputs input {
            padding: 10px;
            width: 80px;
            font-size: 1em;
        }

        .flow-inputs button {
            padding: 10px 20px;
            font-size: 1em;
            background-color: #007BFF;
            color: white;
            cursor: pointer;
            border-radius: 5px;
        }

        .flow-inputs button:hover {
            background-color: #0056b3;
        }

    </style>
</head>
<body>
    <div id="app">
        <header>
            <h1>Dashboard de Control de Gastos</h1>
        </header>

        <div class="charts">
            <div class="chart-container">
                <canvas id="pieChart"></canvas>
            </div>

            <div class="chart-container">
                <canvas id="barChart"></canvas>
            </div>

            <div class="chart-container">
                <canvas id="flowChart"></canvas>
            </div>
        </div>

        <div id="saldo" class="saldo">
            Dinero Sobrante: Q0.00
        </div>

        <div class="update-form">
            <input type="number" id="input-transporte" placeholder="Gastos Transporte (Q)" oninput="updateCharts()">
            <input type="number" id="input-renta" placeholder="Gastos Renta (Q)" oninput="updateCharts()">
            <input type="number" id="input-luz" placeholder="Gastos Luz (Q)" oninput="updateCharts()">
            <input type="number" id="input-agua" placeholder="Gastos Agua (Q)" oninput="updateCharts()">
            <input type="number" id="input-varios" placeholder="Gastos Varios (Q)" oninput="updateCharts()">
            <input type="number" id="input-ingresos" placeholder="Ingresos (Q)" oninput="updateCharts()">
            <button onclick="updateCharts()">Actualizar Gráficos</button>
        </div>

        <div class="flow-inputs">
            <input type="number" id="input-flujo-gastos-enero" placeholder="Enero Gastos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-gastos-febrero" placeholder="Febrero Gastos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-gastos-marzo" placeholder="Marzo Gastos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-gastos-abril" placeholder="Abril Gastos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-gastos-mayo" placeholder="Mayo Gastos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-gastos-junio" placeholder="Junio Gastos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-ingresos-enero" placeholder="Enero Ingresos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-ingresos-febrero" placeholder="Febrero Ingresos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-ingresos-marzo" placeholder="Marzo Ingresos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-ingresos-abril" placeholder="Abril Ingresos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-ingresos-mayo" placeholder="Mayo Ingresos" oninput="updateFlowChart()">
            <input type="number" id="input-flujo-ingresos-junio" placeholder="Junio Ingresos" oninput="updateFlowChart()">
            <button onclick="updateFlowChart()">Actualizar Flujo</button>
        </div>

        <div id="notification" class="notification">¡Datos actualizados correctamente!</div>
    </div>

    <script>
        const months = ['Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo', 'Junio'];

        let gastosMeses = {
            enero: 0, febrero: 0, marzo: 0, abril: 0, mayo: 0, junio: 0
        };
        let ingresosMeses = {
            enero: 0, febrero: 0, marzo: 0, abril: 0, mayo: 0, junio: 0
        };

        // Inicializamos los gráficos
        let pieChart = new Chart(document.getElementById('pieChart'), {
            type: 'pie',
            data: {
                labels: ['Transporte', 'Renta', 'Luz', 'Agua', 'Varios'],
                datasets: [{
                    label: 'Distribución de Gastos',
                    data: [0, 0, 0, 0, 0],
                    backgroundColor: ['#FF5733', '#33FF57', '#3357FF', '#FF33A1', '#A133FF'],
                    borderColor: ['#FF5733', '#33FF57', '#3357FF', '#FF33A1', '#A133FF'],
                    borderWidth: 1
                }]
            }
        });

        let barChart = new Chart(document.getElementById('barChart'), {
            type: 'bar',
            data: {
                labels: ['Gastos', 'Ingresos'],
                datasets: [{
                    label: 'Monto Total',
                    data: [0, 0],
                    backgroundColor: ['#FF5733', '#28a745'],
                    borderColor: ['#FF5733', '#28a745'],
                    borderWidth: 1
                }]
            }
        });

        let flowChart = new Chart(document.getElementById('flowChart'), {
            type: 'line',
            data: {
                labels: months,
                datasets: [
                    {
                        label: 'Flujo de Gastos',
                        data: new Array(months.length).fill(0),
                        borderColor: '#FF5733',
                        fill: false,
                        tension: 0.4
                    },
                    {
                        label: 'Flujo de Ingresos',
                        data: new Array(months.length).fill(0),
                        borderColor: '#28a745',
                        fill: false,
                        tension: 0.4
                    }
                ]
            }
        });

        function updateCharts() {
            // Obtener los valores de los inputs y actualizar los gastos e ingresos
            gastosMeses['enero'] = parseFloat(document.getElementById('input-transporte').value) || 0;
            gastosMeses['febrero'] = parseFloat(document.getElementById('input-renta').value) || 0;
            gastosMeses['marzo'] = parseFloat(document.getElementById('input-luz').value) || 0;
            gastosMeses['abril'] = parseFloat(document.getElementById('input-agua').value) || 0;
            gastosMeses['mayo'] = parseFloat(document.getElementById('input-varios').value) || 0;
            ingresosMeses['enero'] = parseFloat(document.getElementById('input-ingresos').value) || 0;

            // Actualizar gráfico de pastel
            pieChart.data.datasets[0].data = [
                gastosMeses['enero'], 
                gastosMeses['febrero'], 
                gastosMeses['marzo'], 
                gastosMeses['abril'], 
                gastosMeses['mayo']
            ];
            pieChart.update();

            // Actualizar gráfico de barras
            const totalGastos = Object.values(gastosMeses).reduce((sum, value) => sum + value, 0);
            barChart.data.datasets[0].data = [totalGastos, ingresosMeses['enero']];
            barChart.update();
        }

        function updateFlowChart() {
            // Obtener los valores de flujo de gastos e ingresos
            const flujoGastos = [
                parseFloat(document.getElementById('input-flujo-gastos-enero').value) || 0,
                parseFloat(document.getElementById('input-flujo-gastos-febrero').value) || 0,
                parseFloat(document.getElementById('input-flujo-gastos-marzo').value) || 0,
                parseFloat(document.getElementById('input-flujo-gastos-abril').value) || 0,
                parseFloat(document.getElementById('input-flujo-gastos-mayo').value) || 0,
                parseFloat(document.getElementById('input-flujo-gastos-junio').value) || 0
            ];
            
            const flujoIngresos = [
                parseFloat(document.getElementById('input-flujo-ingresos-enero').value) || 0,
                parseFloat(document.getElementById('input-flujo-ingresos-febrero').value) || 0,
                parseFloat(document.getElementById('input-flujo-ingresos-marzo').value) || 0,
                parseFloat(document.getElementById('input-flujo-ingresos-abril').value) || 0,
                parseFloat(document.getElementById('input-flujo-ingresos-mayo').value) || 0,
                parseFloat(document.getElementById('input-flujo-ingresos-junio').value) || 0
            ];

            // Actualizar gráfico de flujo
            flowChart.data.datasets[0].data = flujoGastos;
            flowChart.data.datasets[1].data = flujoIngresos;
            flowChart.update();
        }

        updateCharts();
        updateFlowChart();

    </script>
</body>
</html>
