<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Control de Carga – Sistema Profesional</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.3.0/dist/chart.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"></script>
  <style>
    :root{
      --bg:#0a0e1a; --panel:#0f1621; --card:#141b2e; --muted:#94a3b8;
      --gold:#fbbf24; --white:#f8fafc; --accent:#3b82f6; --sidebar-w:260px;
      --success:#10b981; --danger:#ef4444;
    }
    *{box-sizing:border-box;margin:0;padding:0}
    html,body{height:100%;font-family:-apple-system,BlinkMacSystemFont,'Inter',Segoe UI,Arial,sans-serif;background:linear-gradient(135deg,var(--bg) 0%,#020408 100%);color:var(--white);overflow:hidden}
    
    /* Scrollbar futurista */
    ::-webkit-scrollbar{width:8px}
    ::-webkit-scrollbar-track{background:#111a2c}
    ::-webkit-scrollbar-thumb{background:var(--accent);border-radius:4px}
    ::-webkit-scrollbar-thumb:hover{background:#4a90f2}

    .app{display:flex;height:100vh;position:relative}

    /* --- SIDEBAR (Menú Meses) --- */
    .sidebar{
      width:var(--sidebar-w);background:var(--panel);padding:20px 0;
      box-shadow:2px 0 10px rgba(0,0,0,0.5);z-index:100;flex-shrink:0;
      transition:transform 0.3s ease;
    }
    .sidebar.hidden{transform:translateX(calc(-1 * var(--sidebar-w)));}
    .sidebar h2{
      text-align:center;color:var(--gold);margin-bottom:20px;
      font-size:1.4rem;border-bottom:1px solid rgba(251,191,36,0.1);padding-bottom:10px;
    }
    .month-list{
      list-style:none;padding:0;overflow-y:auto;height:calc(100% - 70px);
    }
    .month-list li{
      padding:12px 20px;cursor:pointer;font-size:1rem;
      transition:background 0.2s, color 0.2s;border-left:4px solid transparent;
    }
    .month-list li:hover{background:rgba(255,255,255,0.05);}
    .month-list li.active{
      background:rgba(59,130,246,0.15);color:var(--accent);
      font-weight:600;border-left-color:var(--accent);
    }

    /* --- MAIN CONTENT --- */
    .content{
      flex-grow:1;padding:20px;overflow-y:auto;
      padding-left: calc(var(--sidebar-w) + 20px); /* Ajuste inicial */
      transition:padding-left 0.3s ease;
    }
    .content.full-width{padding-left:20px;}

    .header-main{
        display:flex;justify-content:space-between;align-items:center;
        margin-bottom:20px;padding-bottom:10px;border-bottom:1px solid rgba(255,255,255,0.1);
    }
    .header-main h1{color:var(--accent);font-size:1.8rem;display:flex;align-items:center;gap:10px;}
    .stats-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:20px;margin-bottom:20px;}

    /* --- FORMULARIOS --- */
    form{
      background:var(--panel);padding:20px;border-radius:12px;
      box-shadow:0 4px 10px rgba(0,0,0,0.3);margin-bottom:20px;
      border:1px solid rgba(255,255,255,0.05);
    }
    .form-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:15px;}
    
    label{display:block;margin-bottom:5px;color:var(--muted);font-size:0.85rem;}
    input[type="text"], input[type="number"], input[type="date"], select, .search-input{
      width:100%;padding:10px;border-radius:6px;border:1px solid rgba(255,255,255,0.1);
      background:var(--card);color:var(--white);font-size:1rem;
      transition:border-color 0.2s, box-shadow 0.2s;
    }
    input:focus, select:focus{
      outline:none;border-color:var(--accent);
      box-shadow:0 0 5px rgba(59,130,246,0.5);
    }
    .form-actions{grid-column:span 4;display:flex;justify-content:flex-end;gap:10px;margin-top:10px;}
    
    /* --- TABLAS --- */
    table{width:100%;border-collapse:collapse;}
    th,td{padding:12px 10px;text-align:center;border-bottom:1px solid rgba(255,255,255,0.05);font-size:0.9rem;}
    th{background:rgba(255,255,255,0.05);color:var(--gold);font-weight:600;text-transform:uppercase;}
    tr:hover{background:rgba(255,255,255,0.02);}
    td.left{text-align:left;}

    /* --- GRÁFICOS --- */
    .chart-container{
        background:var(--panel);padding:20px;border-radius:12px;
        box-shadow:0 4px 10px rgba(0,0,0,0.3);
        height:350px;
        border:1px solid rgba(255,255,255,0.05);
    }

    /* --- RESUMEN DE DATOS (KPI) --- */
    .summary-grid, .summary-row{
      display:grid;grid-template-columns:repeat(3,1fr);gap:20px;margin-bottom:20px;
    }
    .card-s{
      background:var(--card);padding:15px;border-radius:10px;
      border-left:4px solid var(--gold);box-shadow:0 2px 5px rgba(0,0,0,0.2);
      transition:transform 0.2s;border:1px solid rgba(255,255,255,0.05);
    }
    .card-s .label{color:var(--muted);font-size:0.8rem;text-transform:uppercase;}
    .card-s .value{font-size:1.6rem;font-weight:700;color:var(--white);margin-top:5px;}

    /* --- BOTONES GENERALES --- */
    .btn{
      padding:10px 18px;border:none;border-radius:8px;
      cursor:pointer;font-weight:600;transition:all 0.2s;font-size:1rem;
    }
    .primary{background:var(--accent);color:var(--white);}
    .primary:hover{background:#4a90f2;transform:translateY(-1px);}
    .danger{background:var(--danger);color:var(--white);}
    .danger:hover{background:#dc2626;}
    .secondary{background:rgba(255,255,255,0.1);color:var(--white);border:1px solid rgba(255,255,255,0.2);}
    .secondary:hover{background:rgba(255,255,255,0.2);}
    .ghost{
      background:transparent;color:var(--white);border:1px solid rgba(255,255,255,0.2);
      padding:8px 12px;
    }
    .ghost:hover{background:rgba(255,255,255,0.1);}
    
    /* --- Utilerías --- */
    .hidden{display:none!important;}
    .actions button{
        font-size:0.8rem;margin:2px;padding:6px 8px;border-radius:4px;
        border:1px solid rgba(255,255,255,0.1);cursor:pointer;
        background:rgba(59,130,246,0.1);color:var(--accent);
    }
    .actions button:hover{background:rgba(59,130,246,0.2);}
    .actions button.danger{background:rgba(239,68,68,0.1);color:var(--danger);}
    .actions button.danger:hover{background:rgba(239,68,68,0.2);}

    /* --- MOBILE & RESPONSIVE --- */
    @media (max-width: 1200px) {
        .form-grid{grid-template-columns:repeat(2,1fr);}
        .form-actions{grid-column:span 2;}
        .stats-grid, .summary-grid{grid-template-columns:repeat(2,1fr);}
    }

    @media (max-width: 768px) {
        .sidebar{position:fixed;height:100vh;z-index:1000;top:0;left:0;}
        .sidebar.hidden{transform:translateX(-100%);}
        .content{padding:20px;}
        .content.full-width{padding-left:20px;}
        .form-grid{grid-template-columns:1fr;}
        .form-actions{grid-column:span 1;}
        .stats-grid, .summary-grid{grid-template-columns:1fr;}
        .toggle-btn{display:block;}
    }

    /* --- TOAST / NOTIFICACIONES --- */
    .toast-container{position:fixed;bottom:20px;right:20px;z-index:5000;display:flex;flex-direction:column;gap:10px;}
    .toast{
      background:var(--panel);color:var(--white);padding:15px 20px;border-radius:8px;
      box-shadow:0 4px 15px rgba(0,0,0,0.4);border-left:4px solid var(--accent);
      min-width:250px;animation:slideIn 0.3s ease-out;
    }
    .toast.success{border-left-color:var(--success);}
    .toast.error{border-left-color:var(--danger);}
    @keyframes slideIn{from{opacity:0;transform:translateX(100%)}to{opacity:1;transform:translateX(0)}}
    
    /* --- MODAL CALCULADORA --- */
    .modal-calc{
      position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.6);
      display:none;align-items:center;justify-content:center;z-index:4000;
    }
    .modal-calc.visible{display:flex;}
    .calculator{
      background:var(--card);padding:20px;border-radius:12px;box-shadow:0 8px 25px rgba(0,0,0,0.5);
      width:300px;
    }
    .calc-display{
      background:var(--bg);color:var(--white);font-size:2.5rem;text-align:right;
      padding:15px;border-radius:8px;margin-bottom:15px;height:70px;
      overflow-x:auto;white-space:nowrap;
    }
    .calc-buttons{
      display:grid;grid-template-columns:repeat(4,1fr);gap:10px;
    }
    .calc-buttons button{
      background:#2d3748;color:var(--white);border:none;padding:15px;
      border-radius:8px;font-size:1.2rem;cursor:pointer;transition:background 0.2s;
    }
    .calc-buttons button:hover{background:#4a5568;}
    .calc-buttons button.operator{background:var(--accent);color:var(--white);}
    .calc-buttons button.operator:hover{background:#4a90f2;}
    .calc-buttons button.equal{background:var(--gold);color:var(--bg);grid-column:span 2;}
    .calc-buttons button.equal:hover{background:#eac45f;}

    /* NUEVO: Estilo para botones de periodo */
    .period-btn.active {
        background: linear-gradient(135deg, var(--accent), #4a90f2); 
        border-color: var(--accent);
        color: var(--white);
        font-weight: 600;
        box-shadow: 0 4px 12px rgba(59,130,246,0.3);
    }
    .period-btn.active:hover {
        transform: translateY(0);
        background: linear-gradient(135deg, var(--accent), #4a90f2); 
    }
  </style>
</head>

<body>
  <div class="app">

    <div id="sidebar" class="sidebar">
      <h2>📅 Meses de Registro</h2>
      <ul id="sidebarMonths" class="month-list">
        </ul>
    </div>

    <div id="content" class="content full-width">
      <div class="header-main">
        <h1 id="monthTitle"></h1>
        <div style="display:flex;gap:10px;">
            <button class="primary btn" onclick="window.showCalculator()" id="btnShowCalculator">🧮 Calculadora</button>
            <button class="secondary btn" onclick="exportToExcel()">⬇️ Exportar Mes (Excel)</button>
            <button class="danger btn" onclick="deleteMonthData()">🚫 Borrar Mes</button>
            <button class="secondary btn toggle-btn hidden" onclick="toggleSidebar()">☰</button>
        </div>
      </div>
      
      <div style="margin-bottom: 20px; padding: 10px 15px; background: var(--card); border-radius: 8px; border: 1px solid rgba(255,255,255,0.1); display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 10px;">
          <h4 style="color: var(--gold); margin: 0; font-size: 1rem;">Gestión de Datos Portátiles</h4>
          <div style="display: flex; gap: 10px; flex-wrap: wrap;">
              <button class="primary btn" onclick="window.downloadDataFile()">⬇️ Guardar Archivo de Datos</button>
              <input type="file" id="uploadDataFile" accept=".json" style="display: none;" onchange="window.uploadDataFile(event)">
              <button class="secondary btn" onclick="document.getElementById('uploadDataFile').click()">⬆️ Cargar Archivo de Datos</button>
          </div>
      </div>


      <form id="dataForm">
        <h3>➕ Ingreso de Carga - <span id="modeTitle">NUEVO REGISTRO</span></h3>
        <input type="hidden" id="editIndex" value="-1">
        <div class="form-grid">
          <div>
            <label for="fecha">Fecha (*)</label>
            <input type="date" id="fecha" required>
          </div>
          <div>
            <label for="material">Material (*)</label>
            <input type="text" id="material" list="materiales" required placeholder="Ej: Material Fino">
            <datalist id="materiales"></datalist>
          </div>
          <div>
            <label for="volquetadas">Volquetadas (*)</label>
            <input type="number" id="volquetadas" min="1" required placeholder="Cantidad de Volquetadas">
          </div>
          <div>
            <label for="peso">Peso (TON) (*)</label>
            <input type="number" id="peso" step="0.01" min="0.01" required placeholder="Peso en Toneladas">
          </div>
          <div>
            <label for="destino">Destino / Cliente</label>
            <input type="text" id="destino" list="destinos" placeholder="Ej: Obra A">
            <datalist id="destinos"></datalist>
          </div>
          <div style="grid-column: span 3;">
            <label for="observaciones">Observaciones</label>
            <input type="text" id="observaciones" placeholder="Detalles o Incidencias">
          </div>
        </div>
        <div class="form-actions">
          <button type="button" class="secondary btn hidden" id="btnCancelEdit" onclick="cancelEdit()">Cancelar Edición</button>
          <button type="submit" class="primary btn" id="btnSubmit">💾 Guardar Registro</button>
        </div>
      </form>

      <div id="periodSelection" style="margin-top: 20px; padding: 16px; background: rgba(59,130,246,0.05); border-radius: 10px; border: 1px solid rgba(59,130,246,0.1);">
        <h4 style="color: var(--gold); margin-bottom: 15px; font-size: 16px;">🔍 Vista Detallada por Periodo</h4>
        <div style="display: flex; gap: 10px; flex-wrap: wrap;">
          <button class="ghost period-btn active" data-period="0">📊 Mes Completo (Todo)</button>
          <button class="ghost period-btn" data-period="1">P1 (Días 1 - 10)</button>
          <button class="ghost period-btn" data-period="2">P2 (Días 11 - 20)</button>
          <button class="ghost period-btn" data-period="3">P3 (Días 21 - 31)</button>
        </div>
        
        <div id="periodSummaryRow" class="summary-row" style="margin-top: 15px;">
          </div>
      </div>

      <div style="overflow:auto">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;margin-top:20px;">
            <h2>Historial de Registros</h2>
            <input type="text" id="search" class="search-input" placeholder="Buscar por Material o Destino..." style="width:300px;">
        </div>
        <table id="table">
          <thead>
            <tr>
              <th>Fecha</th><th>Material</th><th>Volquetadas</th><th>Peso (TON)</th><th>Destino</th><th>Acciones</th>
            </tr>
          </thead>
          <tbody id="tbody">
            </tbody>
        </table>
      </div>

      <h2 style="margin-top: 20px; color: var(--success);">Resumen de Cálculos del Mes</h2>
      <div id="summaryRow" class="summary-grid">
        </div>
      
      <div class="chart-container" style="margin-top:20px;">
          <canvas id="loadChart"></canvas>
      </div>
      
      <footer style="text-align:center;padding:20px 0;font-size:0.8rem;color:var(--muted);margin-top:20px;border-top:1px solid rgba(255,255,255,0.05);">
        Sistema de Control de Carga | Desarrollado por Julian Fonnegra
      </footer>
    </div>
  </div>
  
  <div class="toast-container" id="toastContainer"></div>

  <div id="calculatorModal" class="modal-calc">
    <div class="calculator">
      <div id="calcDisplay" class="calc-display">0</div>
      <div class="calc-buttons">
        <button onclick="window.clearCalc()">C</button>
        <button onclick="window.appendValue('backspace')">←</button>
        <button onclick="window.appendValue('.')">.</button>
        <button onclick="window.setOperator('/')" class="operator">÷</button>
        
        <button onclick="window.appendValue('7')">7</button>
        <button onclick="window.appendValue('8')">8</button>
        <button onclick="window.appendValue('9')">9</button>
        <button onclick="window.setOperator('*')" class="operator">×</button>
        
        <button onclick="window.appendValue('4')">4</button>
        <button onclick="window.appendValue('5')">5</button>
        <button onclick="window.appendValue('6')">6</button>
        <button onclick="window.setOperator('-')" class="operator">−</button>
        
        <button onclick="window.appendValue('1')">1</button>
        <button onclick="window.appendValue('2')">2</button>
        <button onclick="window.appendValue('3')">3</button>
        <button onclick="window.setOperator('+')" class="operator">+</button>
        
        <button onclick="window.hideCalculator()" class="secondary">Cerrar</button>
        <button onclick="window.appendValue('0')">0</button>
        <button onclick="window.calculateResult()" class="equal">=</button>
      </div>
    </div>
  </div>

<script>
  // ====================================================================
  // === VARIABLES GLOBALES Y UTILIDADES ===
  // ====================================================================
  // La clave de localStorage se mantiene solo para la migración inicial.
  const LEGACY_STORAGE_KEY = 'dailyLoadData';
  const MONTHS = ["Enero","Febrero","Marzo","Abril","Mayo","Junio","Julio","Agosto","Septiembre","Octubre","Noviembre","Diciembre"];
  
  // Estructura de datos: Array de 12 meses, cada uno con un array de registros
  let data = Array(12).fill(0).map(() => []); 

  let activeMonth = new Date().getMonth(); 
  let activePeriod = 0; // 0=Mes completo, 1, 2, 3 = Periodos
  const sidebar = document.getElementById('sidebar'); 
  const sidebarMonths = document.getElementById('sidebarMonths');
  const tbody = document.getElementById('tbody');
  const chartCanvas = document.getElementById('loadChart');
  let loadChartInstance;

  // --- FUNCIONES BÁSICAS ---
  // IMPORTANTE: Eliminamos saveData() con localStorage. Ahora se usa downloadDataFile.
  function formatDate(isoDate) { return isoDate ? new Date(isoDate).toLocaleDateString('es-CO', {day:'2-digit', month:'short', year:'numeric'}) : '-'; }
  function showToast(message, type='success') {
    const container = document.getElementById('toastContainer');
    const toast = document.createElement('div');
    toast.className = `toast ${type}`;
    toast.textContent = message;
    container.appendChild(toast);
    setTimeout(() => {
      toast.remove();
    }, 4000);
  }
  function toggleSidebar() {
    sidebar.classList.toggle('hidden');
    document.getElementById('content').classList.toggle('full-width');
  }

  // ====================================================================
  // === NUEVA LÓGICA DE ALMACENAMIENTO (PORTABILIDAD) ===
  // ====================================================================

  /**
   * Intenta migrar los datos desde localStorage si existen.
   */
  function initializeData() {
    const localData = localStorage.getItem(LEGACY_STORAGE_KEY);
    if (localData) {
        try {
            const parsedData = JSON.parse(localData);
            if (Array.isArray(parsedData)) {
                data = parsedData;
                showToast("✅ Datos migrados correctamente desde el navegador. Por favor, guarde su primer archivo JSON.", 'success');
                // Opcional: Limpiar el localStorage para evitar confusiones futuras
                localStorage.removeItem(LEGACY_STORAGE_KEY);
            }
        } catch (e) {
            console.error("Error al cargar datos antiguos:", e);
        }
    }
    // Si no hay datos en localStorage o la carga falla, `data` se mantiene como un array vacío/inicializado.
  }
  
  /**
   * Fuerza la descarga del archivo JSON con los datos actuales.
   */
  window.downloadDataFile = function() {
      const dataToSave = JSON.stringify(data, null, 2);
      const blob = new Blob([dataToSave], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'ControlCarga.json';
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
      showToast("💾 Archivo de datos ControlCarga.json descargado.", 'success');
  };

  /**
   * Carga los datos desde un archivo JSON seleccionado por el usuario.
   */
  window.uploadDataFile = function(event) {
      const file = event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(e) {
          try {
              const loadedData = JSON.parse(e.target.result);
              // Validar que sea un array de 12 meses
              if (Array.isArray(loadedData) && loadedData.length === 12 && loadedData.every(Array.isArray)) {
                  data = loadedData;
                  // Reiniciar la vista
                  initializeActiveMonth();
                  showMonthPage(activeMonth);
                  showToast("⬆️ Datos cargados correctamente. ¡Listo para trabajar!", 'success');
              } else {
                  showToast("❌ Error: El archivo JSON no tiene el formato correcto (Array de 12 meses).", 'error');
              }
          } catch (error) {
              showToast("❌ Error al procesar el archivo. Asegúrese de que sea un JSON válido.", 'error');
          }
      };
      reader.readAsText(file);
      // Limpiar el input para permitir la recarga del mismo archivo
      event.target.value = null;
  };


  // ====================================================================
  // === LÓGICA DE FILTRADO Y RESUMEN ===
  // ====================================================================

  function filterByPeriod(rows, period) {
    if (period === 0) return rows; // Mes completo
    
    // Define los días de inicio y fin para los periodos P1, P2, P3
    const startDay = period === 1 ? 1 : (period === 2 ? 11 : 21);
    const endDay = period === 1 ? 10 : (period === 2 ? 20 : 31);
    
    return rows.filter(r => {
        const day = r.fecha ? new Date(r.fecha).getDate() : null;
        return day >= startDay && day <= endDay;
    });
  }

  function calculateTotals(rows) {
      const totalPeso = rows.reduce((s, r) => s + (parseFloat(r.peso) || 0), 0);
      const totalVolquetadas = rows.reduce((s, r) => s + (parseInt(r.volquetadas) || 0), 0);
      const avg = totalVolquetadas > 0 ? (totalPeso / totalVolquetadas) : 0;
      return { totalPeso, totalVolquetadas, avg };
  }

  function renderSummary(totals, title, elementId, styleType) {
      const element = document.getElementById(elementId);
      if (!element) return;

      let pesoStyle = '';
      let volqStyle = '';
      
      // Aplicar estilo de color Accent (Azul) para el resumen de periodo
      if (styleType === 'period') {
          pesoStyle = 'style="border-left-color:var(--accent);"';
          volqStyle = 'style="border-left-color:var(--accent);"';
      }

      let html = `
          <div class="card-s" ${pesoStyle}>
              <div class="label">${title} - PESO TOTAL (TON)</div>
              <div class="value">${totals.totalPeso.toFixed(2)}</div>
          </div>
          <div class="card-s" ${volqStyle}>
              <div class="label">${title} - VOLQUETADAS</div>
              <div class="value">${totals.totalVolquetadas}</div>
          </div>
      `;

      // Solo la sección de resumen principal (debajo de la tabla) mostrará el promedio
      if (elementId === 'summaryRow') {
          html += `
              <div class="card-s" style="border-left-color:var(--success);">
                  <div class="label">Carga Promedio</div>
                  <div class="value">${totals.avg.toFixed(2)} T/V</div>
              </div>
          `;
      }

      element.innerHTML = html;
  }

  function renderPeriodAndMonthSummary(filteredRows) {
      // 1. Resumen de Periodo (Arriba de la tabla, usa los datos filtrados)
      const periodTotals = calculateTotals(filteredRows);
      const periodTitle = activePeriod === 0 ? `Mes Completo` : `Periodo P${activePeriod}`;
      renderSummary(periodTotals, periodTitle, 'periodSummaryRow', 'period');
      
      // 2. Resumen General del Mes (Debajo de la tabla, siempre usa el mes completo para coherencia)
      const fullMonthRows = data[activeMonth]; 
      const monthTotals = calculateTotals(fullMonthRows);
      renderSummary(monthTotals, `Total Mes (${MONTHS[activeMonth]})`, 'summaryRow', 'month');
  }


  // ====================================================================
  // === RENDERIZADO PRINCIPAL ===
  // ====================================================================
  
  // Función para determinar el mes activo
  function initializeActiveMonth() {
      let hasData = data.some(month => month.length > 0);
      if (!hasData) {
          activeMonth = new Date().getMonth();
      } else {
          let latestMonth = -1;
          let latestDate = new Date(0);
          data.forEach((monthData, index) => {
              if (monthData.length > 0) {
                  const lastRecordDate = new Date(monthData[monthData.length - 1].fecha);
                  if (lastRecordDate > latestDate) {
                      latestDate = lastRecordDate;
                      latestMonth = index;
                  }
              }
          });
          if (latestMonth !== -1) {
              activeMonth = latestMonth;
          } else {
              activeMonth = new Date().getMonth();
          }
      }
  }


  function renderSidebar() {
    sidebarMonths.innerHTML = '';
    const currentYear = new Date().getFullYear();
    
    MONTHS.forEach((monthName, index) => {
      const li = document.createElement('li');
      // Mostrar el año en el nombre del mes
      const displayYear = index === 0 ? currentYear - 1 : currentYear; // Se puede ajustar el año si es un sistema que abarca dos años
      li.textContent = `${monthName} (${data[index].length} Reg.)`; 
      li.dataset.monthIndex = index;
      if (index === activeMonth) {
        li.classList.add('active');
      }
      li.onclick = () => {
        showMonthPage(index);
      };
      sidebarMonths.appendChild(li);
    });
  }

  function renderTable(searchTerm=''){
      const monthData = data[activeMonth];
      let rows = monthData.slice().sort((a,b)=> new Date(b.fecha)-new Date(a.fecha));

      // 1. FILTRAR POR PERIODO 
      rows = filterByPeriod(rows, activePeriod); 

      // 2. FILTRAR POR BÚSQUEDA
      const search = searchTerm.toLowerCase();
      const rowsToDisplay = rows.filter(r=>{
          if(search==='') return true;
          return (r.fecha&&r.fecha.toLowerCase().includes(search)) ||
                (r.material&&r.material.toLowerCase().includes(search)) ||
                (r.destino&&r.destino.toLowerCase().includes(search));
      });

      // 3. RENDERIZAR TABLA (Asegurando el índice original para edit/delete)
      tbody.innerHTML = rowsToDisplay.map((r)=>{ 
          // Encontrar el índice original en data[activeMonth] para las acciones
          const originalIndex = monthData.indexOf(r); 

          // Calcular promedio para mostrar en tabla (solo estético)
          const avg = (r.volquetadas > 0) ? (r.peso / r.volquetadas).toFixed(2) : '0.00';
          
          return `<tr>
                  <td>${formatDate(r.fecha)}</td>
                  <td class="left">${r.material}</td>
                  <td>${r.volquetadas||'-'}</td>
                  <td>${(r.peso||0).toFixed(2)}</td>
                  <td class="left">${r.destino||'-'}</td>
                  <td class="actions">
                      <button onclick="window.editRow(${originalIndex})">✏️ Editar</button>
                      <button class="danger" onclick="window.deleteRow(${originalIndex})">🗑️</button>
                  </td>
              </tr>`;
      }).join('');
      
      // 4. RENDERIZAR RESÚMENES Y GRÁFICO 
      renderPeriodAndMonthSummary(rowsToDisplay); 
      renderChart(rowsToDisplay); 
  }

  function renderChart(rows) {
      if (loadChartInstance) {
          loadChartInstance.destroy();
      }

      // Agrupar por fecha
      const dailyData = rows.reduce((acc, r) => {
          const date = r.fecha;
          acc[date] = acc[date] || { peso: 0, volquetadas: 0 };
          acc[date].peso += parseFloat(r.peso) || 0;
          acc[date].volquetadas += parseInt(r.volquetadas) || 0;
          return acc;
      }, {});

      const sortedDates = Object.keys(dailyData).sort();
      const labels = sortedDates.map(formatDate);
      const pesoData = sortedDates.map(date => dailyData[date].peso.toFixed(2));
      const volquetadasData = sortedDates.map(date => dailyData[date].volquetadas);

      const dataConfig = {
          labels: labels,
          datasets: [
              {
                  label: 'Peso Total (TON)',
                  data: pesoData,
                  borderColor: 'var(--accent)',
                  backgroundColor: 'rgba(59, 130, 246, 0.2)',
                  tension: 0.3,
                  yAxisID: 'y'
              },
              {
                  label: 'Volquetadas',
                  data: volquetadasData,
                  borderColor: 'var(--gold)',
                  backgroundColor: 'rgba(251, 191, 36, 0.2)',
                  type: 'bar',
                  yAxisID: 'y1'
              }
          ]
      };

      const config = {
          type: 'line',
          data: dataConfig,
          options: {
              responsive: true,
              maintainAspectRatio: false,
              plugins: {
                  legend: { labels: { color: 'var(--white)' } }
              },
              scales: {
                  x: { 
                      ticks: { color: 'var(--muted)' },
                      grid: { color: 'rgba(255, 255, 255, 0.1)' }
                  },
                  y: {
                      type: 'linear',
                      display: true,
                      position: 'left',
                      title: { display: true, text: 'Peso (TON)', color: 'var(--accent)' },
                      ticks: { color: 'var(--accent)' },
                      grid: { color: 'rgba(255, 255, 255, 0.1)' }
                  },
                  y1: {
                      type: 'linear',
                      display: true,
                      position: 'right',
                      title: { display: true, text: 'Volquetadas', color: 'var(--gold)' },
                      ticks: { color: 'var(--gold)' },
                      grid: { drawOnChartArea: false }
                  }
              }
          }
      };

      loadChartInstance = new Chart(chartCanvas, config);
  }

  function updateDatalists() {
    const monthData = data.flat();
    const materiales = [...new Set(monthData.map(r => r.material).filter(Boolean))];
    const destinos = [...new Set(monthData.map(r => r.destino).filter(Boolean))];

    document.getElementById('materiales').innerHTML = materiales.map(m => `<option value="${m}">`).join('');
    document.getElementById('destinos').innerHTML = destinos.map(d => `<option value="${d}">`).join('');
  }

  function showMonthPage(monthIndex) {
    activeMonth = monthIndex;
    
    // 1. Resetear el filtro de periodo
    activePeriod = 0; 
    const periodButtons = document.querySelectorAll('.period-btn');
    periodButtons.forEach(b => b.classList.remove('active'));
    const allBtn = document.querySelector('.period-btn[data-period="0"]');
    if(allBtn) allBtn.classList.add('active');

    // 2. Actualizar Título
    document.getElementById('monthTitle').textContent = `Vista Mensual - ${MONTHS[activeMonth]}`;

    // 3. Renderizar todos los elementos dependientes
    renderSidebar();
    renderTable();
    updateDatalists();
  }


  // ====================================================================
  // === CRUD Y EXPORTACIÓN ===
  // ====================================================================

  document.getElementById('dataForm').addEventListener('submit', function(e) {
    e.preventDefault();
    const editIndex = parseInt(document.getElementById('editIndex').value);

    const newRecord = {
      fecha: document.getElementById('fecha').value,
      material: document.getElementById('material').value,
      volquetadas: parseInt(document.getElementById('volquetadas').value),
      peso: parseFloat(document.getElementById('peso').value),
      destino: document.getElementById('destino').value,
      observaciones: document.getElementById('observaciones').value,
      timestamp: new Date().toISOString()
    };

    if (editIndex !== -1) {
      // Editar
      data[activeMonth][editIndex] = newRecord;
      showToast('✅ Registro actualizado con éxito. ¡No olvide guardar el archivo de datos!');
    } else {
      // Nuevo registro
      data[activeMonth].push(newRecord);
      showToast('✅ Nuevo registro guardado con éxito. ¡No olvide guardar el archivo de datos!');
    }

    // Ya no guardamos en localStorage, el usuario debe descargar el archivo
    showMonthPage(activeMonth); // Re-renderizar todo
    this.reset();
    document.getElementById('editIndex').value = '-1';
    document.getElementById('modeTitle').textContent = 'NUEVO REGISTRO';
    document.getElementById('btnCancelEdit').classList.add('hidden');
    document.getElementById('btnSubmit').textContent = '💾 Guardar Registro';
  });

  window.editRow = function(index) {
    const record = data[activeMonth][index];
    if (!record) return;

    document.getElementById('fecha').value = record.fecha;
    document.getElementById('material').value = record.material;
    document.getElementById('volquetadas').value = record.volquetadas;
    document.getElementById('peso').value = record.peso;
    document.getElementById('destino').value = record.destino;
    document.getElementById('observaciones').value = record.observaciones;
    document.getElementById('editIndex').value = index;

    document.getElementById('modeTitle').textContent = 'EDITANDO REGISTRO';
    document.getElementById('btnCancelEdit').classList.remove('hidden');
    document.getElementById('btnSubmit').textContent = '✏️ Actualizar Registro';
    window.scrollTo({ top: 0, behavior: 'smooth' }); // Subir al formulario
  }

  window.cancelEdit = function() {
    document.getElementById('dataForm').reset();
    document.getElementById('editIndex').value = '-1';
    document.getElementById('modeTitle').textContent = 'NUEVO REGISTRO';
    document.getElementById('btnCancelEdit').classList.add('hidden');
    document.getElementById('btnSubmit').textContent = '💾 Guardar Registro';
  }

  window.deleteRow = function(index) {
    if (confirm('¿Está seguro de ELIMINAR este registro? Esta acción es irreversible.')) {
      data[activeMonth].splice(index, 1);
      // Ya no guardamos en localStorage
      showToast('🗑️ Registro eliminado. ¡No olvide guardar el archivo de datos!');
      showMonthPage(activeMonth);
    }
  }

  function deleteMonthData() {
    if (confirm(`⚠️ ¿Está seguro de ELIMINAR TODOS los ${data[activeMonth].length} registros de ${MONTHS[activeMonth]}? Esta acción es irreversible.`)) {
      data[activeMonth] = [];
      // Ya no guardamos en localStorage
      showToast(`🗑️ Datos de ${MONTHS[activeMonth]} eliminados. ¡No olvide guardar el archivo de datos!`);
      showMonthPage(activeMonth);
    }
  }

  function exportToExcel() {
    const monthName = MONTHS[activeMonth];
    const ws = XLSX.utils.json_to_sheet(data[activeMonth].map(r => ({
      Fecha: formatDate(r.fecha),
      Material: r.material,
      Volquetadas: r.volquetadas,
      'Peso (TON)': r.peso,
      Destino: r.destino,
      Observaciones: r.observaciones,
      'Fecha Creación': r.timestamp
    })));

    const wb = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(wb, ws, monthName);
    XLSX.writeFile(wb, `Reporte_Carga_${monthName}.xlsx`);
    showToast('⬇️ Exportación a Excel completada.');
  }


  // ====================================================================
  // === CALCULADORA ===
  // ====================================================================
  const calculatorModal = document.getElementById('calculatorModal');
  const calcDisplay = document.getElementById('calcDisplay');
  let currentInput = '0';
  let prevInput = '0';
  let operator = null;
  let resultCalculated = false;

  function updateDisplay() {
    calcDisplay.textContent = currentInput === '' ? '0' : currentInput;
  }

  window.appendValue = function(val) {
    if (resultCalculated) {
      currentInput = '0';
      resultCalculated = false;
    }

    if (val === 'backspace') {
      currentInput = currentInput.slice(0, -1);
      if (currentInput === '') currentInput = '0';
    } else if (val === '.') {
      if (!currentInput.includes('.')) {
        currentInput += '.';
      }
    } else {
      if (currentInput === '0') {
        currentInput = val;
      } else {
        currentInput += val;
      }
    }
    updateDisplay();
  };

  window.setOperator = function(op) {
    if (operator && currentInput !== '0') {
      calculateResult();
    }
    prevInput = currentInput;
    operator = op;
    currentInput = '';
    resultCalculated = false;
    updateDisplay();
  };

  window.calculateResult = function() {
    if (!operator || resultCalculated) return;

    let res;
    const num1 = parseFloat(prevInput);
    const num2 = parseFloat(currentInput);

    switch (operator) {
      case '+': res = num1 + num2; break;
      case '-': res = num1 - num2; break;
      case '*': res = num1 * num2; break;
      case '/': 
        if (num2 === 0) {
          showToast('❌ División por cero', 'error');
          res = 0; // O manejar como error
        } else {
          res = num1 / num2;
        }
        break;
      default: return;
    }
    currentInput = res.toString();
    operator = null;
    prevInput = '0';
    resultCalculated = true;
    updateDisplay();
  };

  window.clearCalc = function() {
    currentInput = '0';
    operator = null;
    prevInput = '0';
    resultCalculated = false;
    updateDisplay();
  };

  window.showCalculator = function() {
    calculatorModal.classList.add('visible');
    clearCalc();
  };

  window.hideCalculator = function() {
    calculatorModal.classList.remove('visible');
  };

  document.getElementById('btnShowCalculator').addEventListener('click', showCalculator);
  
  // ====================================================================
  // === INICIALIZACIÓN Y EVENT LISTENERS ===
  // ====================================================================

  document.getElementById('search').addEventListener('input', function() {
    renderTable(this.value);
  });

  // Event Listeners para los botones de periodo
  document.addEventListener('DOMContentLoaded', () => {
    const periodButtons = document.querySelectorAll('.period-btn');
    periodButtons.forEach(button => {
        button.addEventListener('click', function() {
            // Desactivar todos los botones
            periodButtons.forEach(b => b.classList.remove('active'));
            
            // Activar el botón presionado
            this.classList.add('active');
            
            // Establecer la variable global activePeriod
            activePeriod = parseInt(this.dataset.period);
            
            // Re-renderizar la tabla y resúmenes con el filtro actual
            const searchEl = document.getElementById('search');
            renderTable(searchEl ? searchEl.value : ''); 

            // Ocultar sidebar en móvil si está abierto
            if(window.innerWidth<=768 && !sidebar.classList.contains('hidden')) toggleSidebar();
        });
    });
  });

  // Manejo de responsive para ocultar el sidebar
  if (window.innerWidth <= 768) {
      sidebar.classList.add('hidden');
      document.querySelector('.toggle-btn').classList.remove('hidden');
  } else {
      document.getElementById('content').classList.remove('full-width');
  }


  // Lógica de inicio: intenta migrar/cargar datos y luego renderiza
  initializeData();
  initializeActiveMonth();
  showMonthPage(activeMonth); 
</script>
</body>
</html>
