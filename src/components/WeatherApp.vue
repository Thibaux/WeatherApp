<template>
    <main>
        <div class="search-box">
            <input type="text" class="search-bar" placeholder="Search for a city..." v-model="query"
                @keypress="fetchWeather" />
        </div>
        <div class="weather-wrap">
            <div class="404Error" v-if="httpcode == '404'">
                <p>
                    Sorry! {{ query }} is not found in the Open weather map database. <br>
                    Please change your query.
                </p>
            </div>
            <div class="500Error" v-if="httpcode == '500'">
                <p>
                    Sorry! There was an error with the Open weather map API. <br>
                    Pleas try again later.
                </p>
            </div>
            <div class="
                location-box-container" v-if="typeof weather.main != 'undefined' && httpcode == '200'">
                <div class="
                location-box">
                    <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
                    <div class="date">{{ dateBuilder() }}</div>
                </div>
                <div class="weather-box">
                    <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
                    <div class="weather">Feels like: {{ Math.round(weather.main.feels_like) }} °C</div>
                    <div class="min_max_temp">
                        <div class="weather min">Min: {{ Math.round(weather.main.temp_min) }} °C</div>
                        <div class="weather max">Max: {{ Math.round(weather.main.temp_max) }} °C</div>
                    </div>
                    <div class="wind_speed_sky">
                        <div class="weather wind_speed">Wind speed: {{ weather.wind.speed }} km/h</div>
                        <div class="weather sky">Sky: {{ weather.weather[0].description }} </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>
<script>
export default {
    name: "WeatherApp",
    data() {
        return {
            api_key: process.env.VUE_APP_API_KEY,
            api_base: "http://api.openweathermap.org/data/2.5/",
            query: "",
            weather: {},
            httpcode: ''
        };
    },
    methods: {
        fetchWeather(e) {
            if (e.key == "Enter") {
                fetch(
                        `${this.api_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
                    )
                    .then((res) => {
                        return res.json();
                    })
                    .then(this.setResults);
            }
        },
        setResults(results) {
            if (results.cod == 404) {
                this.httpcode = "404";
            } else if (results.cod == 500) {
                this.httpcode = "500";
            } else if (results.cod == 200) {
                this.httpcode = "200";
                console.log(this.httpcode)
                this.weather = results;
            }
        },
        dateBuilder() {
            let d = new Date();
            let months = [
                "January",
                "February",
                "March",
                "April",
                "May",
                "June",
                "July",
                "August",
                "September",
                "October",
                "November",
                "December",
            ];
            let days = [
                "Sunday",
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Friday",
                "Saturday",
            ];

            let day = days[d.getDay()];
            let date = d.getDate();
            let month = months[d.getMonth()];
            let year = d.getFullYear();

            return `${day} ${date} ${month} ${year}`;
        },
    },
};
</script>
<style>
main {
    display: grid;
    justify-content: center;
    height: 100vh;
    padding: 25px;
}

.search-box {
    width: 100%;
}

.search-box .search-bar {
    display: block;
    width: 90vw;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
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

.weather-wrap {
    display: grid;
    justify-content: center;
    align-content: center;
    margin-bottom: 5rem;
}

.weather-wrap p {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-box {
    display: grid;
    text-align: center;
}

.weather-box .temp {
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
    justify-self: center;
}

.weather-box .weather {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .min_max_temp {
    display: grid;
    grid-template-columns: 1fr 1fr;
}

.min_max_temp .max {
    padding-left: 3rem;
}

.weather-box>div:not(first-child) {
    padding-bottom: 0.5rem;
}
</style>