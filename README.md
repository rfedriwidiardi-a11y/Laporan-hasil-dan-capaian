# Laporan-hasil-dan-capaian
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Monitoring Kinerja Karyawan</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body{
  font-family:Arial, sans-serif;
  background:linear-gradient(135deg,#1e3c72,#2a5298);
  color:#fff;
  padding:20px;
}
.container{
  max-width:1100px;
  margin:auto;
  background:#111;
  padding:20px;
  border-radius:12px;
  box-shadow:0 0 20px rgba(0,0,0,.6);
}
h1{text-align:center}
table{
  width:100%;
  border-collapse:collapse;
  margin-top:15px;
}
th,td{
  border:1px solid #444;
  padding:8px;
  text-align:center;
}
th{background:#222}
input{
  width:100%;
  padding:5px;
  background:#000;
  color:#fff;
  border:1px solid #555;
}
button{
  padding:10px 15px;
  border:none;
  border-radius:6px;
  background:#00c6ff;
  cursor:pointer;
  font-weight:bold;
}
button:hover{background:#00aaff}
.notif{
  margin-top:15px;
  background:#300;
  padding:10px;
  border-radius:8px;
}
.ok{background:#033}
canvas{margin-top:20px}
</style>
</head>

<body>
<div class="container">
<h1>Monitoring Kinerja Karyawan</h1>

<button onclick="tambahBaris()">➕ Tambah Karyawan</button>

<table id="tabel">
<tr>
<th>Nama</th>
<th>Email</th>
<th>Target</th>
<th>Output</th>
<th>%</th>
</tr>
</table>

<br>
<button onclick="updateData()">UPDATE & CEK</button>

<div id="notif" class="notif"></div>

<canvas id="grafik"></canvas>
</div>

<script>
let chart;

function tambahBaris(){
  let t=document.getElementById("tabel");
  let r=t.insertRow();
  for(let i=0;i<5;i++){
    let c=r.insertCell();
    c.innerHTML=i<4?'<input>':'0%';
  }
}

function updateData(){
  let t=document.getElementById("tabel");
  let labels=[], values=[];
  let notif=[];
  
  for(let i=1;i<t.rows.length;i++){
    let nama=t.rows[i].cells[0].children[0].value;
    let email=t.rows[i].cells[1].children[0].value;
    let target=parseFloat(t.rows[i].cells[2].children[0].value)||0;
    let output=parseFloat(t.rows[i].cells[3].children[0].value)||0;
    let persen=target?((output/target)*100).toFixed(1):0;

    t.rows[i].cells[4].innerText=persen+"%";
    labels.push(nama||"Karyawan "+i);
    values.push(persen);

    if(persen<=50 && email){
      notif.push(`⚠️ ${nama} (${email}) di bawah 50%`);
    }
  }

  tampilGrafik(labels,values);

  document.getElementById("notif").innerHTML=
    notif.length?
    "<b>Notifikasi:</b><br>"+notif.join("<br>"):
    "<b>✅ Semua karyawan di atas 50%</b>";
}

function tampilGrafik(l,v){
  if(chart) chart.destroy();
  chart=new Chart(document.getElementById("grafik"),{
    type:"bar",
    data:{
      labels:l,
      datasets:[{
        label:"Persentase Kinerja",
        data:v
      }]
    }
  });
}
</script>
</body>
</html>
