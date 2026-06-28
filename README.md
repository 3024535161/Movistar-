<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>CRM Movistar Portabilidad</title>

<style>
body{
font-family:Arial;
background:#0b1220;
color:white;
margin:0;
padding:20px;
}

h1{
text-align:center;
color:#22c55e;
}

.container{
max-width:1200px;
margin:auto;
}

form{
background:#111c33;
padding:15px;
border-radius:10px;
margin-bottom:15px;
}

.row{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:10px;
}

input{
width:100%;
padding:10px;
border:none;
border-radius:5px;
margin:5px 0;
}

button{
padding:8px;
border:none;
border-radius:5px;
cursor:pointer;
}

.add{
background:#22c55e;
color:black;
margin-top:10px;
width:100%;
}

.search{
width:100%;
padding:10px;
margin-bottom:10px;
border-radius:5px;
border:none;
}

table{
width:100%;
border-collapse:collapse;
background:#111c33;
}

th,td{
border:1px solid #223;
padding:8px;
text-align:center;
font-size:13px;
}

.badge{
padding:4px 8px;
border-radius:5px;
}

.red{background:#ef4444;}
.orange{background:#f59e0b;color:black;}
.blue{background:#38bdf8;color:black;}
.green{background:#22c55e;color:black;}
.purple{background:#a855f7;}

.small-btn{
font-size:11px;
padding:5px;
margin:2px;
}
</style>
</head>

<body>

<div class="container">

<h1>CRM PORTABILIDAD MOVISTAR</h1>

<!-- FORMULARIO ORDENADO -->
<form id="form">

<div class="row">
<input id="nombre" placeholder="Nombre cliente" required>
<input id="cedula" placeholder="Cédula" required>
<input id="exp" type="date" required>
</div>

<div class="row">
<input id="numero" placeholder="Número a portar" required>
<input id="nip" placeholder="NIP" required>
<input id="ciudad" placeholder="Ciudad" required>
</div>

<div class="row">
<input id="departamento" placeholder="Departamento" required>
<input id="entrega" type="date" placeholder="Fecha entrega SIM">
</div>

<button class="add" type="submit">Guardar Cliente</button>

</form>

<input id="buscar" class="search" placeholder="Buscar cliente..." onkeyup="filtrar()">

<!-- TABLA -->
<table>
<thead>
<tr>
<th>Cliente</th>
<th>Cédula</th>
<th>EXP</th>
<th>Número</th>
<th>NIP</th>
<th>Ciudad</th>
<th>Departamento</th>
<th>Entrega SIM</th>
<th>SIM</th>
<th>Activación</th>
<th>Consumo</th>
<th>Chichón</th>
<th>Estado</th>
<th>Acciones</th>
</tr>
</thead>

<tbody id="tabla"></tbody>
</table>

</div>

<script>

let data = JSON.parse(localStorage.getItem("crm")) || [];

document.getElementById("form").addEventListener("submit",e=>{
e.preventDefault();

data.push({
nombre:nombre.value,
cedula:cedula.value,
exp:exp.value,
numero:numero.value,
nip:nip.value,
ciudad:ciudad.value,
departamento:departamento.value,
entrega:entrega.value,

sim:false,
activacion:false,
consumo:false,
chichon:false
});

save();
form.reset();
render();
});

function render(){
let t=document.getElementById("tabla");
t.innerHTML="";

data.forEach((c,i)=>{

let estado="Pendiente SIM";
let color="red";

if(c.chichon){estado="Chichón";color="purple";}
else if(c.consumo){estado="Con consumo";color="green";}
else if(c.activacion){estado="Activado";color="blue";}
else if(c.sim){estado="SIM entregada";color="orange";}

t.innerHTML+=`
<tr>
<td>${c.nombre}</td>
<td>${c.cedula}</td>
<td>${c.exp}</td>
<td>${c.numero}</td>
<td>${c.nip}</td>
<td>${c.ciudad}</td>
<td>${c.departamento}</td>
<td>${c.entrega}</td>

<td><button onclick="toggleSIM(${i})">${c.sim?"✔":"✖"}</button></td>
<td><button onclick="toggleAct(${i})">${c.activacion?"✔":"✖"}</button></td>
<td><button onclick="toggleCon(${i})">${c.consumo?"✔":"✖"}</button></td>
<td><button onclick="toggleChi(${i})">${c.chichon?"✔":"✖"}</button></td>

<td><span class="badge ${color}">${estado}</span></td>

<td><button onclick="del(${i})">Eliminar</button></td>
</tr>
`;
});
}

function toggleSIM(i){data[i].sim=!data[i].sim;save();}
function toggleAct(i){data[i].activacion=!data[i].activacion;save();}
function toggleCon(i){data[i].consumo=!data[i].consumo;save();}
function toggleChi(i){data[i].chichon=!data[i].chichon;save();}

function del(i){data.splice(i,1);save();}

function save(){
localStorage.setItem("crm",JSON.stringify(data));
render();
}

function filtrar(){
let v=buscar.value.toLowerCase();
document.querySelectorAll("tbody tr").forEach(r=>{
r.style.display=r.innerText.toLowerCase().includes(v)?"":"none";
});
}

render();

</script>

</body>
</html>
