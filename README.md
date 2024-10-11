<Sistema de manutenção>
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
    <option value="Monitor">Monitor</option> <!-- Adicionado Monitor -->
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
    <option value="Sala de Vidro">Sala de Vidro</option> <!-- Adicionado Sala de Vidro -->
</select>
<div class="guide">* Selecione o setor ao qual o equipamento pertence.</div>
<button class="button" onclick="addEquipment()">Adicionar Equipamento</button>

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
    const equipmentList = [];

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
                status: 'Ativo'
            };

            equipmentList.push(equipment);
            updateEquipmentTable();
            clearInputs();
        } else {
            alert('Por favor, preencha todos os campos.');
        }
    }

    function updateEquipmentTable() {
        const tableBody = document.getElementById('equipmentTable').getElementsByTagName('tbody')[0];
        tableBody.innerHTML = '';

        equipmentList.forEach((equipment, index) => {
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
                    <button onclick="generateReport(${index})">Gerar Relatório</button>
                    <button onclick="printReport(${index})">Imprimir Relatório</button>
                </div>
            `;
            row.insertCell(5).textContent = equipment.status;
            row.insertCell(6).innerHTML = `
                <button onclick="removeEquipment(${index})">Remover</button>
                <button onclick="viewMaintenanceHistory(${index})">Histórico</button>
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
                date,
                time,
                details,
                nextMaintenance: new Date(new Date(date).getTime() + 30 * 24 * 60 * 60 * 1000).toLocaleDateString()
            };

            equipmentList[index].maintenanceHistory.push(report);
            alert('Relatório de manutenção gerado e salvo com sucesso!');
        } else {
            alert('Por favor, preencha todos os campos de manutenção.');
        }
    }

    function printReport(index) {
        const history = equipmentList[index].maintenanceHistory;
        let reportContent = `Relatório de Manutenção para ${equipmentList[index].number}:\n`;
        reportContent += `Número de Tombamento: ${equipmentList[index].tombNumber}\n`;
        reportContent += `Responsável: Valdir Rodrigues\n`;
        reportContent += `Função: Monitor de TI\n`;
        reportContent += `Instituição: SENAC PAULISTA\n\n`; // Adicionado nome da instituição

        if (history.length === 0) {
            reportContent += 'Nenhum registro de manutenção encontrado.';
        } else {
            history.forEach((report, i) => {
                reportContent += `Relatório ${i + 1}:\n`;
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

    function viewMaintenanceHistory(index) {
        const history = equipmentList[index].maintenanceHistory;
        let historyContent = `Histórico de Manutenção para ${equipmentList[index].number}:\n`;

        if (history.length === 0) {
            historyContent += 'Nenhum registro de manutenção encontrado.';
        } else {
            history.forEach((report, i) => {
                historyContent += `Relatório ${i + 1}:\n`;
                historyContent += `Data: ${report.date}\n`;
                historyContent += `Hora: ${report.time}\n`;
                historyContent += `Descrição: ${report.details}\n`;
                historyContent += `Próxima Manutenção: ${report.nextMaintenance}\n\n`;
            });
        }

        alert(historyContent);
    }

    function removeEquipment(index) {
        equipmentList.splice(index, 1);
        updateEquipmentTable();
    }

    function clearInputs() {
        document.getElementById('equipmentType').value = '';
        document.getElementById('machineNumber').value = '';
        document.getElementById('tombNumber').value = '';
        document.getElementById('sector').value = '';
    }
</script>

</body>
</html>
