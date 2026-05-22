<!DOCTYPE html>
<html lang="pt-BR">

<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Controle de Chips</title>

<style>

body{
    margin:0;
    padding:20px;
    background:#000;
    color:#fff;
    font-family:Arial,sans-serif;
}

h1{
    text-align:center;
    color:#00aaff;
    margin-bottom:20px;
}

.botoes{
    margin-bottom:15px;
}

button{
    background:#0066ff;
    color:white;
    border:none;
    padding:10px 15px;
    border-radius:6px;
    cursor:pointer;
    font-weight:bold;
    margin-right:5px;
}

button:hover{
    background:#004fc4;
}

table{
    width:100%;
    border-collapse:collapse;
    background:#111;
    min-width:1200px;
}

.table-box{
    overflow-x:auto;
}

th{
    background:#0066ff;
    border:1px solid #333;
    padding:10px;
    font-size:13px;
}

td{
    border:1px solid #333;
    padding:4px;
}

input,
textarea{
    width:100%;
    background:#1a1a1a;
    color:white;
    border:none;
    outline:none;
    padding:6px;
    box-sizing:border-box;
    font-size:13px;
}

textarea{
    min-height:40px;
    resize:vertical;
}

.excluir{
    background:#c70000;
}

.excluir:hover{
    background:#8f0000;
}

</style>

</head>

<body>

<h1>CONTROLE DE CHIPS</h1>

<div class="botoes">

<button onclick="adicionarLinha()">
Adicionar Linha
</button>

<button onclick="salvarDados()">
Salvar Online
</button>

</div>

<div class="table-box">

<table>

<thead>

<tr>

<th>Telefone</th>
<th>Operadora</th>
<th>Titular</th>
<th>CPF</th>
<th>Data Nascimento</th>
<th>Estado</th>
<th>Data Ativação</th>
<th>Data Recarga</th>
<th>Observações</th>
<th>Ação</th>

</tr>

</thead>

<tbody id="tabela"></tbody>

</table>

</div>

<script type="module">

import { initializeApp }

from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";

import {

getFirestore,
collection,
addDoc,
getDocs,
deleteDoc,
doc

}

from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

const firebaseConfig = {

apiKey: "AIzaSyDPYi6vfexZXDS_MUG14fuLufepK6TcDts",

authDomain: "controle-chips-c8559.firebaseapp.com",

projectId: "controle-chips-c8559",

storageBucket: "controle-chips-c8559.firebasestorage.app",

messagingSenderId: "94286679289",

appId: "1:94286679289:web:7b5fe74233febbe688a628"

};

const app = initializeApp(firebaseConfig);

const db = getFirestore(app);

const tabela = document.getElementById("tabela");

function criarLinha(dados = {}) {

const tr = document.createElement("tr");

tr.innerHTML = `

<td>
<input value="${dados.telefone || ''}" placeholder="Telefone">
</td>

<td>
<input value="${dados.operadora || ''}" placeholder="Operadora">
</td>

<td>
<input value="${dados.titular || ''}" placeholder="Titular">
</td>

<td>
<input value="${dados.cpf || ''}" placeholder="CPF">
</td>

<td>
<input type="date" value="${dados.nascimento || ''}">
</td>

<td>
<input value="${dados.estado || ''}" placeholder="Estado">
</td>

<td>
<input type="date" value="${dados.ativacao || ''}">
</td>

<td>
<input type="date" value="${dados.recarga || ''}">
</td>

<td>
<textarea placeholder="Observações">${dados.obs || ''}</textarea>
</td>

<td>
<button class="excluir"
onclick="this.closest('tr').remove()">
Excluir
</button>
</td>

`;

tabela.appendChild(tr);

}

window.adicionarLinha = function(){

criarLinha();

}

async function carregarDados(){

tabela.innerHTML = "";

const snapshot =
await getDocs(collection(db, "chips"));

if(snapshot.empty){

for(let i = 0; i < 10; i++){

criarLinha();

}

return;

}

snapshot.forEach((documento) => {

criarLinha(documento.data());

});

}

window.salvarDados = async function(){

const linhas =
document.querySelectorAll("#tabela tr");

const antigos =
await getDocs(collection(db, "chips"));

for(const item of antigos.docs){

await deleteDoc(doc(db, "chips", item.id));

}

for(const linha of linhas){

const campos =
linha.querySelectorAll("input, textarea");

await addDoc(collection(db, "chips"), {

telefone: campos[0].value,
operadora: campos[1].value,
titular: campos[2].value,
cpf: campos[3].value,
nascimento: campos[4].value,
estado: campos[5].value,
ativacao: campos[6].value,
recarga: campos[7].value,
obs: campos[8].value

});

}

alert("Dados salvos online!");

carregarDados();

}

carregarDados();

</script>

</body>

</html>
