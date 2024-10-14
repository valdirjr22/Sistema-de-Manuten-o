<Sistema de Manutenção>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Manutenção de Equipamentos</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }
        th {
            background-color: #f2f2f2;
        }
        .button {
            margin: 5px;
        }
        .guide {
            font-size: 0.9em;
            color: #555;
            margin-top: 5px;
        }
        .maintenance-details {
            display: none;
            margin-top: 10px;
        }
    </style>
</head>
<body>

<h1>Sistema de Manutenção de Equipamentos</h1>

<h2>Adicionar Equipamento</h2>
<select id="equipmentType">
    <option value="">Selecione o Tipo de Equipamento</option>
    <option value="TV">TV</option>
    <option value="Computador">Computador</option>
    <option value="Chromebook">Chromebook</option>
    <option value="Notgame">Notgame</option>
    <option value="Minipc">Minipc</option>
    <option value="Datashow">Datashow</option>
    <option value="Impressora">Impressora</option>
    <option value="Monitor">Monitor</option>
</select>
<div class="guide">* Selecione o tipo de equipamento.</div>
<input type="text" id="machineNumber" placeholder="Número da Máquina">
<div class="guide">* Exemplo: 001, 002, etc.</div>
<input type="text" id="tombNumber" placeholder="Número de Tombamento">
<div class="guide">* Exemplo: T001, T002, etc.</div>
<select id="sector">
    <option value="">Selecione o Setor</option>
    <option value="Coordenação">Coordenação</option>
    <option value="Direção">Direção</option>
    <option value="Secretaria">Secretaria</option>
    <option value="Sala dos Professores">Sala dos Professores</option>
    <option value="Sala do Financeiro">Sala do Financeiro</option>
    <option value="Recepção">Recepção</option>
    <option value="Biblioteca">Biblioteca</option>
    <option value="Sala de Inovação">Sala de Inovação</option>
    <option value="Sala Maker">Sala Maker</option>
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
<button class="button" onclick="addEquipment()">Adicionar Equipamento</button>

<h2>Pesquisar Serviços</h2>
<input type="text" id="searchInput" placeholder="Buscar por OS, Tombamento ou Máquina">
<button class="button" onclick="searchEquipment()">Pesquisar</button>

<h2>Lista de Equipamentos</h2>
<table id="equipmentTable">
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
    let osCounter = 1;  // Contador para o número da OS

    function saveEquipmentList() {
        localStorage.setItem('equipmentList', JSON.stringify(equipmentList));
        localStorage.setItem('osCounter', osCounter);  // Salvar contador de OS
    }

    function loadEquipmentList() {
        const storedList = localStorage.getItem('equipmentList');
        if (storedList) {
            return JSON.parse(storedList);
        }
        return [];
    }

    const equipmentList = loadEquipmentList();  // Carrega do localStorage
    osCounter = parseInt(localStorage.getItem('osCounter')) || 1;  // Carrega contador de OS
    updateEquipmentTable();  // Atualiza a tabela com os dados carregados

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
                status: 'Ativo',
                osNumber: null  // Inicializa como nulo
            };

            equipmentList.push(equipment);
            saveEquipmentList();  // Salva no localStorage
            updateEquipmentTable();
            clearInputs();
        } else {
            alert('Por favor, preencha todos os campos.');
        }
    }

    function updateEquipmentTable(filteredList = equipmentList) {
        const tableBody = document.getElementById('equipmentTable').getElementsByTagName('tbody')[0];
        tableBody.innerHTML = '';

        filteredList.forEach((equipment, index) => {
            const row = tableBody.insertRow();
            row.insertCell(0).textContent = equipment.type;
            row.insertCell(1).textContent = equipment.number;
            row.insertCell(2).textContent = equipment.tombNumber;
            row.insertCell(3).textContent = equipment.sector;

            row.insertCell(4).innerHTML = `
                <select onchange="updateMaintenanceType(${index}, this.value)">
                    <option value="">Selecione</option>
                    <option value="Preventiva">Preventiva</option>
                    <option value="Corretiva">Corretiva</option>
                </select>
                <div id="maintenanceDetails${index}" class="maintenance-details">
                    <input type="date" id="date${index}" placeholder="Data">
                    <input type="time" id="time${index}" placeholder="Hora">
                    <textarea id="details${index}" placeholder="Descrição do que foi feito"></textarea>
                    <button class="button" onclick="generateReport(${index})">Gerar Relatório</button>
                </div>
            `;
            
            // Exibindo o status com o número da OS, se existir
            row.insertCell(5).textContent = equipment.status + (equipment.osNumber ? ` (OS: ${equipment.osNumber})` : '');

            row.insertCell(6).innerHTML = `
                <button onclick="printReport(${index})">Imprimir Relatório</button>
                <button onclick="removeEquipment(${index})">Remover</button>
                <button class="button" onclick="showFullReport(${index})">Gerar Relatório</button>
            `;
        });
    }

    function updateMaintenanceType(index, type) {
        equipmentList[index].maintenanceType = type;
        const detailsDiv = document.getElementById(`maintenanceDetails${index}`);
        detailsDiv.style.display = type === 'Preventiva' ? 'block' : 'none';
    }

    function generateReport(index) {
        const date = document.getElementById(`date${index}`).value;
        const time = document.getElementById(`time${index}`).value;
        const details = document.getElementById(`details${index}`).value;

        if (date && time && details) {
            const report = {
                osNumber: `OS-${osCounter++}`,  // Gerando número da OS automaticamente
                date,
                time,
                details,
                nextMaintenance: new Date(new Date(date).getTime() + 30 * 24 * 60 * 60 * 1000).toLocaleDateString()
            };

            equipmentList[index].maintenanceHistory.push(report);
            equipmentList[index].osNumber = report.osNumber;  // Atualiza o número da OS
            equipmentList[index].status = 'Ativo';  // Atualiza o status
            saveEquipmentList();  // Salva no localStorage
            updateEquipmentTable();  // Atualiza a tabela
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
                reportContent += `Relatório (OS: ${report.osNumber}):\n`;  // Incluindo o número da OS
                reportContent += `Data: ${report.date}\n`;
                reportContent += `Hora: ${report.time}\n`;
                reportContent += `Descrição: ${report.details}\n`;
                reportContent += `Próxima Manutenção: ${report.nextMaintenance}\n\n`;
            });
        }

        alert(reportContent);  // Mostrando o conteúdo do relatório em um alerta
    }

    function printReport(index) {
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
                reportContent += `Relatório (OS: ${report.osNumber}):\n`;  // Incluindo o número da OS
                reportContent += `Data: ${report.date}\n`;
                reportContent += `Hora: ${report.time}\n`;
                reportContent += `Descrição: ${report.details}\n`;
                reportContent += `Próxima Manutenção: ${report.nextMaintenance}\n\n`;
            });
        }

        const printWindow = window.open('', '', 'width=600,height=400');
        printWindow.document.write('<pre>' + reportContent + '</pre>');
        printWindow.document.close();
        printWindow.print();
    }

    function removeEquipment(index) {
        equipmentList.splice(index, 1);
        saveEquipmentList();  // Salva no localStorage
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
</script>

</body>
</html>
