---
publish: true
tags: [지식, 프론트엔드]
---

# React 개요

---

## React란?

> **React**는 페이스북(현재 Meta)에서 개발한 사용자 인터페이스(UI)를 구축하기 위한 JavaScript 라이브러리입니다. 컴포넌트 기반 아키텍처와 가상 DOM을 활용하여 효율적이고 유지보수 가능한 웹 애플리케이션을 만들 수 있도록 도와줍니다. 단일 페이지 애플리케이션(SPA) 개발에 널리 사용되며, 가장 인기 있는 프론트엔드 라이브러리입니다.

---

## 목차

1. [React의 역사](#react의-역사)
2. [장단점](#장단점)
3. [폴더 구조](#폴더-구조)
4. [기본적으로 알아야 할 것](#기본적으로-알아야-할-것)

---

## React의 역사

### 1. 초기 개발 (2011-2013)
* **2011년**: Facebook의 Jordan Walke가 React 개발 시작
* **배경**: Facebook의 뉴스피드 성능 개선 필요
* **2013년 5월**: React가 오픈소스로 공개
* **초기 반응**: JSX 문법에 대한 논란, 학습 곡선 우려

### 2. React 0.x 시대 (2013-2015)
* **특징**: 
  * 컴포넌트 기반 아키텍처
  * 가상 DOM 개념 도입
  * 단방향 데이터 흐름
* **주요 개념**: Props, State, 생명주기 메서드
* **문제점**: 복잡한 설정, 빌드 도구 필요

### 3. React 15 (2016년)
* **2016년 4월**: React 15.0 릴리즈
* **주요 개선**:
  * 새로운 재조정 알고리즘
  * 에러 경계 (Error Boundaries) 개념 도입
  * 불필요한 `<div>` 래퍼 제거 가능
* **성능**: 이전 버전 대비 50% 이상 성능 향상

### 4. React 16 (2017년) - "Fiber"
* **2017년 9월**: React 16.0 릴리즈
* **주요 변화**:
  * **Fiber 아키텍처**: 새로운 재조정 알고리즘
  * **Fragment**: 불필요한 DOM 요소 제거
  * **Portal**: DOM 트리 외부 렌더링
  * **Error Boundary**: 컴포넌트 트리의 에러 처리
  * **서버 사이드 렌더링**: 완전한 SSR 지원
* **성능**: 큰 리스트 렌더링 성능 대폭 개선

### 5. React Hooks (2019년)
* **2019년 2월**: React 16.8 릴리즈
* **혁신적 변화**:
  * **Hooks 도입**: 함수형 컴포넌트에서 상태 관리 가능
  * `useState`, `useEffect` 등 주요 Hooks 제공
  * 클래스 컴포넌트 대신 함수형 컴포넌트 사용 권장
* **영향**: React 개발 패러다임 전환

### 6. React 17 (2020년)
* **2020년 10월**: React 17.0 릴리즈
* **특징**: "No New Features"
* **주요 변경**:
  * 새로운 JSX Transform
  * 이벤트 위임 방식 변경
  * 점진적 업그레이드 지원
* **목적**: 미래 버전을 위한 기반 마련

### 7. React 18 (2022년)
* **2022년 3월**: React 18.0 릴리즈
* **주요 기능**:
  * **동시성 (Concurrency)**: 우선순위 기반 렌더링
  * **자동 배칭**: 상태 업데이트 자동 그룹화
  * **Suspense 개선**: 서버 컴포넌트 지원 준비
  * **Transitions**: 긴급하지 않은 업데이트 지연
  * **Hydration**: 서버 렌더링된 콘텐츠 개선
* **성능**: 더 나은 사용자 경험 제공

### 8. React 19 (2024년)
* **예상 기능**:
  * React Compiler (자동 최적화)
  * 서버 컴포넌트 정식 지원
  * Actions (폼 처리 개선)
  * 더 나은 Hydration

### 생태계 발전
* **2015년**: Redux 등장 (상태 관리)
* **2016년**: Create React App (CRA) 출시
* **2017년**: Next.js 인기 상승 (SSR 프레임워크)
* **2018년**: React Native 모바일 개발 확산
* **2020년**: Vite, Remix 등 새로운 도구 등장

---

## 장단점

### 장점

#### 1. 컴포넌트 기반 아키텍처
* **재사용성**: 컴포넌트를 여러 곳에서 재사용 가능
* **모듈화**: 작은 단위로 코드 분리 가능
* **유지보수성**: 컴포넌트별로 독립적 수정 가능
* **테스트 용이**: 컴포넌트 단위 테스트 쉬움

#### 2. 가상 DOM (Virtual DOM)
* **성능 최적화**: 실제 DOM 조작 최소화
* **효율적 업데이트**: 변경된 부분만 업데이트
* **크로스 브라우저 호환**: 브라우저 차이 추상화
* **예측 가능한 렌더링**: 선언적 UI 작성

#### 3. 단방향 데이터 흐름
* **예측 가능성**: 데이터 흐름이 명확함
* **디버깅 용이**: 문제 추적이 쉬움
* **데이터 일관성**: 단일 소스의 진실 (Single Source of Truth)
* **버그 예방**: 데이터 흐름이 일정하여 실수 감소

#### 4. 풍부한 생태계
* **거대한 커뮤니티**: 가장 큰 프론트엔드 커뮤니티
* **풍부한 라이브러리**: 거의 모든 기능에 대한 라이브러리 존재
* **지속적인 업데이트**: 활발한 개발 및 유지보수
* **학습 자료**: 튜토리얼, 블로그, 강의 풍부

#### 5. 유연성
* **프레임워크가 아님**: 필요한 부분만 선택적 사용
* **다양한 통합**: 다른 라이브러리와 쉽게 통합
* **점진적 도입**: 기존 프로젝트에 점진적으로 도입 가능
* **자유로운 아키텍처**: 프로젝트 구조 자유롭게 설계

#### 6. 개발 도구
* **React DevTools**: 강력한 디버깅 도구
* **빠른 개발 환경**: Hot Reload 지원
* **TypeScript 지원**: 타입 안정성 제공
* **다양한 빌드 도구**: Webpack, Vite, Parcel 등

#### 7. 직관적인 문법 (JSX)
* **HTML과 유사**: 기존 웹 개발자에게 친숙
* **표현력**: JavaScript 표현식 사용 가능
* **가독성**: 컴포넌트 구조가 명확
* **IDE 지원**: 자동완성, 문법 강조 등

#### 8. Meta의 지속적 지원
* **엔터프라이즈급 지원**: 대기업에서 실제 사용
* **장기적 지원**: 지속적인 개발 및 개선
* **안정성**: 프로덕션 환경에서 검증됨

### 단점

#### 1. 학습 곡선
* **JSX 문법**: 초보자에게 생소할 수 있음
* **개념 이해**: 컴포넌트, Props, State 개념 필요
* **Hooks**: 함수형 컴포넌트와 Hooks 학습 필요
* **상태 관리**: 복잡한 상태 관리 패턴 이해 필요

#### 2. 빠른 변화
* **업데이트 빈도**: 새로운 기능과 패턴이 자주 등장
* **베스트 프랙티스 변화**: 권장사항이 자주 변경
* **라이브러리 호환성**: 버전 업데이트 시 호환성 문제
* **유지보수 부담**: 최신 패턴을 따라가기 어려움

#### 3. 프레임워크가 아님
* **설정 필요**: 라우팅, 상태 관리, 빌드 도구 직접 선택
* **의사결정 부담**: 많은 선택지 중 결정 필요
* **보일러플레이트**: 초기 설정에 시간 소요
* **일관성 부족**: 프로젝트마다 구조가 다를 수 있음

#### 4. SEO (검색 엔진 최적화)
* **클라이언트 렌더링**: 기본적으로 CSR (Client-Side Rendering)
* **초기 로딩**: JavaScript 번들 다운로드 필요
* **SSR 필요**: SEO가 중요한 경우 Next.js 등 필요
* **추가 설정**: SSR 구현을 위한 추가 작업

#### 5. 번들 크기
* **라이브러리 크기**: React 자체가 비교적 큼
* **의존성**: 많은 라이브러리 사용 시 번들 크기 증가
* **최적화 필요**: Code Splitting, Tree Shaking 등 필요
* **초기 로딩 시간**: 큰 번들은 느린 초기 로딩

#### 6. 복잡한 상태 관리
* **전역 상태**: Redux, Zustand, Jotai 등 필요
* **상태 동기화**: 여러 컴포넌트 간 상태 공유 복잡
* **상태 업데이트**: 불변성 유지 필요
* **성능 최적화**: 메모이제이션, 최적화 전략 필요

#### 7. 과도한 리렌더링
* **불필요한 렌더링**: 최적화 없이는 자주 리렌더링
* **성능 이슈**: 큰 리스트나 복잡한 컴포넌트에서 성능 저하
* **최적화 필요**: React.memo, useMemo, useCallback 등 필요
* **디버깅 어려움**: 리렌더링 원인 파악 어려울 수 있음

#### 8. 문서화
* **공식 문서**: 좋지만 초보자에게는 복잡할 수 있음
* **버전별 차이**: 문서가 버전별로 달라 혼란 가능
* **최신 정보**: 커뮤니티 자료가 빠르게 오래될 수 있음

---

## 폴더 구조

### 기본 React 프로젝트 구조 (Create React App)

```
my-react-app/
├── public/                        # 정적 파일
│   ├── index.html                 # HTML 템플릿
│   ├── favicon.ico
│   └── manifest.json
├── src/                           # 소스 코드
│   ├── components/                # 재사용 가능한 컴포넌트
│   │   ├── Button/
│   │   │   ├── Button.jsx
│   │   │   ├── Button.module.css
│   │   │   └── index.js
│   │   ├── Card/
│   │   └── Modal/
│   ├── pages/                     # 페이지 컴포넌트
│   │   ├── Home/
│   │   ├── About/
│   │   └── Contact/
│   ├── hooks/                     # 커스텀 Hooks
│   │   ├── useAuth.js
│   │   └── useFetch.js
│   ├── context/                   # Context API
│   │   └── AuthContext.js
│   ├── services/                  # API 서비스
│   │   └── api.js
│   ├── utils/                     # 유틸리티 함수
│   │   └── helpers.js
│   ├── assets/                    # 이미지, 폰트 등
│   │   ├── images/
│   │   └── fonts/
│   ├── styles/                    # 전역 스타일
│   │   ├── global.css
│   │   └── variables.css
│   ├── App.jsx                    # 루트 컴포넌트
│   ├── App.css
│   ├── index.js                   # 진입점
│   └── index.css                  # 전역 CSS
├── package.json
├── README.md
└── .gitignore
```

### 기능 기반 폴더 구조 (Feature-based)

```
src/
├── features/                      # 기능별 폴더
│   ├── auth/
│   │   ├── components/
│   │   │   ├── LoginForm.jsx
│   │   │   └── SignupForm.jsx
│   │   ├── hooks/
│   │   │   └── useAuth.js
│   │   ├── services/
│   │   │   └── authService.js
│   │   ├── context/
│   │   │   └── AuthContext.js
│   │   └── pages/
│   │       ├── Login.jsx
│   │       └── Signup.jsx
│   ├── users/
│   │   ├── components/
│   │   │   ├── UserList.jsx
│   │   │   └── UserCard.jsx
│   │   ├── hooks/
│   │   │   └── useUsers.js
│   │   ├── services/
│   │   │   └── userService.js
│   │   └── pages/
│   │       └── Users.jsx
│   └── products/
│       ├── components/
│       ├── hooks/
│       └── services/
├── shared/                        # 공유 컴포넌트/유틸
│   ├── components/
│   │   ├── Button/
│   │   ├── Input/
│   │   └── Modal/
│   ├── hooks/
│   └── utils/
├── layouts/                       # 레이아웃 컴포넌트
│   ├── MainLayout.jsx
│   └── AuthLayout.jsx
├── routes/                        # 라우팅 설정
│   └── index.js
├── store/                         # 상태 관리 (Redux 등)
│   ├── slices/
│   ├── store.js
│   └── hooks.js
└── App.jsx
```

### Atomic Design 패턴

```
src/
├── components/
│   ├── atoms/                     # 가장 작은 단위
│   │   ├── Button/
│   │   ├── Input/
│   │   └── Label/
│   ├── molecules/                 # atoms 조합
│   │   ├── SearchBox/
│   │   └── FormField/
│   ├── organisms/                 # molecules 조합
│   │   ├── Header/
│   │   └── ProductList/
│   ├── templates/                 # 레이아웃 구조
│   │   └── PageTemplate/
│   └── pages/                     # 완성된 페이지
│       ├── HomePage/
│       └── AboutPage/
```

### Next.js 프로젝트 구조

```
my-next-app/
├── pages/                         # 라우팅 (파일 기반)
│   ├── index.js                   # / (홈)
│   ├── about.js                   # /about
│   ├── users/
│   │   ├── index.js               # /users
│   │   └── [id].js                # /users/:id
│   └── api/                       # API 라우트
│       └── users.js
├── components/                    # 컴포넌트
│   ├── layout/
│   └── ui/
├── public/                        # 정적 파일
├── styles/                        # 스타일
├── lib/                           # 유틸리티
├── hooks/                         # 커스텀 Hooks
└── next.config.js                 # Next.js 설정
```

### 컴포넌트 폴더 구조 (옵션들)

#### 옵션 1: 단일 파일
```
components/
├── Button.jsx
├── Button.css
└── Button.test.js
```

#### 옵션 2: 폴더 구조 (권장)
```
components/
├── Button/
│   ├── Button.jsx
│   ├── Button.module.css
│   ├── Button.test.js
│   └── index.js                   # export default Button
```

#### 옵션 3: 세분화된 구조
```
components/
├── Button/
│   ├── Button.jsx
│   ├── Button.module.css
│   ├── Button.test.js
│   ├── Button.stories.js          # Storybook
│   └── index.js
```

### 폴더 구조 베스트 프랙티스

#### 1. 컴포넌트는 폴더로 관리
* 각 컴포넌트는 독립적인 폴더
* 관련 파일(스타일, 테스트) 함께 관리

#### 2. 기능 기반 구조 권장
* 기능별로 코드 그룹화
* 관련된 컴포넌트, Hook, 서비스 함께 위치

#### 3. 공유 컴포넌트 분리
* 여러 곳에서 사용하는 컴포넌트는 `shared/` 또는 `components/`
* 기능별 컴포넌트는 각 기능 폴더 내부

#### 4. 절대 경로 사용
* `import Button from '@/components/Button'`
* `jsconfig.json` 또는 `tsconfig.json`에 설정

```json
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "@/*": ["*"]
    }
  }
}
```

---

## 기본적으로 알아야 할 것

### 1. 핵심 개념

#### 컴포넌트 (Component)
* **역할**: UI를 구성하는 재사용 가능한 단위
* **종류**: 함수형 컴포넌트, 클래스 컴포넌트 (구식)
* **특징**: Props를 받아서 UI를 반환

```jsx
// 함수형 컴포넌트 (권장)
function Welcome({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// 화살표 함수
const Welcome = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};
```

#### JSX (JavaScript XML)
* **역할**: JavaScript에서 HTML과 유사한 문법 사용
* **특징**: JavaScript 표현식 사용 가능, 반드시 하나의 루트 요소

```jsx
const element = <h1>Hello, World!</h1>;

// 표현식 사용
const name = "React";
const element = <h1>Hello, {name}!</h1>;

// 조건부 렌더링
{isLoggedIn ? <Dashboard /> : <Login />}

// 리스트 렌더링
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

#### Props (Properties)
* **역할**: 부모 컴포넌트에서 자식 컴포넌트로 데이터 전달
* **특징**: 읽기 전용, 불변성 유지

```jsx
// Props 전달
<Button label="Click me" onClick={handleClick} />

// Props 받기
function Button({ label, onClick }) {
  return <button onClick={onClick}>{label}</button>;
}
```

#### State (상태)
* **역할**: 컴포넌트 내부의 변경 가능한 데이터
* **특징**: State 변경 시 컴포넌트 리렌더링
* **Hooks**: `useState` 사용

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### 2. 주요 Hooks

#### useState
* **역할**: 컴포넌트에 상태 추가
* **반환값**: [현재 상태, 상태 업데이트 함수]

```jsx
const [state, setState] = useState(초기값);

// 예시
const [name, setName] = useState('');
const [count, setCount] = useState(0);
const [user, setUser] = useState({ name: '', age: 0 });

// 함수형 업데이트
setCount(prevCount => prevCount + 1);
```

#### useEffect
* **역할**: 사이드 이펙트 처리 (API 호출, 구독, DOM 조작)
* **의존성 배열**: 언제 실행할지 제어

```jsx
import { useEffect } from 'react';

// 컴포넌트 마운트 시 한 번 실행
useEffect(() => {
  console.log('Component mounted');
}, []);

// 특정 값 변경 시 실행
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);

// 클린업 함수
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Tick');
  }, 1000);
  
  return () => {
    clearInterval(timer);  // 컴포넌트 언마운트 시 정리
  };
}, []);
```

#### useContext
* **역할**: Context 값을 구독
* **사용**: 여러 컴포넌트에서 전역 상태 공유

```jsx
import { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Button />
    </ThemeContext.Provider>
  );
}

function Button() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Button</button>;
}
```

#### useReducer
* **역할**: 복잡한 상태 로직 관리
* **사용**: 여러 하위 값을 가진 복잡한 상태

```jsx
import { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

#### useMemo
* **역할**: 계산 결과 메모이제이션
* **사용**: 비용이 큰 계산 최적화

```jsx
import { useMemo } from 'react';

function ExpensiveComponent({ items }) {
  const expensiveValue = useMemo(() => {
    return items.reduce((sum, item) => sum + item.value, 0);
  }, [items]);
  
  return <div>{expensiveValue}</div>;
}
```

#### useCallback
* **역할**: 함수 메모이제이션
* **사용**: 자식 컴포넌트에 전달하는 함수 최적화

```jsx
import { useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);
  
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []);  // 의존성이 없으면 항상 같은 함수 참조
  
  return <Child onClick={handleClick} />;
}
```

### 3. 이벤트 처리

```jsx
function Button() {
  const handleClick = (e) => {
    e.preventDefault();
    console.log('Button clicked');
  };
  
  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}

// 인라인 함수
<button onClick={() => console.log('Clicked')}>
  Click
</button>

// 매개변수 전달
<button onClick={(e) => handleClick(id, e)}>
  Click
</button>
```

### 4. 조건부 렌더링

```jsx
// 삼항 연산자
{isLoggedIn ? <Dashboard /> : <Login />}

// 논리 연산자 (&&)
{isLoading && <Spinner />}

// if 문 (조건부로 반환)
function Greeting({ isLoggedIn }) {
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

// 변수 사용
function Mailbox({ unreadMessages }) {
  const unreadCount = unreadMessages.length;
  return (
    <div>
      {unreadCount > 0 && (
        <h2>You have {unreadCount} unread messages.</h2>
      )}
    </div>
  );
}
```

### 5. 리스트 렌더링

```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}

// key의 중요성
// ❌ 잘못된 예 (인덱스 사용)
{todos.map((todo, index) => (
  <li key={index}>{todo.text}</li>
))}

// ✅ 올바른 예 (고유 ID 사용)
{todos.map(todo => (
  <li key={todo.id}>{todo.text}</li>
))}
```

### 6. 폼 처리

```jsx
import { useState } from 'react';

function Form() {
  const [formData, setFormData] = useState({
    name: '',
    email: ''
  });
  
  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(formData);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### 7. HTTP 요청 (Fetch API)

```jsx
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetch('/api/users')
      .then(response => response.json())
      .then(data => {
        setUsers(data);
        setLoading(false);
      })
      .catch(err => {
        setError(err);
        setLoading(false);
      });
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### 8. 커스텀 Hooks

```jsx
// useFetch.js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetch(url)
      .then(response => response.json())
      .then(data => {
        setData(data);
        setLoading(false);
      })
      .catch(err => {
        setError(err);
        setLoading(false);
      });
  }, [url]);
  
  return { data, loading, error };
}

// 사용
function UserList() {
  const { data, loading, error } = useFetch('/api/users');
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error</div>;
  
  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### 9. Context API (전역 상태 관리)

```jsx
import { createContext, useContext, useState } from 'react';

const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  
  const login = (userData) => {
    setUser(userData);
  };
  
  const logout = () => {
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}

// 사용
function App() {
  return (
    <AuthProvider>
      <Dashboard />
    </AuthProvider>
  );
}

function Dashboard() {
  const { user, logout } = useAuth();
  
  return (
    <div>
      <p>Welcome, {user.name}!</p>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

### 10. 성능 최적화

#### React.memo
```jsx
import { memo } from 'react';

const Button = memo(({ label, onClick }) => {
  return <button onClick={onClick}>{label}</button>;
});
```

#### useMemo와 useCallback
```jsx
function Parent({ items }) {
  const expensiveValue = useMemo(() => {
    return items.reduce((sum, item) => sum + item.value, 0);
  }, [items]);
  
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []);
  
  return <Child value={expensiveValue} onClick={handleClick} />;
}
```

### 11. 주요 라이브러리

#### React Router (라우팅)
```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users/:id" element={<UserDetail />} />
      </Routes>
    </BrowserRouter>
  );
}
```

#### Axios (HTTP 클라이언트)
```jsx
import axios from 'axios';

axios.get('/api/users')
  .then(response => console.log(response.data))
  .catch(error => console.error(error));
```

---

## 학습 순서 추천

1. **기초**: JSX, 컴포넌트, Props
2. **상태 관리**: useState, 이벤트 처리
3. **생명주기**: useEffect 기본 사용
4. **조건부 렌더링**: 삼항 연산자, 논리 연산자
5. **리스트 렌더링**: map, key
6. **폼 처리**: 제어 컴포넌트
7. **고급 Hooks**: useContext, useReducer
8. **성능 최적화**: React.memo, useMemo, useCallback
9. **커스텀 Hooks**: 재사용 가능한 로직 추출
10. **상태 관리**: Context API 또는 Redux
11. **라우팅**: React Router
12. **테스트**: React Testing Library

---

## 참고 자료

* [React 공식 문서](https://react.dev/)
* [React 한국어 문서](https://ko.react.dev/)
* [React Hooks 문서](https://react.dev/reference/react)
* [Create React App](https://create-react-app.dev/)
* [Next.js 문서](https://nextjs.org/docs)

---

## 참고

* React는 라이브러리이므로 필요한 도구들을 직접 선택해야 합니다
* 함수형 컴포넌트와 Hooks를 사용하는 것이 현재 권장 방식입니다
* 성능 최적화는 필요할 때만 적용하세요 (과도한 최적화는 금물)
* 커뮤니티가 크므로 문제 해결이 상대적으로 쉽습니다
* 지속적으로 업데이트되므로 최신 문서를 확인하세요

