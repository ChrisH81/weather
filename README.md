# Weather API

Third-party APIs allow developers to access their data and functionality by making requests with specific parameters to a URL. Developers are often tasked with retrieving data from another application's API and using it in the context of their own. Your challenge is to build a weather dashboard that will run in the browser and feature dynamically updated HTML and CSS.

## URL
[Live URL](https://chrish81.github.io/weather/.)

## Screen Shot

![Screen shot image](https://i.imgur.com/EFRh10Y.jpg)


## Code

AS A traveler
I WANT to see the weather outlook for multiple cities
SO THAT I can plan a trip accordingly
GIVEN a weather dashboard with form inputs
WHEN I search for a city
THEN I am presented with current and future conditions for that city and that city is added to the search history
```
  <link
  rel="stylesheet"
  href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
  />
  <link
  rel="stylesheet"
  href="https://use.fontawesome.com/releases/v5.8.1/css/all.css"
  integrity="sha384-50oBUHEmvpQ+1lW4y57PTFmhCaXp0ML5d60M1M7uH2+nqUivzIebhndOJK28anvf"
  crossorigin="anonymous"
  />
  <link
  href="https://fonts.googleapis.com/css?family=Open+Sans&display=swap"
  rel="stylesheet"
  />
  <link rel="stylesheet" href="style.css" />
  <title>Weather Dashboard</title>

  <header>
    <h1 class="display-4">Weather Dashboard</h1>
  </header>

  <div id="left-sidebar">
    <div id="search-title">Search for a city</div>
    <!-- Search form -->
    <div class="input-group md-form form-sm form-2 pl-0">
      <input id="city-search" class="form-control my-0 py-1 lime-border" type="text" placeholder="Search" aria-label="Search">
      <div class="input-group-append">
        <span class="input-group-text lime lighten-2" id="basic-text1">
          <i onclick="doSearch()" class="fas fa-search text-grey" aria-hidden="true"></i>

```
WHEN I view current weather conditions for that city
THEN I am presented with the city name, the date, an icon representation of weather conditions, the temperature, the humidity, the wind speed, and the UV index
```
  <div id="search-history">
    </div>
  </div>
  <div id ="main-content">
    <div id="temperature-info">
      <div id="city-date"></div><br/>      
      <div id="temp"></div><br/>
      <div id="humid"></div><br/>
      <div id="wind-speed"></div><br/>
      <div id="uv-index"></div><br/>
    </div>
     <div id="forecast-5day">
      <h2>5 day forecast</h2>
      <div id="forecast">
      </div>
    </div>
  </div>

```
WHEN I view the UV index
THEN I am presented with a color that indicates whether the conditions are favorable, moderate, or severe
```
    const setUV = async (lat, lon) => {
       const response = await fetch(`http://api.openweathermap.org/data/2.5/uvi?appid=${API_KEY}&lat=${lat}&lon=${lon}`);
        results = await response.json(); //extract JSON from the http response
        if (results.value > 9) {
          document.getElementById('uv-index').style.backgroundColor = "red";
        }
        else if (results.value > 3) {
          document.getElementById('uv-index').style.backgroundColor = "orange";
        }
        else {
          document.getElementById('uv-index').style.backgroundColor = "green";
        }
        document.getElementById('uv-index').innerHTML = "UV index: " + results.value;
    }

```

WHEN I view future weather conditions for that city
THEN I am presented with a 5-day forecast that displays the date, an icon representation of weather conditions, the temperature, and the humidity
```
   function setFiveDayForecast(results) {
      const foreCast = document.getElementById('forecast');
      foreCast.innerHTML="";
      let currentDay = new Date();
      for (let i =1; i <6; i ++) {
        currentDay.setDate(new Date(currentDay).getDate()+1);
        const dateTimeFormat = new Intl.DateTimeFormat('en', { year: 'numeric', month: 'short', day: '2-digit' }) 
        const [{ value: month },,{ value: day },,{ value: year }] = dateTimeFormat.formatToParts(currentDay);

        const tempDiv = document.createElement("div");
        const iconCode = results.list[i].weather[0].icon;
tempDiv.innerHTML = `${month}-${day}-${year}`
 + "<br/>" + `<span><img src="http://openweathermap.org/img/wn/${iconCode}@2x.png"/>
</span>` + "Temperature: " + kelvinToFarenheit(results.list[i].main.temp) + ' &deg;F' + "<br/>" +  "Humidity: " + results.list[i].main.humidity + " %";;

        document.getElementById('forecast').append(tempDiv);

```

WHEN I click on a city in the search history
THEN I am again presented with current and future conditions for that city
```
function populateSearchHistory() {
      const searchHistory = document.getElementById("search-history");
      searchHistory.innerHTML = "";
      for (let i = 0;  i < localStorage.length; i++ ) {
        const button = document.createElement('button');
        button.innerHTML = localStorage.key(i);
        button.onclick= () => { doSearch(localStorage.key(i)) };
        searchHistory.append(button);
      }
    }
    populateSearchHistory();
```

## Contributing
Pull requests are welcome. 