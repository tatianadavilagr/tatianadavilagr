<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SG-SST: Identificación Trabajadores Alto Riesgo - IEB Ingeniería Especializada</title>
    <style>
        /* Estilos CSS Embebidos */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
            color: #333;
        }

        .container {
            max-width: 900px;
            margin: auto;
            background: #fff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
        }

        h1, h2, h3 {
            color: #0056b3; /* Azul corporativo (ejemplo) */
            margin-bottom: 0.8em;
        }
        h1 {
            text-align: center;
            border-bottom: 2px solid #0056b3;
            padding-bottom: 10px;
            margin-bottom: 20px;
        }
        h2 {
            border-left: 4px solid #0056b3;
            padding-left: 10px;
        }

        .form-section {
            margin-bottom: 25px;
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #f9f9f9;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #555;
        }

        select, input[type="text"], input[type="number"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box; /* Para incluir padding en el width */
        }

        .checkbox-group label {
            font-weight: normal;
            display: flex; /* Alinea checkbox y texto */
            align-items: center;
            margin-bottom: 10px;
        }

        .checkbox-group input[type="checkbox"] {
            margin-right: 10px;
            width: auto; /* Ancho automático para checkbox */
            margin-bottom: 0; /* Quita margen inferior */
        }

        button {
            background-color: #0056b3;
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.3s ease;
            margin-right: 10px;
        }

        button:hover {
            background-color: #003d82;
        }

        button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }

        #resultadoEvaluacion {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background-color: #e9ecef;
        }

        #resultadoEvaluacion p {
            margin: 5px 0;
            font-size: 1.1em;
        }
         #resultadoEvaluacion p strong {
            color: #0056b3;
         }

        .status-si {
            color: #dc3545; /* Rojo para indicar riesgo/cotización especial */
            font-weight: bold;
        }

        .status-no {
            color: #28a745; /* Verde */
            font-weight: bold;
        }

        #tablaResumen {
            margin-top: 30px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }

        th, td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: left;
        }

        th {
            background-color: #0056b3;
            color: white;
        }

        tr:nth-child(even) {
            background-color: #f2f2f2;
        }

        /* Clase para ocultar elementos */
        .hidden {
            display: none;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>SG-SST IEB Ingeniería Especializada</h1>
        <h2>Identificación de Trabajadores de Alto Riesgo y Cotización Especial de Pensión</h2>
        <p><strong>Referencia Normativa:</strong> Decreto 1072/2015, Res. 0312/2019, Decreto 2090/2003</p>

        <!-- Sección 1: Selección de Empleado (Simula enlace con módulo de empleados) -->
        <div class="form-section">
            <h3>1. Selección del Trabajador</h3>
            <label for="selectEmpleado">Seleccione Trabajador:</label>
            <select id="selectEmpleado">
                <option value="">-- Seleccione un empleado --</option>
                <!-- Empleados se cargarán aquí por JS -->
            </select>
        </div>

        <!-- Sección 2: Identificación de Actividades de Alto Riesgo (Visible al seleccionar empleado) -->
        <div id="seccionEvaluacion" class="form-section hidden">
            <h3>2. Identificación de Actividades de Alto Riesgo (Decreto 2090 de 2003)</h3>
            <p>Marque las actividades que el trabajador realiza de forma permanente (dedicación exclusiva igual o superior al 70% del tiempo laboral):</p>
            <div id="listaTareasAltoRiesgo" class="checkbox-group">
                <!-- Tareas se cargarán aquí por JS -->
            </div>
            <button id="btnEvaluar" disabled>Evaluar Elegibilidad Pensión Especial</button>
        </div>

        <!-- Sección 3: Resultado de la Evaluación (Visible después de evaluar) -->
        <div id="resultadoEvaluacion" class="hidden">
            <h3>3. Resultado de la Evaluación</h3>
            <p><strong>Empleado:</strong> <span id="resNombreEmpleado"></span></p>
            <p><strong>¿Califica para Pensión Especial (Decreto 2090/2003)?</strong> <span id="resCalificaPension"></span></p>
            <p><strong>Cotización Adicional (10% sobre IBC):</strong> <span id="resCotizacionAdicional"></span></p>
            <p><strong>Responsable de la Cotización Adicional:</strong> <span id="resResponsable"></span></p>
            <button id="btnGuardarRegistro" disabled>Guardar Registro</button>
        </div>

        <!-- Sección 4: Resumen de Trabajadores de Alto Riesgo -->
        <div id="tablaResumen" class="form-section">
            <h3>4. Resumen: Trabajadores Identificados con Alto Riesgo y Cotización Especial</h3>
            <table id="tablaEmpleadosAltoRiesgo">
                <thead>
                    <tr>
                        <th>ID Empleado</th>
                        <th>Nombre</th>
                        <th>Cargo</th>
                        <th>¿Califica Pensión Especial?</th>
                        <th>Cotización Adicional</th>
                    </tr>
                </thead>
                <tbody id="tbodyResumen">
                    <!-- Filas se agregarán aquí por JS -->
                    <tr>
                        <td colspan="5" style="text-align: center;">No hay registros guardados aún.</td>
                    </tr>
                </tbody>
            </table>
        </div>

    </div>

    <script>
        // JavaScript Embebido
        document.addEventListener('DOMContentLoaded', function() {

            // --- DATOS SIMULADOS ---
            const empleados = [
                { id: 'EMP001', nombre: 'Carlos Ramírez', cargo: 'Soldador Estructural' },
                { id: 'EMP002', nombre: 'Lucía Fernández', cargo: 'Ingeniera de Proyectos' },
                { id: 'EMP003', nombre: 'Andrés Soto', cargo: 'Técnico en Alturas' },
                { id: 'EMP004', nombre: 'Mariana Vélez', cargo: 'Supervisora SST' },
                { id: 'EMP005', nombre: 'Javier Muñoz', cargo: 'Operador de Maquinaria Pesada (Minería a cielo abierto)' },
                { id: 'EMP006', nombre: 'Sofia Castro', cargo: 'Auxiliar Administrativa' },
                { id: 'EMP007', nombre: 'Ricardo Mesa', cargo: 'Vigilante (con arma de fuego)'}
            ];

            // Tareas de Alto Riesgo según Decreto 2090 de 2003 (ejemplos relevantes)
            const tareasAltoRiesgo = [
                { id: 't1', desc: 'Trabajos en minería subterránea.' },
                { id: 't2', desc: 'Trabajos expuestos a altas temperaturas (superiores a valores límites permisibles).' },
                { id: 't3', desc: 'Trabajos con exposición a radiaciones ionizantes.' },
                { id: 't4', desc: 'Trabajos con exposición a sustancias comprobadamente cancerígenas.' },
                { id: 't5', desc: 'Actividad de técnicos aeronáuticos con funciones de controladores de tráfico aéreo.' },
                { id: 't6', desc: 'Trabajos en Cuerpos de Bomberos (actividad de extinción de incendios).' },
                { id: 't7', desc: 'Trabajos en el INPEC (custodia y vigilancia).' },
                { id: 't8', desc: 'Trabajos en actividades de alto riesgo en Ecopetrol (definidas en acuerdo).' },
                { id: 't9', desc: 'Trabajos en minería a cielo abierto (Carbones Minerales - Nivel 14 en adelante).' },
                { id: 't10', desc: 'Trabajos de vigilancia y seguridad privada (con arma de fuego, expuestos a riesgo directo)' },
                 { id: 't11', desc: 'Trabajos en alturas (exposición permanente - según interpretación específica, aunque no listado explícito en D.2090 para pensión especial, es alto riesgo Res. 0312)' },
                // Agregar más tareas según sea necesario y la interpretación normativa
            ];

            // Almacenamiento simulado de registros guardados
            let registrosGuardados = {}; // { empleadoId: { datos } }

            // --- REFERENCIAS AL DOM ---
            const selectEmpleado = document.getElementById('selectEmpleado');
            const seccionEvaluacion = document.getElementById('seccionEvaluacion');
            const listaTareasContainer = document.getElementById('listaTareasAltoRiesgo');
            const btnEvaluar = document.getElementById('btnEvaluar');
            const resultadoEvaluacionDiv = document.getElementById('resultadoEvaluacion');
            const resNombreEmpleado = document.getElementById('resNombreEmpleado');
            const resCalificaPension = document.getElementById('resCalificaPension');
            const resCotizacionAdicional = document.getElementById('resCotizacionAdicional');
            const resResponsable = document.getElementById('resResponsable');
            const btnGuardarRegistro = document.getElementById('btnGuardarRegistro');
            const tbodyResumen = document.getElementById('tbodyResumen');

            // --- FUNCIONES ---

            // Carga inicial de empleados en el select
            function cargarEmpleados() {
                empleados.forEach(emp => {
                    const option = document.createElement('option');
                    option.value = emp.id;
                    option.textContent = `${emp.nombre} (${emp.cargo})`;
                    selectEmpleado.appendChild(option);
                });
            }

            // Carga las tareas de alto riesgo como checkboxes
            function cargarTareas() {
                listaTareasContainer.innerHTML = ''; // Limpiar lista previa
                tareasAltoRiesgo.forEach(tarea => {
                    const div = document.createElement('div');
                    const checkbox = document.createElement('input');
                    checkbox.type = 'checkbox';
                    checkbox.id = tarea.id;
                    checkbox.value = tarea.id;
                    checkbox.dataset.desc = tarea.desc; // Guardar descripción
                    checkbox.classList.add('tarea-checkbox'); // Clase para seleccionar fácil

                    const label = document.createElement('label');
                    label.htmlFor = tarea.id;
                    label.textContent = tarea.desc;

                    div.appendChild(checkbox);
                    div.appendChild(label);
                    listaTareasContainer.appendChild(div);
                });
            }

            // Limpia el formulario de evaluación y resultados
             function limpiarFormularioEvaluacion() {
                // Desmarcar checkboxes
                const checkboxes = listaTareasContainer.querySelectorAll('.tarea-checkbox');
                checkboxes.forEach(cb => cb.checked = false);

                // Ocultar y resetear resultados
                resultadoEvaluacionDiv.classList.add('hidden');
                resNombreEmpleado.textContent = '';
                resCalificaPension.textContent = '';
                resCotizacionAdicional.textContent = '';
                resResponsable.textContent = '';
                resCalificaPension.className = ''; // Limpiar clases de estilo (status-si/status-no)

                // Deshabilitar botones
                btnEvaluar.disabled = true;
                btnGuardarRegistro.disabled = true;
            }

            // Evento cuando se cambia el empleado seleccionado
            selectEmpleado.addEventListener('change', function() {
                const empleadoId = this.value;
                limpiarFormularioEvaluacion(); // Limpia estado anterior

                if (empleadoId) {
                    seccionEvaluacion.classList.remove('hidden');
                    cargarTareas(); // Cargar/Recargar tareas (por si acaso)
                    // Habilitar botón Evaluar SÓLO si hay un empleado seleccionado
                    btnEvaluar.disabled = false;

                    // Opcional: Pre-cargar datos si ya existen para este empleado
                    if (registrosGuardados[empleadoId]) {
                        const registro = registrosGuardados[empleadoId];
                        // Marcar checkboxes guardados
                        const checkboxes = listaTareasContainer.querySelectorAll('.tarea-checkbox');
                        checkboxes.forEach(cb => {
                            if (registro.tareasSeleccionadas.includes(cb.value)) {
                                cb.checked = true;
                            }
                        });
                        // Mostrar resultados guardados (sin permitir guardar de nuevo hasta re-evaluar)
                        mostrarResultados(empleadoId, registro.califica, true); // El 'true' indica que es data cargada
                    }

                } else {
                    seccionEvaluacion.classList.add('hidden');
                }
            });

            // Función para evaluar la elegibilidad
            function evaluarElegibilidad() {
                const empleadoId = selectEmpleado.value;
                if (!empleadoId) return; // No debería pasar si el botón está bien gestionado

                const checkboxesSeleccionados = listaTareasContainer.querySelectorAll('.tarea-checkbox:checked');
                let calificaParaPensionEspecial = false;
                let tareasSeleccionadasIds = [];

                 // Tareas específicas del Decreto 2090 que SÍ otorgan pensión especial
                 // Simplificado: Asumimos que las primeras 10 de nuestra lista sí califican directamente
                 // En un caso real, esto requiere un mapeo más preciso o una propiedad booleana en el objeto tarea.
                 const tareasQueCalifican = ['t1', 't2', 't3', 't4', 't5', 't6', 't7', 't9', 't10']; // t8 depende de acuerdo Ecopetrol

                checkboxesSeleccionados.forEach(cb => {
                    tareasSeleccionadasIds.push(cb.value);
                    if (tareasQueCalifican.includes(cb.value)) {
                        calificaParaPensionEspecial = true;
                    }
                });

                 // Guardar temporalmente las tareas seleccionadas para el guardado final
                 // Usamos un atributo de datos en un elemento contenedor para pasar la info
                 resultadoEvaluacionDiv.dataset.tareasSeleccionadas = JSON.stringify(tareasSeleccionadasIds);

                mostrarResultados(empleadoId, calificaParaPensionEspecial);
                btnGuardarRegistro.disabled = false; // Habilitar guardado después de evaluar
            }

             // Función para mostrar los resultados de la evaluación
            function mostrarResultados(empleadoId, califica, esDataCargada = false) {
                const empleado = empleados.find(emp => emp.id === empleadoId);
                resNombreEmpleado.textContent = empleado ? `${empleado.nombre} (${empleado.cargo})` : 'Desconocido';

                if (califica) {
                    resCalificaPension.textContent = 'SÍ';
                    resCalificaPension.className = 'status-si';
                    resCotizacionAdicional.textContent = 'SÍ (+10% adicional sobre IBC)';
                    resResponsable.textContent = 'A cargo exclusivo del EMPLEADOR (IEB Ingeniería Especializada)';
                } else {
                    resCalificaPension.textContent = 'NO';
                    resCalificaPension.className = 'status-no';
                    resCotizacionAdicional.textContent = 'NO Aplica';
                    resResponsable.textContent = 'N/A';
                }
                resultadoEvaluacionDiv.classList.remove('hidden');

                // Si es data cargada, deshabilitar guardar para forzar re-evaluación si se quiere cambiar
                 btnGuardarRegistro.disabled = esDataCargada;
            }


            // Evento para el botón Evaluar
            btnEvaluar.addEventListener('click', evaluarElegibilidad);

            // Función para guardar el registro (simulado)
            function guardarRegistro() {
                const empleadoId = selectEmpleado.value;
                if (!empleadoId) return;

                const empleado = empleados.find(emp => emp.id === empleadoId);
                const calificaTexto = resCalificaPension.textContent;
                const cotizaTexto = resCotizacionAdicional.textContent;
                 const tareasSeleccionadas = JSON.parse(resultadoEvaluacionDiv.dataset.tareasSeleccionadas || '[]');

                // Guardar en el objeto simulado
                registrosGuardados[empleadoId] = {
                    id: empleado.id,
                    nombre: empleado.nombre,
                    cargo: empleado.cargo,
                    califica: calificaTexto === 'SÍ',
                    cotizacion: cotizaTexto,
                    tareasSeleccionadas: tareasSeleccionadas // Guardamos las tareas marcadas
                };

                actualizarTablaResumen();
                alert(`Registro guardado para ${empleado.nombre}`);
                btnGuardarRegistro.disabled = true; // Deshabilitar después de guardar
            }

            // Evento para el botón Guardar
            btnGuardarRegistro.addEventListener('click', guardarRegistro);

             // Función para actualizar la tabla resumen
            function actualizarTablaResumen() {
                tbodyResumen.innerHTML = ''; // Limpiar tabla

                const idsGuardados = Object.keys(registrosGuardados);

                if (idsGuardados.length === 0) {
                    tbodyResumen.innerHTML = '<tr><td colspan="5" style="text-align: center;">No hay registros guardados aún.</td></tr>';
                    return;
                }

                idsGuardados.forEach(id => {
                    const registro = registrosGuardados[id];
                    const fila = document.createElement('tr');

                    fila.innerHTML = `
                        <td>${registro.id}</td>
                        <td>${registro.nombre}</td>
                        <td>${registro.cargo}</td>
                        <td class="${registro.califica ? 'status-si' : 'status-no'}">${registro.califica ? 'SÍ' : 'NO'}</td>
                        <td>${registro.cotizacion}</td>
                    `;
                    tbodyResumen.appendChild(fila);
                });
            }

            // --- INICIALIZACIÓN ---
            cargarEmpleados();
            actualizarTablaResumen(); // Mostrar tabla vacía o con datos pre-cargados si los hubiera

        });
    </script>

</body>
</html>  
