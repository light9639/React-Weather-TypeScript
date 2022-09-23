# **:zap: React-Weather 템플릿**
<img src="https://user-images.githubusercontent.com/95972251/191897597-fe00b646-d2de-4556-b586-12642901ebb6.png" alt="weather" width="50%" />

**:sparkles: <a href="https://light9639.github.io/React-Weather/">React-Weather 템플릿</a> :sparkles:**

## **📋 작성법**
- 우선 리액트 프로젝트를 생성합니다.

```bash
npx install create-react-app
```

- openweathermap 홈페이지를 가입하고 current weather data API를 받아오자.

```bash
const api = {
	key: "",`
	base: "https://api.openweathermap.org/data/2.5/"
}
```

- api 변수를 지정해서 개인 고유의 key값과 url의 값을 지정해주고, functional 컴포넌트로 시작한다.

```bash
import React, { useState } from 'react';
const api = {
  key: "3301b70063f21cb2c74340e57c4ac48e",
  base: "https://api.openweathermap.org/data/2.5/"
}

function App() {
  const [query, setQuery] = useState('');
  const [weather, setWeather] = useState({});

  const search = evt => {
    if (evt.key === "Enter") {
      fetch(`${api.base}weather?q=${query}&units=metric&appid=${api.key}`)
        .then(res => res.json())
        .then(result => {
          setWeather(result);
          setQuery('');
          console.log(result);
        });
    }
  }

  const dateBuilder = (d) => {
    let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
    let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

    let day = days[d.getDay()];
    let date = d.getDate();
    let month = months[d.getMonth()];
    let year = d.getFullYear();

    return `${day} ${date} ${month} ${year}`
  }

  return (
    <div className={(typeof weather.main != "undefined") ? ((weather.main.temp > 16) ? 'app warm' : 'app') : 'app'}>
      <main>
        <div className="search-box">
          <input
            type="text"
            className="search-bar"
            placeholder="Search..."
            onChange={e => setQuery(e.target.value)}
            value={query}
            onKeyPress={search}
          />
        </div>
        {(typeof weather.main != "undefined") ? (
          <div>
            <div className="location-box">
              <div className="location">{weather.name}, {weather.sys.country}</div>
              <div className="date">{dateBuilder(new Date())}</div>
            </div>
            <div className="weather-box">
              <div className="temp">
                {Math.round(weather.main.temp)}°c
              </div>
              <div className="weather">{weather.weather[0].main}</div>
            </div>
          </div>
        ) : ('')}
      </main>
    </div>
  );
}

export default App;
```

- Css 추가
```bash
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

.app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: center;
  transition: 0.4 ease;
}

.app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.75));
  padding: 25px;
}

.search-box {
  width: 100%;
  margin: 0 0 75px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  appearance: none;
  background: none;
  border: none;
  outline: none;

  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 5px;
  margin-top: 0px;

  box-shadow: 0px 5px rgba(0, 0, 0, 0.2);

  color: #313131;
  font-size: 20px;

  transition: 0.4s ease;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 2px 2px rgba(50, 50, 70, 0.5);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  position: relative;
  display: inline-block;
  margin: 30px auto;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 16px;

  padding: 15px 25px;

  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(50, 50, 70, 0.5);
  text-align: center;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.2);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
}
```

- 완성되면 검색창 화면에 국가나 도시 이름을 입력하면 정보가 출력된다.

## **:paperclip: 출처**
- 출처1 : <a href="https://velog.io/@hongcoder/React%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Weather-App-%EB%A7%8C%EB%93%A4%EA%B8%B0">velog 링크</a>
- 출처2 : <a href="https://www.youtube.com/watch?v=GuA0_Z1llYU">youtube 링크</a>
