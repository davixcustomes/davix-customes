<!DOCTYPE html>
<html lang="ro">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DAVIX CUSTOMES</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    color: #222;
    background: url('https://images.pexels.com/photos/110844/pexels-photo-110844.jpeg?auto=compress&cs=tinysrgb&h=750&w=1260') no-repeat center center fixed;
    background-size: cover;
    position: relative;
}

body::before {
    content: "";
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(255, 255, 255, 0.85);
    z-index: -1;
}

header {
    background: #111;
    color: white;
    padding: 20px;
    text-align: center;
    font-size: 26px;
}

nav {
    background: #222;
    padding: 12px;
    text-align: center;
}

nav a {
    color: white;
    margin: 0 15px;
    text-decoration: none;
    font-size: 18px;
}

nav a:hover {
    color: #00ffa0;
}

.section {
    width: 90%;
    max-width: 900px;
    margin: 40px auto;
    background: white;
    padding: 30px;
    border-radius: 12px;
    box-shadow: 0 0 15px #00000020;
}

h2 {
    border-left: 5px solid #111;
    padding-left: 10px;
}

/* --- GRILA 2x2 --- */
.services {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(2, auto);
    gap: 20px;
    justify-items: center;
}

@media (max-width: 600px) {
  .services {
    grid-template-columns: 1fr;
    grid-template-rows: auto;
  }
}

.card {
    padding: 20px;
    background: #fafafa;
    border-radius: 10px;
    box-shadow: 0 0 10px #00000010;
    text-align: center;
    width: 100%;
}

input, select, button {
    width: 100%;
    padding: 14px;
    margin-top: 12px;
    border-radius: 8px;
    border: 1px solid #ccc;
    font-size: 16px;
}

button {
    background: #111;
    color: white;
    cursor: pointer;
}

button:hover {
    background: #333;
}
</style>
</head>
<body>

<header>DAVIX CUSTOMES </header>

<nav>
<a href="#acasa">Acasă</a>
<a href="#servicii">Servicii</a>
<a href="#programari">Programări</a>
</nav>

<!-- ACASA -->
<div class="section" id="acasa">
<h2>Bine ai venit!</h2>
<p>
La <b>Davix Customes</b> oferim servicii profesionale de reparații auto, vopsitorie și polish.  
Programează-te rapid, totul este simplu și gratuit!
</p>
</div>

<!-- SERVICII -->
<div class="section" id="servicii">
<h2>Serviciile noastre</h2>
<div class="services">
<div class="card">
<h3>🔧 Reparații Auto</h3>
<p>Diagnoză, mecanică, tinichigerie – prețuri corecte și calitate premium.</p>
</div>
<div class="card">
<h3>🎨 Vopsitorie Auto</h3>
<p>Cabină profesionistă, culori premium și rezultate impecabile.</p>
</div>
<div class="card">
<h3>✨ Polish Auto</h3>
<p>Lustruire profesională, corecție lac, protecție ceramică.</p>
</div>
<div class="card">
<h3>🧼 Curățare Interior</h3>
<p>Curățare profesională a tapițeriei, covorașelor și bordului, pentru mașina ta ca nouă.</p>
</div>
</div>
</div>

<!-- PROGRAMARI -->
<div class="section" id="programari">
<h2>Fă o programare</h2>
<label>Serviciu</label>
<select id="service">
<option value="Reparații auto">Reparații auto</option>
<option value="Vopsitorie auto">Vopsitorie auto</option>
<option value="Polish profesional">Polish profesional</option>
<option value="Curatare interior">Curățare interior</option>
</select>

<label>Nume</label>
<input type="text" id="name">

<label>Număr de telefon</label>
<input type="text" id="phone">

<label>Data programării</label>
<select id="date"></select>

<label>Ora programării</label>
<select id="hour"></select>

<button onclick="sendToWhatsApp()">Trimite pe WhatsApp</button>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
    const hourSelect = document.getElementById("hour");
    const dateSelect = document.getElementById("date");
    const today = new Date();

    // Generare date pentru 30 zile, fără duminica
    for(let i=0; i<30; i++){
        let d = new Date();
        d.setDate(today.getDate()+i);
        if(d.getDay()===0) continue; // duminica blocată
        let day = d.getDate()<10 ? "0"+d.getDate() : d.getDate();
        let month = (d.getMonth()+1)<10 ? "0"+(d.getMonth()+1) : (d.getMonth()+1);
        let display = `${day}.${month}`;
        let option = document.createElement("option");
        option.value = d.getDay(); // ziua săptămânii
        option.textContent = display;
        dateSelect.appendChild(option);
    }

    // Populare ore inițial
    populateHours(parseInt(dateSelect.value));

    // Actualizare ore la schimbare dată
    dateSelect.addEventListener("change", function() {
        populateHours(parseInt(this.value));
    });

    function populateHours(dayOfWeek){
        hourSelect.innerHTML = "";
        let startHour = 9, endHour = 18;
        if(dayOfWeek === 6){ // sâmbătă
            startHour = 10;
            endHour = 14;
        }
        for(let h=startHour; h<=endHour; h++){
            for(let m of [0,30]){
                if(h===endHour && m>0) continue;
                let hh = h<10 ? "0"+h : h;
                let mm = m===0 ? "00" : "30";
                let option = document.createElement("option");
                option.value = `${hh}:${mm}`;
                option.textContent = `${hh}:${mm}`;
                hourSelect.appendChild(option);
            }
        }
    }
});

function sendToWhatsApp(){
    const service = document.getElementById("service").value;
    const name = document.getElementById("name").value;
    const phone = document.getElementById("phone").value;
    const date = document.getElementById("date").selectedOptions[0].text;
    const hour = document.getElementById("hour").value;

    if(!name || !phone || !date || !hour){
        alert("Completează toate câmpurile!");
        return;
    }

    const yourNumber = "40761884120";

    const text =
    `Salut! Vreau să fac o programare:\n\n` +
    `Nume: ${name}\n` +
    `Telefon: ${phone}\n` +
    `Serviciu: ${service}\n` +
    `Data: ${date}\n` +
    `Ora: ${hour}`;

    const url = `https://wa.me/${yourNumber}?text=${encodeURIComponent(text)}`;
    window.open(url,"_blank");
}
</script>

</body>
</html>
](https://magdaiosif2015-hue.github.io/davix-customes/)
