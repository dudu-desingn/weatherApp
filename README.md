# weatherApp
site para saber previsao do tempo
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Previsão do Tempo 🌦️</title>
  <link rel="stylesheet" href="jornada.css">
</head>

<body>

  <div class="div-up">
    <p>🌎 Site para Previsão do Tempo e Agricultura 🌱</p>
  </div>

  <form class="weatherForm">
    <input type="text" class="cityInput" placeholder="Digite a cidade">
    <button type="submit">Buscar Clima</button>
  </form>

  <div class="card" style="display: none"></div>

  <script src="Jorney_Node.js"></script>

</body>

/* RESET E BODY */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body{
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(180deg, #f0f4f3, #e2f0e9);
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
    color: #103457;
}

/* TOPO */
.div-up{
    background: linear-gradient(180deg, #9fffC7, #bdffbd);
    width: 100%;
    height: 100px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 32px;
    font-weight: 700;
    letter-spacing: 1px;
    color: #103457;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    transition: background 0.5s ease;
}

.div-up p{
    transition: transform 0.5s ease;
}

.div-up p:hover{
    transform: scale(1.1);
}

/* FORMULÁRIO */
.weatherForm{
    margin-top: 50px;
    background: linear-gradient(180deg, #c3ee9b, #9bd4bf);
    padding: 25px 40px;
    border-radius: 25px;
    display: flex;
    align-items: center;
    gap: 20px;
    box-shadow: 0 6px 15px rgba(0,0,0,0.1);
    transition: transform 0.3s ease;
}

.weatherForm:hover {
    transform: translateY(-5px);
}

.cityInput{
    padding: 18px 20px;
    width: 300px;
    border-radius: 20px;
    font-size: 22px;
    outline: none;
    border: none;
    box-shadow: inset 0 2px 6px rgba(0,0,0,0.1);
    transition: box-shadow 0.3s ease;
}

.cityInput:focus {
    box-shadow: inset 0 2px 8px rgba(0,0,0,0.2);
}

button{
    font-size: 22px;
    padding: 18px 30px;
    border: none;
    border-radius: 20px;
    background-color: #fffde3;
    color: #103457;
    cursor: pointer;
    font-weight: 600;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    transition: all 0.3s ease;
}

button:hover{
    background-color: #e9e7c9;
    transform: scale(1.05);
}

/* CARD */
.card{
    background: linear-gradient(180deg, #9bd4bf, #c3ee9b);
    margin-top: 30px;
    border-radius: 25px;
    padding: 40px 60px;
    min-width: 400px;
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #103457;
    box-shadow: 0 8px 20px rgba(0,0,0,0.15);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 25px rgba(0,0,0,0.2);
}

/* TEXTO DO CARD */
.cityDisplay{
    font-size: 48px;
    font-weight: 700;
    margin-bottom: 15px;
}

.tempDisplay{
    font-size: 46px;
    margin: 10px 0;
}

.humidityDisplay, .descDisplay{
    font-size: 32px;
    margin: 6px 0;
}

.descDisplay{
    text-transform: capitalize;
}

.weatherEmoji{
    font-size: 90px;
    margin: 15px 0;
    transition: transform 0.5s ease;
}

.weatherEmoji:hover{
    transform: scale(1.2);
}

/* AGRICULTURA */
.farmingInfo, .moonDisplay{
    font-size: 24px;
    text-align: center;
    margin-top: 20px;
    background-color: rgba(255,255,255,0.35);
    padding: 20px;
    border-radius: 20px;
    color: #103457;
    line-height: 1.6;
    width: 100%;
    max-width: 450px;
    box-shadow: inset 0 2px 6px rgba(0,0,0,0.05);
}

/* ERRO */
.errorDisplay{
    font-size: 32px;
    margin-top: 0;
    color: #ff4d4f;
    font-weight: 600;
}

/altere e coloque emojis nas img
onst weatherForm = document.querySelector(".weatherForm");
const card = document.querySelector(".card");
const cityinput = document.querySelector(".cityInput")
const apiKey = "cd27754222de5aa2e56c5687c6c7d851";

weatherForm.addEventListener("submit", async event => {

    event.preventDefault();

    const city = cityinput.value;

    if(city){
        try{
            const weatherData = await getWeatherData(city);
            displaytWeatherInfo(weatherData);
        }
        catch(error){
            console.error(error);
            DisplayError(error);
        }
    }
    else{
        DisplayError("Please enter a city");
    }

});

async function getWeatherData(city){

    const api = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}`;

    const response = await fetch(api);

    if(!response.ok){
        throw new Error("Could not fetch data")
    }

    return await response.json();
}

async function displaytWeatherInfo(data){

    const {name: city,
           main: {temp, humidity},
           weather: [{description, id}]
          } = data;

    const cityDisplay = document.createElement("h1");
    const tempDisplay = document.createElement("p");
    const humidityDisplay = document.createElement("p");
    const descDisplay = document.createElement("p");
    const weatherEmoji = document.createElement("p");

    card.textContent = "";
    card.style.display = "flex";

    cityDisplay.textContent = city;
    tempDisplay.textContent = `${((temp - 273.15) * (9/5) + 32).toFixed(1)}F`;
    humidityDisplay.textContent = `Humidity: ${humidity}%`;
    descDisplay.textContent = description;

    cityDisplay.classList.add("cityDisplay");
    tempDisplay.classList.add("tempDisplay");
    humidityDisplay.classList.add("humidityDisplay");
    descDisplay.classList.add("descDisplay");
    weatherEmoji.classList.add("weatherEmoji");

    card.appendChild(cityDisplay);
    card.appendChild(tempDisplay);
    card.appendChild(humidityDisplay);
    card.appendChild(descDisplay);
    card.appendChild(weatherEmoji);

    const img = getWeatherEmoji(id);
    weatherEmoji.appendChild(img);

}

function getWeatherEmoji(id){

    const img = document.createElement("img");

    if(id === 800){
        img.src = "/IMGS/sun.jpg";
    }
    else if(id >= 200 && id < 300){
        img.src = "/IMGS/storm.jpg";
    }
    else if(id >= 300 && id < 600){
        img.src = "/IMGS/rain.jpg";
    }
    else if(id >= 600 && id < 700){
        img.src = "/IMGS/snow.jpg";
    }
    else{
        img.src = "/IMGS/weather 1.jpg";
    }

    return img;
}

function DisplayError(message){

    const errorDisplay = document.createElement("p");
    errorDisplay.classList.add("errorDisplay");
    errorDisplay.textContent = message;

    card.textContent = "";
    card.appendChild(errorDisplay);
    card.style.display = "flex";

}


//`${((temp - 273.15) * (9/5) + 32).toFixed(1)}F`;
//const apiKey = "cd27754222de5aa2e56c5687c6c7d851";
//`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}`;

const weatherForm = document.querySelector(".weatherForm");
const card = document.querySelector(".card");
const cityinput = document.querySelector(".cityInput");

const apiKey = "cd27754222de5aa2e56c5687c6c7d851";

/* EVENTO */

weatherForm.addEventListener("submit", async event => {

    event.preventDefault();

    const city = cityinput.value;

    if(city){

        try{

            const weatherData = await getWeatherData(city);

            displayWeatherInfo(weatherData);

        }
        catch(error){

            console.error(error);

            displayError(error.message);

        }

    }
    else{

        displayError("Digite uma cidade!");

    }

});

/* API */

async function getWeatherData(city){

    const api =
    `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&lang=pt_br`;

    const response = await fetch(api);

    if(!response.ok){

        throw new Error("Cidade não encontrada ❌");

    }

    return await response.json();

}

/* MOSTRAR CLIMA */

function displayWeatherInfo(data){

    const {
        name: city,
        main: {temp, humidity},
        weather: [{description, id}]
    } = data;

    card.textContent = "";

    card.style.display = "flex";

    /* ELEMENTOS */

    const cityDisplay = document.createElement("h1");

    const tempDisplay = document.createElement("p");

    const humidityDisplay = document.createElement("p");

    const descDisplay = document.createElement("p");

    const weatherEmoji = document.createElement("p");

    const farmingInfo = document.createElement("div");

    /* TEXTOS */

    cityDisplay.textContent = `📍 ${city}`;

    tempDisplay.textContent =
    `🌡️ ${((temp - 273.15)).toFixed(1)}°C`;

    humidityDisplay.textContent =
    `💧 Umidade: ${humidity}%`;

    descDisplay.textContent =
    `☁️ ${description}`;

    weatherEmoji.textContent =
    getWeatherEmoji(id);

    farmingInfo.innerHTML =
    getFarmingAdvice(temp, humidity, id);

    /* CLASSES */

    cityDisplay.classList.add("cityDisplay");

    tempDisplay.classList.add("tempDisplay");

    humidityDisplay.classList.add("humidityDisplay");

    descDisplay.classList.add("descDisplay");

    weatherEmoji.classList.add("weatherEmoji");

    farmingInfo.classList.add("farmingInfo");

    /* APPEND */

    card.appendChild(cityDisplay);

    card.appendChild(tempDisplay);

    card.appendChild(humidityDisplay);

    card.appendChild(descDisplay);

    card.appendChild(weatherEmoji);

    card.appendChild(farmingInfo);
  
    const moonDisplay = document.createElement("p");
    const phase = getMoonPhase(); // Calcula a fase atual
    moonDisplay.innerHTML = `🌙 ${phase}<br>${getMoonPlantingAdvice(phase)}`;
    moonDisplay.classList.add("moonDisplay");
    card.appendChild(moonDisplay);

}

/* EMOJIS */

function getWeatherEmoji(id){

    if(id === 800){

        return "☀️";

    }
    else if(id >= 200 && id < 300){

        return "⛈️";

    }
    else if(id >= 300 && id < 600){

        return "🌧️";

    }
    else if(id >= 600 && id < 700){

        return "❄️";

    }
    else if(id >= 700 && id < 800){

        return "🌫️";

    }
    else{

        return "☁️";

    }

}

/* AGRICULTURA */

function getFarmingAdvice(temp, humidity, id){

    const celsius = temp - 273.15;

    /* SOL */

    if(id === 800){

        return `
        🌞 <b>Excelente clima para agricultura!</b><br><br>

        🌽 Pode plantar:
        milho, tomate, feijão e soja.<br><br>

        💧 Irrigação moderada recomendada.
        `;

    }

    /* CHUVA */

    else if(id >= 300 && id < 600){

        return `
        🌧️ <b>Clima chuvoso e úmido.</b><br><br>

        🥬 Bom para:
        arroz, alface e cana-de-açúcar.<br><br>

        ⚠️ Evite excesso de água no solo.
        `;

    }

    /* TEMPESTADE */

    else if(id >= 200 && id < 300){

        return `
        ⛈️ <b>Tempestade prevista!</b><br><br>

        🚜 Evite plantar hoje.<br><br>

        ⚠️ Proteja plantações e equipamentos.
        `;

    }

    /* NEVE */

    else if(id >= 600 && id < 700){

        return `
        ❄️ <b>Frio intenso.</b><br><br>

        🥔 Pode favorecer batata e hortaliças.<br><br>

        ⚠️ Proteja as plantas do gelo.
        `;

    }

    /* NEBLINA */

    else if(id >= 700 && id < 800){

        return `
        🌫️ <b>Alta umidade.</b><br><br>

        🍄 Bom para culturas úmidas.<br><br>

        ⚠️ Atenção aos fungos.
        `;

    }

    /* NUBLADO */

    else{

        return `
        🌤️ <b>Clima moderado.</b><br><br>

        🌱 Bom para hortaliças e verduras.<br><br>

        💧 Irrigação leve recomendada.
        `;

    }

}

function getMoonPhase(date = new Date()) {
    // Cálculo aproximado da idade da lua
    const year = date.getFullYear();
    const month = date.getMonth() + 1; // Janeiro = 0
    const day = date.getDate();

    const c = e = jd = b = 0;

    let r = year % 100;
    r %= 19;
    r = ((r * 11) % 30) + month + day;
    if (month < 3) r += 2;
    r -= (year < 2000) ? 4 : 8.3;
    r = Math.floor(r + 0.5) % 30;

    const age = (r < 0) ? r + 30 : r;

    if (age === 0) return "🌑 Lua Nova";
    else if (age < 7) return "🌒 Crescente";
    else if (age === 7) return "🌓 Quarto Crescente";
    else if (age < 15) return "🌔 Crescente Gibosa";
    else if (age === 15) return "🌕 Lua Cheia";
    else if (age < 22) return "🌖 Minguante Gibosa";
    else if (age === 22) return "🌗 Quarto Minguante";
    else return "🌘 Minguante";
}

function getMoonPlantingAdvice(phase) {
    switch (phase) {
        case "🌑 Lua Nova":
            return "🌱 Boa para: alface, espinafre e verduras de folhas.";
        case "🌒 Crescente":
        case "🌓 Quarto Crescente":
        case "🌔 Crescente Gibosa":
            return "🌽 Boa para: milho, tomate, feijão e plantas frutíferas.";
        case "🌕 Lua Cheia":
            return "🍉 Boa para: frutas e plantas que dão frutos pesados.";
        case "🌖 Minguante Gibosa":
        case "🌗 Quarto Minguante":
        case "🌘 Minguante":
            return "🥔 Boa para: batata, cenoura, cebola e raízes.";
        default:
            return "🌿 Plantio moderado, sem recomendações específicas.";
    }
}

/* ERRO */

function displayError(message){

    card.textContent = "";

    card.style.display = "flex";

    const errorDisplay = document.createElement("p");

    errorDisplay.classList.add("errorDisplay");

    errorDisplay.textContent = `⚠️ ${message}`;

    card.appendChild(errorDisplay);

}
