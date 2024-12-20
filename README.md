<Sistema de Manuteção Eqipamentos de Informática>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Manutenção de Equipamentos</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 20px;
            background-color: #f4f4f4;
            color: #333;
            animation: fadeIn 0.5s ease;
        }
        h1, h2 {
            color: #34495e;
            margin-bottom: 10px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            transition: transform 0.2s;
        }
        table:hover {
            transform: scale(1.01);
        }
        th, td {
            border: 1px solid #ddd;
            padding: 12px;
            text-align: center;
            transition: background-color 0.3s;
        }
        th {
            background-color: #3498db;
            color: white;
        }
        tr:hover {
            background-color: #ecf0f1;
        }
        .button {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 10px 15px;
            cursor: pointer;
            border-radius: 5px;
            transition: background-color 0.3s, transform 0.2s;
        }
        .button:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
        }
        .guide {
            font-size: 0.9em;
            color: #555;
            margin-top: 5px;
        }
        .maintenance-details {
            display: none;
            margin-top: 10px;
            background-color: #ecf0f1;
            padding: 10px;
            border-radius: 5px;
            transition: all 0.3s ease;
        }
        input[type="text"], select {
            width: calc(100% - 20px);
            padding: 10px;
            margin: 5px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            transition: border 0.3s;
        }
        input[type="text"]:focus, select:focus {
            border: 1px solid #3498db;
            outline: none;
        }
    </style>
</head>
<body>

<h1 class="animate__animated animate__fadeInDown">Sistema de Manutenção de Equipamentos</h1>

<h2 class="animate__animated animate__fadeIn">Adicionar Equipamento</h2>
<select id="equipmentType" required class="animate__animated animate__fadeIn animate__delay-1s">
    <option value="">Selecione o Tipo de Equipamento</option>
    <option value="TV">TV</option>
    <option value="Computador">Computador</option>
    <option value="Chromebook">Chromebook</option>
    <option value="Notgame">Notgame</option>
    <option value="Minipc">Minipc</option>
    <option value="Datashow">Datashow</option>
    <option value="Impressora">Impressora</option>
    <option value="Monitor">Monitor</option>
    <option value="Teclado">Teclado</option>
    <option value="Mouse">Mouse</option>
</select>
<div class="guide">* Selecione o tipo de equipamento.</div>
<input type="text" id="machineNumber" placeholder="Número da Máquina" required class="animate__animated animate__fadeIn animate__delay-1s">
<div class="guide">* Exemplo: 001, 002, etc.</div>
<input type="text" id="tombNumber" placeholder="Número de Tombamento" required class="animate__animated animate__fadeIn animate__delay-1s">
<div class="guide">* Exemplo: T001, T002, etc.</div>
<select id="sector" required class="animate__animated animate__fadeIn animate__delay-1s">
    <option value="">Selecione o Setor</option>
    <option value="Coordenação">Coordenação</option>
    <option value="Direção">Direção</option>
    <option value="Secretaria">Secretaria</option>
    <option value="Sala dos Professores">Sala dos Professores</option>
    <option value="Sala do Financeiro">Sala do Financeiro</option>
    <option value="Recepção">Recepção</option>
    <option value="Biblioteca">Biblioteca</option>
    <option value="Sala de Inovação">Sala de Inovação</option>
    <option value="Sala (Maker)">Sala (Maker)</option>
    <option value="Sala 01">Sala 01</option>
    <option value="Sala 02">Sala 02</option>
    <option value="Sala 03">Sala 03</option>
    <option value="Sala 04">Sala 04</option>
    <option value="Sala 05">Sala 05</option>
    <option value="Sala 06">Sala 06</option>
    <option value="Sala 07">Sala 07</option>
    <option value="Sala 08">Sala 08</option>
    <option value="Sala 09">Sala 09</option>
    <option value="Sala 10">Sala 10</option>
    <option value="Sala 11">Sala 11</option>
    <option value="Sala 12">Sala 12</option>
    <option value="Auditório">Auditório</option>
    <option value="Salão Escola 01">Salão Escola 01</option>
    <option value="Salão Escola 02">Salão Escola 02</option>
    <option value="Sala Cozinha Didática">Sala Cozinha Didática</option>
    <option value="Sala de Vidro">Sala de Vidro</option>
    <option value="Sala do Administrativo">Sala do Administrativo</option>
</select>
<div class="guide">* Selecione o setor ao qual o equipamento pertence.</div>
<button class="button animate__animated animate__fadeIn animate__delay-1s" onclick="addEquipment()">Adicionar Equipamento</button>

<h2 class="animate__animated animate__fadeIn">Pesquisar Serviços</h2>
<input type="text" id="searchInput" placeholder="Buscar por OS, Tombamento ou Máquina" class="animate__animated animate__fadeIn animate__delay-1s">
<button class="button animate__animated animate__fadeIn animate__delay-1s" onclick="searchEquipment()">Pesquisar</button>
<button class="button animate__animated animate__fadeIn animate__delay-1s" onclick="showGeneralHistory()">Histórico Geral</button>
<button class="button animate__animated animate__fadeIn animate__delay-1s" onclick="printGeneralHistory()">Imprimir Histórico Geral</button>

<h2 class="animate__animated animate__fadeIn">Lista de Equipamentos</h2>
<table id="equipmentTable" class="animate__animated animate__fadeIn animate__delay-1s">
    <thead>
        <tr>
            <th>Tipo</th>
            <th>Número</th>
            <th>Número de Tombamento</th>
            <th>Setor</th>
            <th>Tipo de Manutenção</th>
            <th>Status</th>
            <th>Ações</th>
        </tr>
    </thead>
    <tbody>
        <!-- Equipamentos serão adicionados aqui -->
    </tbody>
</table>

<script>
    let osCounter = 1; // Contador para o número da OS

    function saveEquipmentList() {
        localStorage.setItem('equipmentList', JSON.stringify(equipmentList));
        localStorage.setItem('osCounter', osCounter); // Salvar contador de OS
    }

    function loadEquipmentList() {
        const storedList = localStorage.getItem('equipmentList');
        return storedList ? JSON.parse(storedList) : [];
    }

    const equipmentList = loadEquipmentList(); // Carrega do localStorage
    osCounter = parseInt(localStorage.getItem('osCounter')) || 1; // Carrega contador de OS
    updateEquipmentTable(); // Atualiza a tabela com os dados carregados

    function addEquipment() {
        const type = document.getElementById('equipmentType').value;
        const number = document.getElementById('machineNumber').value;
        const tombNumber = document.getElementById('tombNumber').value;
        const sector = document.getElementById('sector').value;

        if (type && number && tombNumber && sector) {
            const equipment = {
                type,
                number,
                tombNumber,
                sector,
                maintenanceHistory: [],
                status: 'Inativo',
                maintenanceType: ''
            };

            equipmentList.push(equipment);
            saveEquipmentList();
            updateEquipmentTable();
            clearInputs();
            alert('Equipamento adicionado com sucesso!');
        } else {
            alert('Por favor, preencha todos os campos.');
        }
    }

    function updateEquipmentTable(filteredList = equipmentList) {
        const tableBody = document.getElementById('equipmentTable').querySelector('tbody');
        tableBody.innerHTML = ''; // Limpa a tabela antes de atualizar

        filteredList.forEach((equipment, index) => {
            const row = tableBody.insertRow();
            row.insertCell(0).textContent = equipment.type || 'N/A';
            row.insertCell(1).textContent = equipment.number || 'N/A';
            row.insertCell(2).textContent = equipment.tombNumber || 'N/A';
            row.insertCell(3).textContent = equipment.sector || 'N/A';
            row.insertCell(4).innerHTML = `
                <select onchange="updateMaintenanceType(${index}, this.value)">
                    <option value="">Selecione</option>
                    <option value="Preventiva">Preventiva</option>
                    <option value="Corretiva">Corretiva</option>
                    <option value="Urgente">Urgente</option>
                </select>
                <div id="maintenanceDetails${index}" class="maintenance-details">
                    <input type="date" id="date${index}" required>
                    <input type="time" id="time${index}" required>
                    <textarea id="details${index}" placeholder="Descrição da manutenção" required></textarea>
                    <button class="button" onclick="generateReport(${index})">Gerar Relatório</button>
                </div>
            `;

            row.insertCell(5).textContent = equipment.status || 'N/A';
            row.insertCell(6).innerHTML = `
                <button class="button" onclick="printReport(${index})">Imprimir Relatório</button>
                <button class="button" onclick="removeEquipment(${index})">Remover</button>
                <button class="button" onclick="showFullReport(${index})">Gerar Relatório</button>
                <button class="button" onclick="showMaintenanceHistory(${index})">Histórico</button>
            `;
        });
    }

    function updateMaintenanceType(index, type) {
        equipmentList[index].maintenanceType = type;
        const detailsDiv = document.getElementById(`maintenanceDetails${index}`);
        detailsDiv.style.display = type ? 'block' : 'none';
        detailsDiv.classList.add('animate__animated', 'animate__fadeIn');
    }

    function generateReport(index) {
        const date = document.getElementById(`date${index}`).value;
        const time = document.getElementById(`time${index}`).value;
        const details = document.getElementById(`details${index}`).value;

        if (date && time && details) {
            const report = {
                osNumber: `OS-${osCounter++}`,
                date,
                time,
                details,
                maintenanceType: equipmentList[index].maintenanceType,
                nextMaintenance: new Date(new Date(date).getTime() + 30 * 24 * 60 * 60 * 1000).toLocaleDateString()
            };

            equipmentList[index].maintenanceHistory.push(report);
            equipmentList[index].osNumber = report.osNumber;
            equipmentList[index].status = 'Ativo';
            saveEquipmentList();
            updateEquipmentTable();
            alert('Relatório de manutenção gerado e salvo com sucesso!');
        } else {
            alert('Por favor, preencha todos os campos de manutenção.');
        }
    }

    function showFullReport(index) {
        const history = equipmentList[index].maintenanceHistory;
        let reportContent = `Relatório de Manutenção para ${equipmentList[index].number}:\n`;
        reportContent += `Número de Tombamento: ${equipmentList[index].tombNumber}\n`;
        reportContent += `Responsável: Valdir Rodrigues\n`;
        reportContent += `Função: Monitor de TI\n`;
        reportContent += `Instituição: SENAC PAULISTA\n\n`;

        if (history.length === 0) {
            reportContent += 'Nenhum registro de manutenção encontrado.';
        } else {
            history.forEach((report) => {
                reportContent += `Relatório (OS: ${report.osNumber}):\n`;
                reportContent += `Data: ${report.date}\n`;
                reportContent += `Hora: ${report.time}\n`;
                reportContent += `Tipo: ${report.maintenanceType}\n`;
                reportContent += `Descrição: ${report.details}\n`;
                reportContent += `Próxima Manutenção: ${report.nextMaintenance}\n\n`;
            });
        }

        alert(reportContent);
    }

    function showMaintenanceHistory(index) {
        const history = equipmentList[index].maintenanceHistory;
        let historyContent = `Histórico de Manutenção para ${equipmentList[index].number}:\n\n`;

        if (history.length === 0) {
            historyContent += 'Nenhum registro de manutenção encontrado.';
        } else {
            history.forEach((report) => {
                historyContent += `Relatório (OS: ${report.osNumber}):\n`;
                historyContent += `Data: ${report.date}\n`;
                historyContent += `Hora: ${report.time}\n`;
                historyContent += `Tipo: ${report.maintenanceType}\n`;
                historyContent += `Descrição: ${report.details}\n`;
                historyContent += `Próxima Manutenção: ${report.nextMaintenance}\n\n`;
            });
        }

        alert(historyContent);
    }

    function printReport(index) {
        const history = equipmentList[index].maintenanceHistory;
        let reportContent = `Equipamento: ${equipmentList[index].number}\n`;
        reportContent += `Tombamento: ${equipmentList[index].tombNumber}\n`;
        reportContent += `Tipo de Manutenção: ${history.length ? history[history.length - 1].maintenanceType : 'N/A'}\n`;
        reportContent += `OS: ${equipmentList[index].osNumber || 'N/A'}\n`;

        if (history.length > 0) {
            const lastReport = history[history.length - 1];
            reportContent += `Data: ${lastReport.date} - Hora: ${lastReport.time}\n`;
            reportContent += `Desc: ${lastReport.details}\n`;
            reportContent += `Próx. Manutenção: ${lastReport.nextMaintenance}\n`;
        } else {
            reportContent += 'Nenhum registro de manutenção encontrado.';
        }

        const printWindow = window.open('', '', 'width=300,height=200');
        printWindow.document.write('<pre>' + reportContent + '</pre>');
        printWindow.document.close();
        printWindow.print();
    }

    function removeEquipment(index) {
        equipmentList.splice(index, 1);
        saveEquipmentList();
        updateEquipmentTable();
    }

    function clearInputs() {
        document.getElementById('equipmentType').value = '';
        document.getElementById('machineNumber').value = '';
        document.getElementById('tombNumber').value = '';
        document.getElementById('sector').value = '';
    }

    function searchEquipment() {
        const searchValue = document.getElementById('searchInput').value.toLowerCase();
        const filteredList = equipmentList.filter(equipment => 
            equipment.number.toLowerCase().includes(searchValue) || 
            equipment.tombNumber.toLowerCase().includes(searchValue) || 
            (equipment.osNumber && equipment.osNumber.toLowerCase().includes(searchValue))
        );
        updateEquipmentTable(filteredList);
    }

    function showGeneralHistory() {
        let generalHistory = 'Histórico Geral de Manutenção:\n\n';
        let hasHistory = false;

        equipmentList.forEach(equipment => {
            if (equipment.maintenanceHistory.length > 0) {
                hasHistory = true;
                generalHistory += `Equipamento: ${equipment.number} (Tombamento: ${equipment.tombNumber})\n`;
                equipment.maintenanceHistory.forEach(report => {
                    generalHistory += `  OS: ${report.osNumber}\n`;
                    generalHistory += `  Data: ${report.date}\n`;
                    generalHistory += `  Hora: ${report.time}\n`;
                    generalHistory += `  Tipo: ${report.maintenanceType}\n`;
                    generalHistory += `  Descrição: ${report.details}\n`;
                    generalHistory += `  Próxima Manutenção: ${report.nextMaintenance}\n\n`;
                });
            }
        });

        if (!hasHistory) {
            generalHistory = 'Nenhum registro de manutenção encontrado.';
        }

        alert(generalHistory);
    }

    function printGeneralHistory() {
        let generalHistory = 'Histórico Geral de Manutenção:\n\n';
        let hasHistory = false;

        equipmentList.forEach(equipment => {
            if (equipment.maintenanceHistory.length > 0) {
                hasHistory = true;
                generalHistory += `Equipamento: ${equipment.number} (Tombamento: ${equipment.tombNumber})\n`;
                equipment.maintenanceHistory.forEach(report => {
                    generalHistory += `  OS: ${report.osNumber}\n`;
                    generalHistory += `  Data: ${report.date}\n`;
                    generalHistory += `  Hora: ${report.time}\n`;
                    generalHistory += `  Tipo: ${report.maintenanceType}\n`;
                    generalHistory += `  Descrição: ${report.details}\n`;
                    generalHistory += `  Próxima Manutenção: ${report.nextMaintenance}\n\n`;
                });
            }
        });

        if (!hasHistory) {
            generalHistory = 'Nenhum registro de manutenção encontrado.';
        }

        const printWindow = window.open('', '', 'width=300,height=200');
        printWindow.document.write('<pre>' + generalHistory + '</pre>');
        printWindow.document.close();
        printWindow.print();
    }
</script>

</body>
</html>
