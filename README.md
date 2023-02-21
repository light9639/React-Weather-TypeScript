# ☀️ React-Weather-TypeScript 템플릿입니다.
:octocat: 바로가기 : https://light9639.github.io/React-Weather-TypeScript/ <br /><br />

<img src="https://assets.zabbix.com/img/brands/openweather.jpg" alt="weather" width="500px" />

**:sparkles: ☀️ React-Weather-TypeScript 템플릿입니다. :sparkles:**
## :tada: React 프로젝트 생성
- React 생성
```bash
npm create-react-app my-app
# or
yarn create react-app my-app
```

- vite를 이용하여 프로젝트를 생성하려면
```bash
npm create vite@latest
# or
yarn create vite
```
- 터미널에서 실행 후 프로젝트 이름 만든 후 React 선택, Typescirpt-SWC 선택하면 생성 완료.
## ⚙️ 'openweathermap' 사이트에서 API 발급받기
- `openweathermap` 홈페이지를 가입하고 `current weather data API`를 받아온다.
- 발급받은 `key` 값을 `App.tsx` 상단에 밑의 코드처럼 넣으면 된다.
```bash
const api = {
   key: {발급받은 키 값},`
   base: "https://api.openweathermap.org/data/2.5/"
}
```
## ✒️ index.html, App.tsx 수정 및 작성
### ⚡ App.tsx
- `api` 변수를 지정해서 개인 고유의 `key`값과 `url`의 값을 지정해준다.
- `useEffect` 안에 `fetch` 함수를 사용하여 로드 시 서울이 나오도록 설정함.
- `search` 함수를 사용하여 도시나 국가명을 영어로 검색했을 한 뒤 `Enter`를 누르면 작동하도록 설정함.
```typescript
import React, { useEffect, useState, KeyboardEvent } from 'react';

// api 가져와서 사용하기.
const api = {
  key: "3301b70063f21cb2c74340e57c4ac48e",
  base: "https://api.openweathermap.org/data/2.5/"
}

export default function App(): JSX.Element {
  const [query, setQuery] = useState<string>('');
  const [weather, setWeather] = useState<any>({});

  // 로드 시 서울이 나오도록 설정함.
  useEffect(() => {
    fetch(`${api.base}weather?q=Seoul&units=metric&appid=${api.key}`)
      .then(res => res.json())
      .then(result => {
        setWeather(result);
        setQuery('');
        console.log(result);
      });
  }, []);

  // onClick시 작동하도록 설정함.
  const search = (evt: KeyboardEvent<HTMLInputElement>) => {
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

  // 날짜를 가져오는 함수 모음
  const dateBuilder = (d: Date) => {
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
```
### ⚡ App.css
- `App.tsx`의 `CSS`를 추가하여 스타일링하기
```css
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

## 💾 완성 화면

- 완성된 후 검색창에 국가나 도시 이름을 입력하면`(ex: Seoul, London, Paris)` 정보가 출력됩니다.

 | <img align="center" src="https://user-images.githubusercontent.com/95972251/191903631-35c36bb9-38da-406c-b9a0-154d1670a8e0.png" alt="warm" /> | <img align="center" src="https://user-images.githubusercontent.com/95972251/191903639-ab5138e9-1e6c-482a-adda-72d6b8742d7e.png" alt="cool" /> |
 | ------------- | ------------- |

## :paperclip: 출처
- 출처1 : <a href="https://velog.io/@hongcoder/React%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Weather-App-%EB%A7%8C%EB%93%A4%EA%B8%B0">velog 링크</a>
- 출처2 : <a href="https://www.youtube.com/watch?v=GuA0_Z1llYU">youtube 링크</a>
