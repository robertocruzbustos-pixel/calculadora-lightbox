<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Costos Lightbox 3D</title>
    
    <!-- Tailwind CSS CDN para un diseño responsive y moderno -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7f9fb;
        }
        .section-header {
            border-left: 4px solid #3b82f6;
            padding-left: 1rem;
        }
        input[type="number"] {
            /* Ocultar flechas en inputs de número (Chrome/Edge/Safari) */
            appearance: none;
            /* Ocultar flechas en inputs de número (Firefox) */
            -moz-appearance: textfield;
        }
        .card {
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.06);
        }
        .calculated-value {
            background-color: #e0f2fe; /* Fondo azul claro para claridad */
            color: #0b69a6;
            padding: 0.5rem;
            border-radius: 0.5rem;
            font-weight: 600;
            margin-top: 0.5rem;
            display: block;
        }
        /* Estilo para el botón de eliminar, usa SVG */
        .remove-btn {
            background: none;
            border: none;
            cursor: pointer;
            padding: 0.25rem;
            border-radius: 0.375rem;
        }
        .remove-btn:hover {
            background-color: #fee2e2;
        }
    </style>
</head>
<body class="p-4 md:p-8">

    <div id="app" class="max-w-4xl mx-auto">
        <h1 class="text-3xl font-bold text-gray-800 mb-6">LightBox 3D: Calculadora de Costos</h1>

        <!-- Pestañas de Navegación -->
        <div class="flex border-b border-gray-200 mb-6">
            <button onclick="showSection('base-costs')" class="tab-btn p-3 text-sm font-medium text-gray-600 border-b-2 border-transparent hover:border-blue-500 transition duration-150 ease-in-out" data-target="base-costs">
                1. Costos Base (Fijos)
            </button>
            <button onclick="showSection('model-input')" class="tab-btn p-3 text-sm font-medium text-gray-600 border-b-2 border-transparent hover:border-blue-500 transition duration-150 ease-in-out" data-target="model-input">
                2. Consumo por Modelo
            </button>
            <button onclick="showSection('results')" class="tab-btn p-3 text-sm font-medium text-gray-600 border-b-2 border-blue-500 text-blue-600" data-target="results">
                3. Resultado Final
            </button>
        </div>

        <!-- 1. Sección de Costos Base (Hoja 1 y 2) -->
        <div id="base-costs" class="tab-content hidden space-y-6">
            <div class="card bg-white p-6 rounded-lg">
                <h2 class="text-xl font-semibold text-gray-700 section-header mb-4">A. Materiales Unitarios (Por Medida)</h2>
                <p class="text-sm text-gray-500 mb-4">Ingresa el costo total de compra y la cantidad para calcular el costo por unidad de medida.</p>
                
                <!-- Filamento PLA -->
                <div class="space-y-2 mb-4 p-3 border rounded-lg">
                    <h3 class="font-medium text-gray-700">Filamento PLA (Gramos)</h3>
                    <label class="block text-sm text-gray-600">Costo de Compra (Total ARS):</label>
                    <input type="number" id="input-pla-total" value="30000" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <label class="block text-sm text-gray-600">Cantidad Comprada (Gramos):</label>
                    <input type="number" id="input-pla-qty" value="1000" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <span class="calculated-value" id="calc-pla-gram">Costo/g: $ 30.00</span>
                </div>

                <!-- Tira LED Blanco -->
                <div class="space-y-2 mb-4 p-3 border rounded-lg">
                    <h3 class="font-medium text-gray-700">Tira LED Blanco (Metros comprados)</h3>
                    <p class="text-xs text-gray-500">Se asume una densidad de 120 LEDs por metro para el cálculo unitario.</p>
                    <label class="block text-sm text-gray-600">Costo de Compra (Total ARS):</label>
                    <input type="number" id="input-led-total" value="1250" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <label class="block text-sm text-gray-600">Cantidad Comprada (Metros):</label>
                    <input type="number" id="input-led-qty" value="5" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <span class="calculated-value" id="calc-led-m">Costo/LED (120/m): $ X.XX</span>
                </div>
                
                <!-- Tira LED RGB -->
                <div class="space-y-2 mb-4 p-3 border rounded-lg">
                    <h3 class="font-medium text-gray-700">Tira LED RGB (Metros comprados)</h3>
                    <p class="text-xs text-gray-500">Se asume una densidad de 60 LEDs por metro para el cálculo unitario.</p>
                    <label class="block text-sm text-gray-600">Costo de Compra (Total ARS):</label>
                    <input type="number" id="input-led-rgb-total" value="1750" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <label class="block text-sm text-gray-600">Cantidad Comprada (Metros):</label>
                    <input type="number" id="input-led-rgb-qty" value="5" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <span class="calculated-value" id="calc-led-rgb-m">Costo/LED (60/m): $ X.XX</span>
                </div>

                <!-- Fuente 12V -->
                <div class="space-y-2 mb-4 p-3 border rounded-lg">
                    <h3 class="font-medium text-gray-700">Fuente 12V (Unidades)</h3>
                    <label class="block text-sm text-gray-600">Costo de Compra (Total ARS):</label>
                    <input type="number" id="input-ps-total" value="1200" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <label class="block text-sm text-gray-600">Cantidad Comprada (Unidades):</label>
                    <input type="number" id="input-ps-qty" value="1" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <span class="calculated-value" id="calc-ps-unit">Costo/u: $ 1200.00</span>
                </div>
                
                <!-- Embalaje/Caja -->
                <div class="space-y-2 p-3 border rounded-lg">
                    <h3 class="font-medium text-gray-700">Embalaje/Caja (Unidades)</h3>
                    <label class="block text-sm text-gray-600">Costo de Compra (Total ARS):</label>
                    <input type="number" id="input-pkg-total" value="500" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <label class="block text-sm text-gray-600">Cantidad Comprada (Unidades):</label>
                    <input type="number" id="input-pkg-qty" value="1" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <span class="calculated-value" id="calc-pkg-unit">Costo/u: $ 500.00</span>
                </div>
            </div>

            <div class="card bg-white p-6 rounded-lg">
                <h2 class="text-xl font-semibold text-gray-700 section-header mb-4">B. Costos Fijos y de Producción (Derivados)</h2>
                
                <!-- Mano de Obra Directa -->
                <div class="space-y-2 mb-4 p-3 border rounded-lg">
                    <h3 class="font-medium text-gray-700">Mano de Obra Directa (MOD)</h3>
                    <label class="block text-sm text-gray-600">Sueldo Asignado a M.O. (Mensual ARS):</label>
                    <input type="number" id="input-mo-salary" value="400000" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <label class="block text-sm text-gray-600">Horas de Trabajo Mensual:</label>
                    <input type="number" id="input-mo-hours" value="160" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <span class="calculated-value" id="calc-mo-min">Costo/min: $ 41.67</span>
                </div>

                <!-- Energía/Electricidad (Impresión) -->
                <div class="space-y-2 mb-4 p-3 border rounded-lg">
                    <h3 class="font-medium text-gray-700">Costo de Energía (Impresión)</h3>
                    <label class="block text-sm text-gray-600">Costo por kWh (ARS):</label>
                    <input type="number" id="input-energy-kwh" value="30" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <label class="block text-sm text-gray-600">Consumo de Impresora (kW):</label>
                    <input type="number" id="input-energy-kw" value="0.1" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <span class="calculated-value" id="calc-energy-min">Costo/min: $ 0.05</span>
                </div>

                <!-- Costos Fijos Indirectos (CIF) - Desglosado -->
                <div class="space-y-2 p-3 border rounded-lg">
                    <h3 class="font-medium text-gray-700 mb-2">Costos Fijos Asignados (CIF)</h3>
                    <p class="text-xs text-gray-500 mb-2">Ingresa el detalle de tus costos fijos mensuales. El total se calculará automáticamente y se prorrateará.</p>
                    
                    <!-- Detalle de Costos Fijos Mensuales -->
                    <div class="space-y-2 border-l-2 border-indigo-300 pl-3 py-1">
                        <label class="block text-sm text-gray-600">Renta o Hipoteca (ARS):</label>
                        <input type="number" id="input-cif-renta" value="10000" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                        
                        <label class="block text-sm text-gray-600">Servicios (Agua, Internet, etc. ARS):</label>
                        <input type="number" id="input-cif-servicios" value="3000" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                        
                        <label class="block text-sm text-gray-600">Impuestos y Licencias (ARS):</label>
                        <input type="number" id="input-cif-impuestos" value="2000" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    </div>

                    <label class="block text-sm text-gray-600 pt-2">Total Costos Fijos Mensuales (ARS):</label>
                    <!-- Campo calculado -->
                    <input type="number" id="input-cif-total" value="15000" disabled class="w-full p-2 border border-blue-300 rounded-lg bg-blue-50 font-bold text-gray-800">

                    <label class="block text-sm text-gray-600">Producción Estimada Mensual (Unidades):</label>
                    <input type="number" id="input-cif-production" value="100" oninput="updateBaseCostsDisplay()" class="w-full p-2 border border-gray-300 rounded-lg">
                    <span class="calculated-value" id="calc-cif-unit">CIF/Unidad: $ 150.00</span>
                </div>
            </div>
        </div>

        <!-- 2. Sección de Consumo por Modelo (Matriz de Consumo) -->
        <div id="model-input" class="tab-content hidden space-y-6">
            <h2 class="text-2xl font-semibold text-gray-700 section-header mb-4">2. Consumo por Modelo</h2>
            <p class="text-sm text-gray-500 mb-4">Define el consumo específico de cada modelo. Los costos base se toman de la pestaña anterior.</p>

            <div id="model-list" class="space-y-4">
                <!-- Los modelos se renderizarán aquí -->
            </div>

            <button onclick="addModel()" class="w-full py-2 px-4 border border-blue-500 text-blue-500 font-semibold rounded-lg hover:bg-blue-50 transition duration-150 ease-in-out">
                + Agregar Nuevo Modelo
            </button>
        </div>

        <!-- 3. Sección de Resultados (Resumen y Cálculo) -->
        <div id="results" class="tab-content space-y-6">
            <h2 class="text-2xl font-semibold text-gray-700 section-header mb-4">3. Resumen y Precios Finales</h2>

            <div class="card bg-white p-6 rounded-lg space-y-4">
                <label class="block">Modelo a Calcular:</label>
                <select id="select-model" onchange="displayResults()" class="w-full p-2 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500"></select>

                <div class="pt-4 border-t border-gray-200">
                    <label class="block font-medium">Margen de Ganancia Deseado (%)</label>
                    <input type="number" id="input-margin" value="35" oninput="displayResults()" class="w-full p-2 border border-gray-300 rounded-lg mb-4">

                    <label class="block font-medium">Comisiones/Impuestos por Venta (%)</label>
                    <input type="number" id="input-commissions" value="15" oninput="displayResults()" class="w-full p-2 border border-gray-300 rounded-lg">
                </div>
            </div>

            <div id="result-display" class="card bg-white p-6 rounded-lg">
                <!-- Resultados se muestran aquí -->
                <p class="text-center text-gray-500">Selecciona o agrega un modelo para ver el cálculo.</p>
            </div>
        </div>
    </div>

    <!-- Custom Modal for Alerts (replacing native alert/confirm) -->
    <div id="custom-modal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center p-4 z-50">
        <div class="bg-white p-6 rounded-lg shadow-2xl max-w-sm w-full">
            <h3 id="modal-title" class="text-xl font-semibold text-gray-800 mb-4">Atención</h3>
            <p id="modal-message" class="text-gray-600 mb-6">Mensaje de la aplicación.</p>
            <div class="flex justify-end space-x-3">
                <button onclick="closeModal()" class="px-4 py-2 bg-blue-500 text-white font-medium rounded-lg hover:bg-blue-600 transition duration-150">Cerrar</button>
            </div>
        </div>
    </div>

    <script>
        // --- CONSTANTES DE DENSIDAD DE LEDS (Necesarias solo para el cálculo de costo base unitario) ---
        const WHITE_LED_DENSITY = 120; // LEDs por metro en tira blanca
        const RGB_LED_DENSITY = 60;    // LEDs por metro en tira RGB
        const LEDS_PER_SEGMENT = 3;    // Los sectores de corte son de 3 LEDs

        // --- DATA STRUCTURE (MATRIZ DE CONSUMO) ---
        let models = [
            {
                id: Date.now(),
                name: "Modelo Básico (Pequeño)",
                ledType: 'white', 
                consumption: {
                    pla_grams: 80,
                    led_quantity: 20, // 20 sectores * 3 LEDs = 60 LEDs
                    mo_minutes: 15,
                    print_hours: 3,   
                    print_minutes: 0,
                }
            },
            {
                id: Date.now() + 1,
                name: "Modelo Grande (RGB)",
                ledType: 'rgb', 
                consumption: {
                    pla_grams: 200,
                    led_quantity: 24, // 24 sectores * 3 LEDs = 72 LEDs
                    mo_minutes: 25,
                    print_hours: 8,   
                    print_minutes: 0,
                }
            }
        ];

        // --- CORE LOGIC (VINCULACIÓN HOJAS) ---
        
        /**
         * Formatea un número como moneda ARS.
         * @param {number} value - El valor a formatear.
         * @returns {string} El valor formateado con '$' y dos decimales.
         */
        const formatCurrency = (value) => {
            return `$ ${Math.max(0, value).toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ",")}`;
        };

        /**
         * Muestra el modal personalizado con un mensaje de alerta.
         * @param {string} message - El mensaje a mostrar.
         * @param {string} title - El título del modal.
         */
        function customAlert(message, title = "Error de Aplicación") {
            document.getElementById('modal-title').innerText = title;
            document.getElementById('modal-message').innerText = message;
            document.getElementById('custom-modal').classList.remove('hidden');
            document.getElementById('custom-modal').classList.add('flex');
        }

        /** Cierra el modal personalizado. */
        function closeModal() {
            document.getElementById('custom-modal').classList.add('hidden');
            document.getElementById('custom-modal').classList.remove('flex');
        }


        /**
         * Calcula y actualiza los costos unitarios en la pestaña 1 (Costos Base).
         * @returns {Object} Los costos base unitarios calculados.
         */
        function calculateBaseUnitCosts() {
            // Función auxiliar para parsear y manejar errores (evita NaN)
            const getVal = (id) => parseFloat(document.getElementById(id)?.value) || 0;
            const safeDiv = (numerator, denominator) => denominator > 0 ? numerator / denominator : 0;

            // A. Materiales Unitarios
            const plaTotal = getVal('input-pla-total');
            const plaQty = getVal('input-pla-qty');
            const ledTotal = getVal('input-led-total');
            const ledQty = getVal('input-led-qty');
            const ledRgbTotal = getVal('input-led-rgb-total');
            const ledRgbQty = getVal('input-led-rgb-qty');
            const psTotal = getVal('input-ps-total');
            const psQty = getVal('input-ps-qty');
            const pkgTotal = getVal('input-pkg-total');
            const pkgQty = getVal('input-pkg-qty');

            const costPla = safeDiv(plaTotal, plaQty);
            const costPs = safeDiv(psTotal, psQty);
            const costPkg = safeDiv(pkgTotal, pkgQty);
            
            // CÁLCULO DE COSTO DE LED POR UNIDAD
            const totalWhiteLeds = ledQty * WHITE_LED_DENSITY; 
            const totalRgbLeds = ledRgbQty * RGB_LED_DENSITY;

            const costLedUnitWhite = safeDiv(ledTotal, totalWhiteLeds);
            const costLedUnitRgb = safeDiv(ledRgbTotal, totalRgbLeds);
            
            // B. Costos Fijos y de Producción
            const moSalary = getVal('input-mo-salary');
            const moHours = getVal('input-mo-hours');
            const energyKwh = getVal('input-energy-kwh');
            const energyKw = getVal('input-energy-kw');

            // ** CIF - Cálculo del total a partir del desglose **
            const cifRenta = getVal('input-cif-renta');
            const cifServicios = getVal('input-cif-servicios');
            const cifImpuestos = getVal('input-cif-impuestos');
            
            const cifTotalCalculated = cifRenta + cifServicios + cifImpuestos;
            document.getElementById('input-cif-total').value = cifTotalCalculated.toFixed(2); // Mostrar el total calculado

            const cifProduction = getVal('input-cif-production');

            // Costo por minuto de Mano de Obra
            const costMoMin = safeDiv(moSalary, (moHours * 60));
            // Costo de Energía por minuto: (Costo/kWh * Consumo/kW) / 60 minutos
            const costEnergyMin = safeDiv((energyKwh * energyKw), 60); 
            // Costo Fijo Indirecto por Unidad
            const costCifUnit = safeDiv(cifTotalCalculated, cifProduction);

            // Actualizar la interfaz de costos unitarios
            document.getElementById('calc-pla-gram').innerText = `Costo/g: ${formatCurrency(costPla)}`;
            document.getElementById('calc-led-m').innerText = `Costo/LED (120/m): ${formatCurrency(costLedUnitWhite)}`;
            document.getElementById('calc-led-rgb-m').innerText = `Costo/LED (60/m): ${formatCurrency(costLedUnitRgb)}`;
            document.getElementById('calc-ps-unit').innerText = `Costo/u: ${formatCurrency(costPs)}`;
            document.getElementById('calc-pkg-unit').innerText = `Costo/u: ${formatCurrency(costPkg)}`;
            document.getElementById('calc-mo-min').innerText = `Costo/min: ${formatCurrency(costMoMin)}`;
            document.getElementById('calc-energy-min').innerText = `Costo/min: ${formatCurrency(costEnergyMin)}`;
            document.getElementById('calc-cif-unit').innerText = `CIF/Unidad: ${formatCurrency(costCifUnit)}`;

            return {
                pla_gram: costPla,
                led_unit_white: costLedUnitWhite, 
                led_unit_rgb: costLedUnitRgb,   
                power_supply: costPs,
                packaging: costPkg,
                mo_min: costMoMin,
                energy_min: costEnergyMin,
                cif_unit: costCifUnit,
            };
        }

        /**
         * Obtiene todos los costos unitarios calculados y los márgenes.
         * @returns {Object} Costos unitarios y márgenes.
         */
        function getBaseCosts() {
            const calculatedCosts = calculateBaseUnitCosts();
            
            return {
                ...calculatedCosts,
                margin: (parseFloat(document.getElementById('input-margin')?.value) || 0) / 100,
                commissions: (parseFloat(document.getElementById('input-commissions')?.value) || 0) / 100,
            };
        }

        /**
         * Calcula el costo unitario total para un modelo específico.
         * @param {Object} model - Objeto del modelo con su consumo.
         * @param {Object} costs - Objeto con los costos base unitarios.
         * @returns {Object} Desglose y CUF (Costo Unitario de Fabricación).
         */
        function calculateModelCost(model, costs) {
            
            // 1. Determinar el costo por UNIDAD de LED
            const isRGB = model.ledType === 'rgb';
            const costPerLedUnit = isRGB ? costs.led_unit_rgb : costs.led_unit_white;

            // 2. Obtener la cantidad total de LEDs consumidos
            const totalLedsConsumed = model.consumption.led_quantity * LEDS_PER_SEGMENT;

            // 3. Obtener el tiempo total de impresión en minutos
            const totalPrintMinutes = (model.consumption.print_hours * 60) + model.consumption.print_minutes;

            // A. Costo Materiales Directos
            const costPla = model.consumption.pla_grams * costs.pla_gram;
            const costLeds = totalLedsConsumed * costPerLedUnit;
            const costMaterials = (
                costPla +
                costLeds + 
                costs.power_supply + 
                costs.packaging 
            );

            // B. Costos Variables de Producción (MOD + Energía)
            const costMOD = model.consumption.mo_minutes * costs.mo_min;
            const costEnergy = totalPrintMinutes * costs.energy_min;
            const costProduction = costMOD + costEnergy;

            // C. Costos Fijos Asignados (CIF)
            const costCIF = costs.cif_unit;

            // CUF: Costo Unitario de Fabricación
            const CUF = costMaterials + costProduction + costCIF;

            // D. Precio Final
            const suggestedPrice = CUF * (1 + costs.margin);
            // Fórmula: Precio Final = Precio Sugerido / (1 - Comisiones)
            const finalPrice = costs.commissions < 1 ? suggestedPrice / (1 - costs.commissions) : suggestedPrice;

            return {
                CUF,
                costPla,
                costLeds,
                costMaterials,
                costProduction,
                costMOD,
                costEnergy,
                costCIF,
                suggestedPrice,
                finalPrice,
            };
        }

        // --- RENDER FUNCTIONS ---

        /**
         * Renderiza la interfaz para ingresar el consumo de cada modelo.
         */
        function renderModelInputs() {
            const container = document.getElementById('model-list');
            container.innerHTML = ''; // Limpiar

            models.forEach(model => {
                const currentLedType = model.ledType;
                const modelCard = document.createElement('div');
                modelCard.className = 'card bg-white p-6 rounded-lg border-l-4 border-blue-500 space-y-3';
                
                modelCard.innerHTML = `
                    <div class="flex justify-between items-center mb-4">
                        <input type="text" id="name-${model.id}" value="${model.name}" oninput="updateModelName(${model.id}, this.value)" class="text-xl font-semibold text-gray-800 p-1 border-b border-gray-300 w-3/4">
                        <button onclick="removeModel(${model.id})" class="remove-btn" title="Eliminar Modelo">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-500 hover:text-red-700" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.86 10.36A2 2 0 0116.14 19H7.86a2 2 0 01-1.99-1.64L5 7m5-3h4a2 2 0 012 2v1m-6 0h6m-3 0V4"></path>
                            </svg>
                        </button>
                    </div>

                    <!-- Selector de Tipo de LED -->
                    <div class="space-y-2">
                        <label class="block text-sm font-medium text-gray-700">Tipo de LED (Afecta el costo base):</label>
                        <select id="led-type-${model.id}" onchange="updateModelLedType(${model.id}, this.value)" class="w-full p-2 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500">
                            <option value="white" ${currentLedType === 'white' ? 'selected' : ''}>Tira LED Blanco (120/m)</option>
                            <option value="rgb" ${currentLedType === 'rgb' ? 'selected' : ''}>Tira LED RGB (60/m)</option>
                        </select>
                    </div>

                    <label class="block pt-3">Filamento PLA (gramos):</label>
                    <input type="number" id="pla-${model.id}" value="${model.consumption.pla_grams}" oninput="updateModelConsumption(${model.id}, 'pla_grams', this.value)" class="w-full p-2 border border-gray-300 rounded-lg">
                    
                    <label class="block">Tira LED (cantidad de SECTORES de 3 LEDs):</label>
                    <input type="number" id="led-${model.id}" value="${model.consumption.led_quantity}" oninput="updateModelConsumption(${model.id}, 'led_quantity', this.value)" class="w-full p-2 border border-gray-300 rounded-lg">
                    <p class="text-xs text-gray-500 mt-1">Cada sector representa ${LEDS_PER_SEGMENT} LEDs. Ingresa el número de sectores de corte que lleva el modelo.</p>
                    
                    <label class="block pt-3 text-sm font-medium text-gray-700">Tiempos de Producción:</label>
                    
                    <label class="block text-sm text-gray-600">Mano de Obra Ensamblaje (minutos):</label>
                    <input type="number" id="mo-${model.id}" value="${model.consumption.mo_minutes}" oninput="updateModelConsumption(${model.id}, 'mo_minutes', this.value)" class="w-full p-2 border border-gray-300 rounded-lg">
                    
                    <!-- TIEMPO DE IMPRESIÓN (HORAS Y MINUTOS) -->
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm text-gray-600">Horas de Impresión:</label>
                            <input type="number" id="print-h-${model.id}" value="${model.consumption.print_hours}" oninput="updateModelConsumption(${model.id}, 'print_hours', this.value)" class="w-full p-2 border border-gray-300 rounded-lg">
                        </div>
                        <div>
                            <label class="block text-sm text-gray-600">Minutos de Impresión:</label>
                            <input type="number" id="print-m-${model.id}" value="${model.consumption.print_minutes}" oninput="updateModelConsumption(${model.id}, 'print_minutes', this.value)" class="w-full p-2 border border-gray-300 rounded-lg">
                        </div>
                    </div>
                `;
                container.appendChild(modelCard);
            });
            renderModelSelector();
            displayResults();
        }

        /**
         * Renderiza las opciones en el selector de modelos.
         */
        function renderModelSelector() {
            const selector = document.getElementById('select-model');
            // Guardar el ID seleccionado previamente
            const currentSelectedId = parseFloat(selector.value);
            
            selector.innerHTML = models.map(m => `<option value="${m.id}">${m.name}</option>`).join('');

            // Intentar re-seleccionar el modelo que estaba activo
            if (models.some(m => m.id === currentSelectedId)) {
                selector.value = currentSelectedId;
            } else if (models.length > 0) {
                selector.value = models[0].id;
            }
        }

        /**
         * Muestra el resultado del cálculo para el modelo seleccionado.
         */
        function displayResults() {
            const selector = document.getElementById('select-model');
            const selectedId = parseFloat(selector?.value);
            const model = models.find(m => m.id === selectedId);
            const costs = getBaseCosts();
            const resultDiv = document.getElementById('result-display');

            if (!model) {
                resultDiv.innerHTML = '<p class="text-center text-gray-500">Selecciona o agrega un modelo para calcular.</p>';
                return;
            }

            const calculation = calculateModelCost(model, costs);
            const format = formatCurrency;
            
            // Texto para mostrar el tipo de LED
            const ledTypeDesc = model.ledType === 'rgb' ? 'Tira LED RGB' : 'Tira LED Blanco';
            const totalLeds = model.consumption.led_quantity * LEDS_PER_SEGMENT;

            resultDiv.innerHTML = `
                <h3 class="text-2xl font-bold mb-6 text-blue-600 border-b pb-2">${model.name}</h3>
                
                <div class="space-y-4 mb-6">
                    <h4 class="text-xl font-semibold text-gray-700">Desglose de Costos</h4>
                    
                    <!-- Materiales Directos -->
                    <div class="p-3 bg-gray-50 rounded-lg">
                        <p class="font-bold mb-2 text-gray-700">1. Materiales Directos (${format(calculation.costMaterials)})</p>
                        <ul class="text-sm text-gray-600 ml-4 list-disc space-y-1">
                            <li>Filamento PLA (${model.consumption.pla_grams}g): ${format(calculation.costPla)}</li>
                            <li>Tira LED (${totalLeds} LEDs de ${ledTypeDesc}): ${format(calculation.costLeds)}</li>
                            <li>Fuente 12V (1 unidad): ${format(costs.power_supply)}</li>
                            <li>Embalaje/Caja (1 unidad): ${format(costs.packaging)}</li>
                        </ul>
                    </div>

                    <!-- Producción Variable -->
                    <div class="p-3 bg-gray-50 rounded-lg">
                        <p class="font-bold mb-2 text-gray-700">2. Producción Variable (${format(calculation.costProduction)})</p>
                        <ul class="text-sm text-gray-600 ml-4 list-disc space-y-1">
                            <li>Mano de Obra (${model.consumption.mo_minutes} min): ${format(calculation.costMOD)}</li>
                            <li>Energía de Impresión (${(model.consumption.print_hours * 60) + model.consumption.print_minutes} min): ${format(calculation.costEnergy)}</li>
                        </ul>
                    </div>

                    <!-- Costos Fijos -->
                    <div class="p-3 bg-gray-50 rounded-lg">
                        <p class="font-bold mb-2 text-gray-700">3. Costos Fijos Asignados (CIF): ${format(calculation.costCIF)}</p>
                        <p class="text-xs text-gray-500 ml-4">Costo fijo mensual prorrateado entre ${document.getElementById('input-cif-production').value} unidades.</p>
                    </div>
                </div>
                
                <div class="border-t border-gray-300 pt-4 space-y-3">
                    <p class="text-2xl font-bold flex justify-between">
                        <span>COSTO UNITARIO DE FABRICACIÓN (CUF):</span>
                        <span class="text-green-600">${format(calculation.CUF)}</span>
                    </p>
                    <p class="text-md flex justify-between">
                        <span>Precio de Venta Sugerido (Con ${costs.margin * 100}% Margen Neto):</span>
                        <span class="font-medium text-lg">${format(calculation.suggestedPrice)}</span>
                    </p>
                    <div class="p-3 bg-red-50 rounded-lg">
                        <p class="text-3xl font-extrabold flex justify-between">
                            <span>PRECIO FINAL RECOMENDADO:</span>
                            <span class="text-red-600">${format(calculation.finalPrice)}</span>
                        </p>
                        <p class="text-xs text-gray-600 text-right mt-1">Este precio incluye ${costs.commissions * 100}% de comisiones/impuestos (ej. Mercado Libre, IVA, etc.).</p>
                    </div>
                </div>
            `;
        }

        // --- MODEL MANIPULATION ---

        function addModel() {
            const newModel = {
                id: Date.now(),
                name: "Nuevo Lightbox",
                ledType: 'white', // Default a blanco
                consumption: {
                    pla_grams: 50,
                    led_quantity: 12, // Ejemplo (12 sectores * 3 LEDs = 36 LEDs)
                    mo_minutes: 10,
                    print_hours: 2,   
                    print_minutes: 0,
                }
            };
            models.push(newModel);
            renderModelInputs();
            // Seleccionar el nuevo modelo para que se muestre inmediatamente
            document.getElementById('select-model').value = newModel.id;
            displayResults();
        }

        function removeModel(id) {
            if (models.length <= 1) {
                customAlert('Debe haber al menos un modelo para poder calcular los costos.', 'Alerta');
                return;
            }
            models = models.filter(m => m.id !== id);
            renderModelInputs();
            displayResults();
        }

        function updateModelConsumption(id, key, value) {
            const model = models.find(m => m.id === id);
            if (model) {
                // Validación básica para asegurar números positivos y limitar minutos
                const numValue = parseFloat(value) || 0;
                
                if (key === 'print_minutes') {
                    // Limitar minutos de impresión a 0-59
                    model.consumption[key] = Math.min(59, Math.max(0, numValue));
                } else {
                    model.consumption[key] = Math.max(0, numValue);
                }
            }
            displayResults();
        }

        function updateModelName(id, name) {
            const model = models.find(m => m.id === id);
            if (model) {
                model.name = name;
            }
            renderModelSelector();
            displayResults();
        }

        /**
         * Actualiza el tipo de LED seleccionado para un modelo.
         * @param {number} id - ID del modelo.
         * @param {string} value - 'white' o 'rgb'.
         */
        function updateModelLedType(id, value) {
            const model = models.find(m => m.id === id);
            if (model) {
                model.ledType = value;
            }
            displayResults();
        }
        
        // --- UI UTILITIES ---

        /** * Llama al cálculo de costos base y al resultado final. 
         * Se usa en los eventos oninput de la pestaña 1 y 3.
         */
        function updateBaseCostsDisplay() {
            calculateBaseUnitCosts();
            displayResults();
        }

        function showSection(targetId) {
            document.querySelectorAll('.tab-content').forEach(div => {
                div.classList.add('hidden');
            });
            document.getElementById(targetId).classList.remove('hidden');

            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('border-blue-500', 'text-blue-600');
                btn.classList.add('border-transparent', 'text-gray-600');
            });

            document.querySelector(`.tab-btn[data-target="${targetId}"]`).classList.add('border-blue-500', 'text-blue-600');
            document.querySelector(`.tab-btn[data-target="${targetId}"]`).classList.remove('border-transparent', 'text-gray-600');
        }

        // --- INICIALIZACIÓN ---
        window.onload = function() {
            // 1. Calcular los costos base iniciales y preparar el selector
            calculateBaseUnitCosts();

            // 2. Renderizar la vista de modelos y resultados
            renderModelInputs();
            
            // 3. Asegurarse de que el modelo principal esté seleccionado y visible
            if (models.length > 0) {
                document.getElementById('select-model').value = models[0].id;
                displayResults();
            }

            // 4. Mostrar la pestaña de resultados por defecto al cargar (porque es la más importante)
            showSection('results');
        };

    </script>
</body>
</html>
