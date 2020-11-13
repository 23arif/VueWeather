<template>
    <div id="app"
         :class="typeof weather.main != 'undefined' && (weather.weather[0].main == 'Clear' ? 'clear' :'' || weather.weather[0].main == 'Clouds' ? 'cloudy' :'' || weather.weather[0].main == 'Drizzle' ? 'drizzle' :'' || weather.weather[0].main == 'Rain' ? 'rain' :'' || weather.weather[0].main == 'Mist' ? 'mist' :'' || weather.weather[0].main == 'Fog' ? 'mist' :'')">
        <main>
            <div class="search-box">
                <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
            </div>

            <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
                <div class="location-box">
                    <div class="location">{{weather.name}},{{weather.sys.country}}</div>
                    <div class="date">{{dateBuilder()}}</div>
                </div>
                <div class="weather-box">
                    <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
                    <div class="weather">{{weather.weather[0].main}}</div>
                </div>
            </div>
            <div class="not-found" v-if="typeof weather.main == 'undefined'">{{city_not_found}}</div>
        </main>
    </div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                api_key: process.env.VUE_APP_API_KEY,
                url_base: 'https://api.openweathermap.org/data/2.5/',
                query: '',
                city_not_found: '',
                weather: {}
            }
        },
        methods: {
            fetchWeather(e) {
                if (e.key == 'Enter') {
                    fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
                        .then(response => {
                            return response.json();
                        }).then(this.setResults)
                }
            },
            setResults(results) {
                this.weather = results;
                if(results.cod == '404'){
                    this.city_not_found ='City not found';
                    console.clear() // clear 404 error from console.
                }
            },
            dateBuilder() {
                let d = new Date();
                let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
                let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']

                let day = days[d.getDay()];
                let date = d.getDate();
                let month = months[d.getMonth()];
                let year = d.getFullYear();

                return `${day} ${date} ${month} ${year}`;
            }
        }
    }
</script>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        font-family: 'montserrat', sans-serif;
    }

    #app {
        background-image: url("./assets/cold-bg.jpg");
        background-size: cover;
        background-position: center;
        transition: 0.4s;
    }

    #app.warm {
        background-image: url("./assets/warm-bg.jpg");
    }

    #app.clear {
        background-image: url("./assets/clear-bg.jpg");
    }

    #app.cloudy {
        background-image: url("./assets/cloudy-bg.jpg");
    }

    #app.drizzle {
        background-image: url("./assets/drizzle-bg.jpg");
    }

    #app.rain {
        background-image: url("./assets/rain-bg.jpg");
    }

    #app.mist {
        background-image: url("./assets/mist-bg.jpg");
    }

    main {
        display: flex;
        justify-items: center;
        align-items: center;
        flex-direction: column;
        min-height: 100vh;
        padding: 25px;
        background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
    }

    .search-box {
        width: 100%;
        margin-bottom: 30px;
    }

    .search-box .search-bar {
        display: block;
        width: 100%;
        padding: 15px;
        color: #313131;
        font-size: 20px;
        appearance: none;
        border: none;
        outline: none;
        bacground: none;
        box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.5);
        border-radius: 0px 16px 0px 16px;
        transition: 0.4s;
    }

    .search-box .search-bar:focus {
        box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.75);
        border-radius: 16px 0px 16px 0px;
    }

    .location-box .location {
        color: #fff;
        font-size: 32px;
        font-weight: 500;
        text-align: center;
        text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
    }

    .location-box .date {
        color: #fff;
        font-size: 20px;
        font-weight: 300;
        font-style: italic;
        text-align: center;
    }

    .weather-box {
        text-align: center;
    }

    .weather-box .temp{
        display: inline-block;
        padding: 10px 25px;
        color: #fff;
        font-size: 102px;
        font-weight: 900;

        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.25);
        border-radius: 16px;
        margin: 30px 0px;
        box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }

    .weather-box .weather {
        color: #fff;
        font-size: 48px;
        font-weight: 700;
        font-style: italic;
        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);

    }

    .not-found{
        display: inline-block;
        text-align: center;
        font-size: 30px;
        padding: 10px 20px;
        color: #fff;
        font-weight: 900;
        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.25);
        border-radius: 16px;
        margin: 30px 0px;
        box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }

</style>
