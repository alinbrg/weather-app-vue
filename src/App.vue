<template>
	<div
		id="app-block"
		:class="
			typeof weather.main != 'undefined' &&
			((weatherInWord == 'Rain' ? 'rain' : '') ||
				(weatherInWord == 'Clouds' ||
				(weather.main.temp > 10 && weather.main.temp < 30)
					? 'clouds'
					: '') ||
				(weather.main.temp > 30 ? 'warm' : ''))
		"
	>
		<main>
			<SearchBoxComp @search="searchKeyWord" />
			<div class="weather-wrap" v-if="typeof weather.main == 'undefined'">
				<div class="location-box">
					<div class="location">
						{{ weather.message }}
					</div>
				</div>
			</div>
			<div class="weather-wrap" v-if="typeof weather.main == 'object'">
				<div class="location-box">
					<div class="location">
						{{ weather.name }},
						{{ weather.sys.country }}
					</div>

					<div class="date">{{ showDate() }}</div>

					<div class="weather-box">
						<div class="temp">{{ Math.round(weather.main.temp) }}℃</div>
						<div class="weather">{{ weatherInWord }}</div>
					</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
import SearchBoxComp from "./components/SearchBoxComp";
export default {
	name: "App",
	components: { SearchBoxComp },
	data() {
		return {
			api_key: "3114cd1adc30e7375e4cdacbf06b6c99",
			url_base: "https://api.openweathermap.org/data/2.5/",
			q: "",
			weather: {},
			weatherInWord: "",
		};
	},
	methods: {
		searchKeyWord(q, e) {
			if (e.key === "Enter") {
				fetch(
					`${this.url_base}weather?q=${q}&units=metric&APPID=${this.api_key}`
				)
					.then((res) => {
						return res.json();
					})
					.then(this.setResults)
					.catch((error) => {
						console.error("Error:", error);
					});
			}
		},
		setResults(data) {
			this.weather = data;
			this.weatherInWord = this.weather.weather[0].main;
		},
		showDate() {
			let d = new Date(),
				months = [
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
				],
				days = [
					"Sunday",
					"Monday",
					"Tuesday",
					"Wednesday",
					"Thursday",
					"Friday",
					"Saturday",
				];

			let day = days[d.getDay()],
				date = d.getDate(),
				month = months[d.getMonth()],
				year = d.getFullYear();

			return `${day}, ${date} ${month}, ${year}`;
		},
	},
};
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: "Courier New", Courier, monospace;
	background: linear-gradient(
		to bottom,
		rgba(0, 0, 0, 0.25),
		rgba(0, 0, 0, 0.25)
	);
}
#app-block.warm {
	background-image: url("./assets/sunny.jpg");
}

#app-block.clouds {
	background-image: url("./assets/mountains-cloud.jpg");
}
#app-block.rain {
	background-image: url("./assets/thunder.jpg");
}
#app-block {
	background-image: url("./assets/mountains.jpg");
	background-size: cover;
	background-position: bottom;
	transition: all 0.4s;
	max-width: 50vw;
	margin: 0 auto;
}

main {
	min-height: 100vh;
	padding: 25px;
	background-image: linear-gradient(
		to bottom,
		rgba(0, 0, 0, 0.25),
		rgba(0, 0, 0, 0.75)
	);
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
	background: none;
	background-color: rgba(255, 255, 255, 0.55);
	border-radius: 16px 0 16px 0;
	box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
	transition: all 0.4s;
}
.search-box .search-bar:focus {
	box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
	background-color: rgba(255, 255, 255, 0.85);
	border-radius: 0px 16px 0 16px;
}

.location-box .location {
	color: white;
	font-size: 32px;
	font-weight: 500;
	text-align: center;
	text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
	color: white;
	font-size: 20px;
	font-weight: 300;
	text-align: center;
	font-style: italic;
}
.weather-box {
	text-align: center;
}
.weather-box .temp {
	display: inline-block;
	padding: 10px 25px;
	color: white;
	font-size: 100px;
	font-weight: 900;
	text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	background-color: rgba(255, 255, 255, 0.25);
	border-radius: 16px;
	margin: 30px 0;
	box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
	font-size: 48px;
	color: white;
	font-weight: 700;
	font-style: italic;
	text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
