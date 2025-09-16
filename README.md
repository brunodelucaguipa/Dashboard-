<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GUIPA - Campanha Nescafé</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0a0a0a;
            color: #e5e7eb;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
        }
        .card {
            background-color: #1c1c1c;
            border-radius: 0.75rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
            padding: 1.5rem;
            transition: transform 0.2s ease-in-out;
        }
        .card:hover {
            transform: translateY(-5px);
        }
        .chart-container {
            background-color: #1c1c1c;
            border-radius: 0.75rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
            padding: 1rem;
            height: 350px;
        }
        h1, h2, h3 {
            color: #f3f4f6;
        }
        .day-button {
            background-color: #262626;
            color: #f3f4f6;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 0.5rem;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        .day-button:hover {
            background-color: #404040;
        }
        .day-button.active {
            background-color: #f97316;
            color: #0a0a0a;
            font-weight: bold;
        }
    </style>
</head>
<body class="bg-zinc-950 text-zinc-200">

<div class="container mx-auto py-8">
    <header class="text-center mb-12">
        <h1 class="text-4xl font-bold mb-2 text-zinc-100">GUIPA - Campanha Nescafé</h1>
        <p class="text-zinc-400 text-lg">Análise de ativações (POA)</p>
    </header>

    <!-- Cards de Resumo -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-12">
        <div class="card text-center">
            <h3 class="text-xl font-semibold text-orange-400">Ativações Totais</h3>
            <p id="totalActivations" class="text-5xl font-bold mt-2">12</p>
        </div>
        <div class="card text-center">
            <h3 class="text-xl font-semibold text-orange-500">Dias de Campanha</h3>
            <p id="campaignDays" class="text-5xl font-bold mt-2">6</p>
        </div>
        <div class="card text-center">
            <h3 class="text-xl font-semibold text-orange-600">Garrafas Distribuídas</h3>
            <p id="totalBottles" class="text-5xl font-bold mt-2">917</p>
        </div>
    </div>

    <!-- Gráficos -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <div class="chart-container">
            <h3 class="text-lg font-semibold mb-4 text-center">Ativações por Território</h3>
            <canvas id="territoryActivationsChart"></canvas>
        </div>
        <div class="chart-container">
            <h3 class="text-lg font-semibold mb-4 text-center">Garrafas Distribuídas por Território</h3>
            <canvas id="territoryBottlesChart"></canvas>
        </div>
        <div class="chart-container">
            <h3 class="text-lg font-semibold mb-4 text-center">Distribuição Diária de Garrafas</h3>
            <div class="flex flex-wrap justify-center gap-2 mb-4">
                <button class="day-button" data-date="08/09/2025">08/09</button>
                <button class="day-button" data-date="09/09/2025">09/09</button>
                <button class="day-button" data-date="10/09/2025">10/09</button>
                <button class="day-button" data-date="11/09/2025">11/09</button>
                <button class="day-button" data-date="12/09/2025">12/09</button>
                <button class="day-button" data-date="15/09/2025">15/09</button>
                <button class="day-button" data-date="16/09/2025">16/09</button>
                <button class="day-button" data-date="17/09/2025">17/09</button>
                <button class="day-button" data-date="18/09/2025">18/09</button>
                <button class="day-button" data-date="19/09/2025">19/09</button>
                <button class="day-button" data-date="22/09/2025">22/09</button>
                <button class="day-button" data-date="23/09/2025">23/09</button>
                <button class="day-button" data-date="24/09/2025">24/09</button>
                <button class="day-button" data-date="25/09/2025">25/09</button>
                <button class="day-button" data-date="26/09/2025">26/09</button>
                <button class="day-button" data-date="29/09/2025">29/09</button>
                <button class="day-button" data-date="30/09/2025">30/09</button>
                <button class="day-button" data-date="01/10/2025">01/10</button>
                <button class="day-button" data-date="02/10/2025">02/10</button>
                <button class="day-button" data-date="03/10/2025">03/10</button>
            </div>
            <p id="dailyBottleCount" class="text-center text-4xl font-bold text-orange-400 mt-4">Clique em uma data</p>
        </div>
    </div>

</div>

<script>
    // Dados fixos fornecidos pelo usuário
    const activations = {
        'Centro empresarial': 8,
        'Faculdade': 1,
        'Academia': 3
    };

    const bottlesDistributed = {
        'Academia': 237,
        'Centro empresarial': 575,
        'Faculdade': 105
    };

    const dailyData = {
        '08/09/2025': 60,
        '09/09/2025': 144,
        '10/09/2025': 210,
        '11/09/2025': 216,
        '12/09/2025': 137,
        '15/09/2025': 150,
        '16/09/2025': 0,
        '17/09/2022': 0,
        '18/09/2025': 0,
        '19/09/2025': 0,
        '22/09/2025': 0,
        '23/09/2025': 0,
        '24/09/2025': 0,
        '25/09/2025': 0,
        '26/09/2025': 0,
        '29/09/2025': 0,
        '30/09/2025': 0,
        '01/10/2025': 0,
        '02/10/2025': 0,
        '03/10/2025': 0,
    };

    // Card data
    document.getElementById('totalActivations').innerText = 12;
    document.getElementById('campaignDays').innerText = 6;
    document.getElementById('totalBottles').innerText = 917;

    // Chart configurations
    const primaryColor = '#f97316';
    const secondaryColor = '#ea580c';
    const tertiaryColor = '#fb923c';
    const fourthColor = '#fcd34d';
    const colors = [primaryColor, secondaryColor, tertiaryColor, fourthColor, '#fdba74', '#fed7aa', '#c2410c'];

    function createBarChart(chartId, labels, data, label) {
        const ctx = document.getElementById(chartId).getContext('2d');
        new Chart(ctx, {
            type: 'bar',
            data: {
                labels: labels,
                datasets: [{
                    label: label,
                    data: data,
                    backgroundColor: colors,
                    borderColor: '#1c1c1c',
                    borderWidth: 1
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: {
                        display: false
                    },
                    tooltip: {
                        bodyColor: '#e5e7eb',
                        titleColor: '#f3f4f6'
                    }
                },
                scales: {
                    x: {
                        grid: { color: 'rgba(255, 255, 255, 0.1)' },
                        ticks: { color: '#e5e7eb' }
                    },
                    y: {
                        grid: { color: 'rgba(255, 255, 255, 0.1)' },
                        ticks: { color: '#e5e7eb' },
                        beginAtZero: true
                    }
                }
            }
        });
    }

    // Creating charts with the provided data
    createBarChart('territoryActivationsChart', Object.keys(activations), Object.values(activations), 'Ativações');
    createBarChart('territoryBottlesChart', Object.keys(bottlesDistributed), Object.values(bottlesDistributed), 'Garrafas');

    // Daily distribution button functionality
    const buttons = document.querySelectorAll('.day-button');
    const display = document.getElementById('dailyBottleCount');
    buttons.forEach(button => {
        button.addEventListener('click', () => {
            buttons.forEach(btn => btn.classList.remove('active'));
            button.classList.add('active');
            const date = button.dataset.date;
            const count = dailyData[date] !== undefined ? dailyData[date] : 'N/A';
            display.innerText = count + ' garrafas';
        });
    });
</script>

</body>
</html>

