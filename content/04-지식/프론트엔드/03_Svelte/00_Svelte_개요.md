---
publish: true
tags: [지식, 프론트엔드]
---

# Svelte 개요

---

## Svelte란?

> **Svelte**는 Rich Harris가 2016년에 개발한 혁신적인 프론트엔드 프레임워크입니다. 다른 프레임워크들과 달리 런타임 프레임워크가 없고, 빌드 타임에 컴파일러가 작동하여 순수 JavaScript로 변환합니다. 이로 인해 작은 번들 크기, 빠른 성능, 간결한 문법을 제공하며, 최근 주목받고 있는 프레임워크입니다.

---

## 목차

1. [Svelte의 역사](#svelte의-역사)
2. [장단점](#장단점)
3. [폴더 구조](#폴더-구조)
4. [기본적으로 알아야 할 것](#기본적으로-알아야-할-것)

---

## Svelte의 역사

### 1. 초기 개발 (2016-2017)
* **2016년 11월**: Svelte 최초 공개
* **개발자**: Rich Harris (The New York Times)
* **배경**: 기존 프레임워크의 번들 크기와 성능 문제 해결
* **혁신적 접근**: "프레임워크 없는 프레임워크" 컨셉

### 2. Svelte 2 (2018년)
* **2018년 4월**: Svelte 2.0 릴리즈
* **주요 특징**:
  * 컴포넌트 기반 아키텍처
  * 반응형 문법 (`:` 바인딩)
  * 작은 번들 크기
* **인지도**: 여전히 작은 커뮤니티

### 3. Svelte 3 (2019년) - 큰 전환점
* **2019년 4월**: Svelte 3.0 릴리즈
* **주요 변화**:
  * **반응형 문법 개선**: `$:` 문법 도입
  * **컴파일러 개선**: 더 효율적인 코드 생성
  * **문서화 강화**: 공식 문서 대폭 개선
  * **개발자 경험 향상**: 더 나은 에러 메시지
* **반응**: 개발자들의 관심 급증

### 4. Svelte 4 (2023년)
* **2023년 6월**: Svelte 4.0 릴리즈
* **주요 개선**:
  * **타입 안정성**: TypeScript 지원 강화
  * **성능 향상**: 컴파일러 최적화
  * **번들 크기 감소**: 프레임워크 자체 크기 감소
  * **개발 도구**: Svelte Language Tools 개선

### 5. Svelte 5 (개발 중)
* **예상 기능**:
  * **Runes**: 새로운 반응성 시스템
  * **더 나은 타입 추론**: TypeScript 지원 개선
  * **성능 최적화**: 추가 최적화

### 6. SvelteKit (2021년)
* **2021년 3월**: SvelteKit 1.0 릴리즈
* **역할**: Next.js, Nuxt.js와 유사한 풀스택 프레임워크
* **기능**:
  * 서버 사이드 렌더링 (SSR)
  * 정적 사이트 생성 (SSG)
  * 파일 기반 라우팅
  * API 라우트
  * 코드 분할
* **영향**: Svelte 생태계 확장

### 커뮤니티 성장
* **2019년**: Stack Overflow 설문에서 가장 사랑받는 프레임워크 1위
* **2020년**: State of JS에서 높은 만족도
* **2021년**: SvelteKit 출시로 실용성 향상
* **2023년**: 계속해서 성장하는 커뮤니티

---

## 장단점

### 장점

#### 1. 작은 번들 크기
* **런타임 없음**: 프레임워크 코드가 번들에 포함되지 않음
* **컴파일 최적화**: 필요한 코드만 생성
* **초기 로딩**: 빠른 초기 로딩 시간
* **모바일 친화**: 느린 네트워크 환경에 유리

#### 2. 뛰어난 성능
* **빌드 타임 최적화**: 컴파일 시점에 최적화 수행
* **가상 DOM 없음**: 직접 DOM 업데이트로 빠름
* **최소한의 리렌더링**: 변경된 부분만 업데이트
* **런타임 오버헤드 없음**: 순수 JavaScript 실행

#### 3. 간결한 문법
* **적은 보일러플레이트**: 최소한의 코드로 작성 가능
* **직관적**: HTML, CSS, JavaScript 그대로 사용
* **학습 곡선 낮음**: 기존 웹 개발 지식으로 시작 가능
* **가독성**: 코드가 명확하고 이해하기 쉬움

#### 4. 내장된 반응성
* **자동 추적**: 변수 변경 자동 감지
* **간단한 문법**: `$:` 문법으로 반응성 선언
* **외부 라이브러리 불필요**: 상태 관리 라이브러리 필요 없음
* **예측 가능**: 명확한 데이터 흐름

#### 5. 스타일링 내장
* **컴포넌트 스코프 CSS**: 스타일 자동 스코핑
* **CSS 변수 지원**: JavaScript에서 CSS 변수 조작
* **애니메이션**: 내장 애니메이션 지원
* **전처리기**: Sass, Less, PostCSS 지원

#### 6. 접근성 (Accessibility)
* **컴파일 타임 검사**: 접근성 문제 컴파일 시 경고
* **시맨틱 HTML**: 표준 HTML 사용
* **ARIA 지원**: 자동 ARIA 속성 처리

#### 7. 개발 경험
* **빠른 빌드**: Vite와 통합으로 빠른 개발 서버
* **Hot Module Replacement**: 즉시 반영
* **명확한 에러 메시지**: 친절한 에러 메시지
* **TypeScript 지원**: 타입 안정성 제공

#### 8. 생태계
* **SvelteKit**: 풀스택 프레임워크
* **Svelte Material UI**: Material Design 컴포넌트
* **Sveltestrap**: Bootstrap 컴포넌트
* **점진적 성장**: 커뮤니티와 라이브러리 증가

### 단점

#### 1. 작은 커뮤니티
* **작은 생태계**: React, Vue에 비해 라이브러리 적음
* **학습 자료**: 튜토리얼과 자료가 상대적으로 적음
* **Stack Overflow**: 질문과 답변이 적음
* **직장 시장**: 채용 공고가 상대적으로 적음

#### 2. 신생 기술
* **안정성 우려**: 아직 비교적 새로운 기술
* **Breaking Changes**: 주요 업데이트 시 변경 가능
* **베스트 프랙티스**: 아직 확립 중
* **장기 지원**: 장기 프로젝트에서의 검증 부족

#### 3. 타사 라이브러리 통합
* **제한적 통합**: 일부 라이브러리가 Svelte 지원 안 함
* **래퍼 필요**: React 라이브러리 사용 시 래퍼 필요
* **선택지 제한**: 특정 기능의 라이브러리 선택지 적음

#### 4. 디버깅
* **소스맵**: 컴파일된 코드 디버깅 복잡
* **도구 제한**: React DevTools 같은 도구 부족
* **에러 추적**: 컴파일된 코드에서 에러 추적 어려울 수 있음

#### 5. TypeScript 지원
* **개선 중**: TypeScript 지원이 계속 개선 중
* **일부 제한**: 일부 고급 타입 기능 제한
* **타입 추론**: 복잡한 경우 타입 추론 부족 가능

#### 6. 서버 사이드 렌더링
* **SvelteKit 필요**: SSR을 위해 SvelteKit 필요
* **설정 복잡도**: Next.js 대비 설정이 더 복잡할 수 있음
* **문서**: SSR 관련 문서가 상대적으로 적음

#### 7. 기업 지원
* **Meta, Google 지원 없음**: React, Angular처럼 대기업 지원 없음
* **개인 개발자 주도**: 주로 개인 개발자와 커뮤니티 주도
* **장기 지원**: 장기적 지원에 대한 불확실성

#### 8. 학습 자료
* **한국어 자료 부족**: 한국어 학습 자료 상대적으로 적음
* **비디오 강의**: 온라인 강의가 React, Vue에 비해 적음
* **책**: 출판된 책이 적음

---

## 폴더 구조

### 기본 Svelte 프로젝트 구조 (Vite)

```
my-svelte-app/
├── public/                        # 정적 파일
│   └── favicon.png
├── src/                           # 소스 코드
│   ├── lib/                       # 재사용 가능한 컴포넌트/유틸
│   │   ├── components/            # 공유 컴포넌트
│   │   │   ├── Button/
│   │   │   │   ├── Button.svelte
│   │   │   │   └── index.js
│   │   │   └── Card/
│   │   ├── stores/                # Svelte stores
│   │   │   └── userStore.js
│   │   └── utils/                 # 유틸리티 함수
│   │       └── helpers.js
│   ├── routes/                    # 라우팅 (SvelteKit)
│   │   ├── +page.svelte          # 홈 (/)
│   │   ├── about/
│   │   │   └── +page.svelte      # /about
│   │   └── users/
│   │       ├── +page.svelte      # /users
│   │       └── [id]/
│   │           └── +page.svelte  # /users/:id
│   ├── app.html                   # HTML 템플릿
│   ├── app.css                    # 전역 스타일
│   └── main.js                    # 진입점
├── static/                        # 정적 파일 (SvelteKit)
├── .svelte-kit/                   # 빌드 캐시 (SvelteKit)
├── package.json
├── vite.config.js                 # Vite 설정
├── svelte.config.js               # Svelte 설정 (SvelteKit)
└── README.md
```

### SvelteKit 프로젝트 구조

```
my-sveltekit-app/
├── src/
│   ├── lib/                       # 재사용 가능한 코드
│   │   ├── components/
│   │   │   ├── Button.svelte
│   │   │   └── Modal.svelte
│   │   ├── stores/
│   │   │   └── store.js
│   │   └── utils/
│   │       └── api.js
│   ├── routes/                    # 파일 기반 라우팅
│   │   ├── +layout.svelte        # 레이아웃
│   │   ├── +page.svelte          # / (홈)
│   │   ├── +error.svelte         # 에러 페이지
│   │   ├── about/
│   │   │   ├── +page.svelte      # /about
│   │   │   └── +page.server.js   # 서버 로직
│   │   ├── blog/
│   │   │   ├── +page.svelte
│   │   │   └── [slug]/
│   │   │       ├── +page.svelte  # /blog/:slug
│   │   │       └── +page.server.js
│   │   └── api/                   # API 라우트
│   │       └── users/
│   │           └── +server.js     # /api/users
│   ├── app.html                   # HTML 템플릿
│   └── app.d.ts                   # 타입 정의
├── static/                        # 정적 파일
├── tests/                         # 테스트
├── package.json
├── svelte.config.js               # SvelteKit 설정
└── vite.config.js
```

### 기능 기반 구조

```
src/
├── lib/
│   ├── features/                  # 기능별 모듈
│   │   ├── auth/
│   │   │   ├── components/
│   │   │   │   ├── LoginForm.svelte
│   │   │   │   └── SignupForm.svelte
│   │   │   ├── stores/
│   │   │   │   └── authStore.js
│   │   │   └── utils/
│   │   │       └── auth.js
│   │   ├── users/
│   │   │   ├── components/
│   │   │   │   ├── UserList.svelte
│   │   │   │   └── UserCard.svelte
│   │   │   ├── stores/
│   │   │   │   └── userStore.js
│   │   │   └── api/
│   │   │       └── users.js
│   │   └── products/
│   ├── shared/                    # 공유 컴포넌트
│   │   ├── components/
│   │   │   ├── Button.svelte
│   │   │   └── Input.svelte
│   │   └── utils/
│   │       └── helpers.js
│   └── stores/                    # 전역 stores
│       └── appStore.js
├── routes/
└── app.html
```

### 컴포넌트 파일 구조

#### 단일 파일 컴포넌트 (.svelte)

```svelte
<!-- ComponentName.svelte -->
<script>
  // JavaScript/TypeScript
  export let prop = 'default';
  
  let state = 'reactive';
  
  $: derived = prop + state;
  
  function handleClick() {
    state = 'updated';
  }
</script>

<!-- HTML 템플릿 -->
<div class="container">
  <h1>{prop}</h1>
  <p>{derived}</p>
  <button on:click={handleClick}>Click</button>
</div>

<!-- 스타일 (자동 스코핑) -->
<style>
  .container {
    padding: 1rem;
  }
  
  h1 {
    color: #333;
  }
</style>
```

### 폴더 구조 베스트 프랙티스

#### 1. 컴포넌트는 파일로 관리
* 각 컴포넌트는 단일 `.svelte` 파일
* 관련 로직, 템플릿, 스타일을 하나의 파일에

#### 2. lib 폴더 활용
* 재사용 가능한 컴포넌트는 `lib/` 폴더
* 프로젝트 전반에서 사용하는 유틸리티 함수

#### 3. Stores 분리
* 전역 상태는 `stores/` 폴더
* 기능별 store는 각 기능 폴더 내부

#### 4. 라우팅 (SvelteKit)
* 파일 기반 라우팅 사용
* `+page.svelte`, `+layout.svelte` 컨벤션 따르기

---

## 기본적으로 알아야 할 것

### 1. 핵심 개념

#### 컴포넌트 (Component)
* **역할**: UI를 구성하는 재사용 가능한 단위
* **파일**: `.svelte` 확장자 사용
* **구성**: `<script>`, `<template>`, `<style>` 섹션

```svelte
<!-- Button.svelte -->
<script>
  export let label = 'Click me';
  export let variant = 'primary';
  
  function handleClick() {
    console.log('Clicked!');
  }
</script>

<button 
  class="btn btn-{variant}" 
  on:click={handleClick}
>
  {label}
</button>

<style>
  .btn {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-primary {
    background-color: #007bff;
    color: white;
  }
</style>
```

#### Props (속성)
* **역할**: 부모에서 자식으로 데이터 전달
* **선언**: `export let` 키워드 사용
* **기본값**: 선언 시 할당 가능

```svelte
<script>
  export let name = 'Guest';  // 기본값
  export let age;
</script>

<p>Hello, {name}! You are {age} years old.</p>

<!-- 사용 -->
<User name="John" age={25} />
```

#### 반응성 (Reactivity)
* **자동 추적**: 변수 변경 시 자동 업데이트
* **반응형 선언**: `$:` 문법 사용
* **반응형 문**: `$: if`, `$: { }` 블록 사용

```svelte
<script>
  let count = 0;
  let doubled = 0;
  
  // 반응형 선언: count가 변경되면 자동으로 실행
  $: doubled = count * 2;
  
  // 복잡한 반응형 문
  $: {
    console.log(`Count is ${count}, doubled is ${doubled}`);
  }
  
  // 조건부 반응성
  $: if (count > 10) {
    console.log('Count is greater than 10');
  }
</script>

<button on:click={() => count++}>
  Count: {count} (Doubled: {doubled})
</button>
```

#### 이벤트 처리
* **DOM 이벤트**: `on:eventname` 문법
* **커스텀 이벤트**: `createEventDispatcher` 사용
* **이벤트 수식어**: `preventDefault`, `stopPropagation` 등

```svelte
<script>
  import { createEventDispatcher } from 'svelte';
  
  const dispatch = createEventDispatcher();
  
  function handleClick(event) {
    dispatch('customEvent', { data: 'value' });
  }
</script>

<!-- DOM 이벤트 -->
<button on:click={handleClick}>Click</button>

<!-- 인라인 핸들러 -->
<button on:click={() => console.log('Clicked')}>
  Click
</button>

<!-- 이벤트 수식어 -->
<form on:submit|preventDefault={handleSubmit}>
  <button type="submit">Submit</button>
</form>

<!-- 이벤트 전달 -->
<ChildComponent on:customEvent={handleCustom} />
```

### 2. 조건부 렌더링

```svelte
<script>
  let isVisible = true;
  let user = { name: 'John', role: 'admin' };
</script>

<!-- if 블록 -->
{#if isVisible}
  <p>This is visible</p>
{/if}

<!-- if-else -->
{#if user.role === 'admin'}
  <AdminPanel />
{:else}
  <UserPanel />
{/if}

<!-- if-else if-else -->
{#if count > 10}
  <p>Greater than 10</p>
{:else if count > 5}
  <p>Greater than 5</p>
{:else}
  <p>Less than or equal to 5</p>
{/if}
```

### 3. 리스트 렌더링

```svelte
<script>
  let items = [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ];
</script>

<!-- each 블록 -->
<ul>
  {#each items as item}
    <li>{item.name}</li>
  {/each}
</ul>

<!-- 인덱스 사용 -->
<ul>
  {#each items as item, index}
    <li>{index + 1}: {item.name}</li>
  {/each}
</ul>

<!-- key 블록 (성능 최적화) -->
<ul>
  {#each items as item (item.id)}
    <li>{item.name}</li>
  {/each}
</ul>

<!-- 빈 상태 처리 -->
{#each items as item}
  <ItemCard {item} />
{:else}
  <p>No items found</p>
{/each}
```

### 4. 비동기 처리

```svelte
<script>
  let promise = fetch('/api/data').then(r => r.json());
</script>

<!-- await 블록 -->
{#await promise}
  <p>Loading...</p>
{:then data}
  <div>
    {#each data as item}
      <p>{item.name}</p>
    {/each}
  </div>
{:catch error}
  <p>Error: {error.message}</p>
{/await}

<!-- 재할당 가능한 promise -->
<script>
  let promise;
  
  async function loadData() {
    promise = fetch('/api/data').then(r => r.json());
  }
  
  loadData();
</script>
```

### 5. Stores (상태 관리)

#### writable store
```javascript
// stores.js
import { writable } from 'svelte/store';

export const count = writable(0);

// 사용
import { count } from './stores.js';

// 구독
count.subscribe(value => {
  console.log(value);
});

// 값 업데이트
count.update(n => n + 1);
count.set(10);

// 자동 구독 (컴포넌트 내부)
<script>
  import { count } from './stores.js';
  
  // 자동 구독 및 구독 해제
  $count += 1;  // 값 변경
  console.log($count);  // 값 읽기
</script>

<p>Count: {$count}</p>
```

#### readable store
```javascript
import { readable } from 'svelte/store';

export const time = readable(new Date(), function start(set) {
  const interval = setInterval(() => {
    set(new Date());
  }, 1000);
  
  return function stop() {
    clearInterval(interval);
  };
});

// 사용
<script>
  import { time } from './stores.js';
</script>

<p>Current time: {$time}</p>
```

#### derived store
```javascript
import { writable, derived } from 'svelte/store';

export const count = writable(0);
export const doubled = derived(count, $count => $count * 2);

// 사용
<script>
  import { doubled } from './stores.js';
</script>

<p>Doubled: {$doubled}</p>
```

#### 커스텀 store
```javascript
import { writable } from 'svelte/store';

function createCount() {
  const { subscribe, set, update } = writable(0);
  
  return {
    subscribe,
    increment: () => update(n => n + 1),
    decrement: () => update(n => n - 1),
    reset: () => set(0)
  };
}

export const count = createCount();

// 사용
<script>
  import { count } from './stores.js';
</script>

<button on:click={count.increment}>+</button>
<button on:click={count.decrement}>-</button>
<button on:click={count.reset}>Reset</button>
```

### 6. 바인딩

#### 양방향 바인딩
```svelte
<script>
  let name = '';
  let value = 50;
</script>

<!-- 입력 바인딩 -->
<input bind:value={name} placeholder="Enter name" />
<p>Hello, {name}!</p>

<!-- 체크박스 -->
<input type="checkbox" bind:checked={isChecked} />

<!-- 라디오 버튼 -->
<input type="radio" bind:group={selected} value="option1" />

<!-- 셀렉트 -->
<select bind:value={selected}>
  <option value="option1">Option 1</option>
  <option value="option2">Option 2</option>
</select>

<!-- 텍스트 영역 -->
<textarea bind:value={text}></textarea>

<!-- 범위 입력 -->
<input type="range" bind:value={value} min="0" max="100" />
```

#### 클래스 바인딩
```svelte
<script>
  let isActive = true;
  let className = 'custom-class';
</script>

<!-- 클래스 바인딩 -->
<div class:active={isActive}>
  Content
</div>

<!-- 축약 문법 (변수명과 클래스명이 같을 때) -->
<div class:isActive>
  Content
</div>

<!-- 여러 클래스 -->
<div class="{className} {isActive ? 'active' : ''}">
  Content
</div>
```

### 7. 생명주기

```svelte
<script>
  import { onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte';
  
  let mounted = false;
  
  onMount(() => {
    // 컴포넌트가 DOM에 마운트된 후 실행
    mounted = true;
    console.log('Component mounted');
    
    // 클린업 함수 반환 가능
    return () => {
      console.log('Cleanup');
    };
  });
  
  onDestroy(() => {
    // 컴포넌트가 파괴되기 전 실행
    console.log('Component destroyed');
  });
  
  beforeUpdate(() => {
    // DOM 업데이트 전 실행
    console.log('Before update');
  });
  
  afterUpdate(() => {
    // DOM 업데이트 후 실행
    console.log('After update');
  });
</script>
```

### 8. 액션 (Actions)

```svelte
<script>
  function longpress(node, duration) {
    let timer;
    
    const handleMousedown = () => {
      timer = setTimeout(() => {
        node.dispatchEvent(new CustomEvent('longpress'));
      }, duration);
    };
    
    const handleMouseup = () => {
      clearTimeout(timer);
    };
    
    node.addEventListener('mousedown', handleMousedown);
    node.addEventListener('mouseup', handleMouseup);
    
    return {
      destroy() {
        node.removeEventListener('mousedown', handleMousedown);
        node.removeEventListener('mouseup', handleMouseup);
      }
    };
  }
</script>

<button use:longpress={500} on:longpress={() => alert('Long pressed!')}>
  Press and hold
</button>
```

### 9. 트랜지션 (Transitions)

```svelte
<script>
  import { fade, fly, slide } from 'svelte/transition';
  let visible = true;
</script>

<!-- 페이드 -->
{#if visible}
  <div transition:fade>
    Fading in and out
  </div>
{/if}

<!-- 슬라이드 -->
{#if visible}
  <div transition:slide={{ duration: 500 }}>
    Sliding
  </div>
{/if}

<!-- 플라이 -->
{#if visible}
  <div in:fly={{ y: 50, duration: 500 }} out:fade>
    Flying in, fading out
  </div>
{/if}

<!-- 커스텀 트랜지션 -->
<script>
  import { quintOut } from 'svelte/easing';
  import { fly } from 'svelte/transition';
</script>

<div transition:fly={{ y: 200, duration: 2000, easing: quintOut }}>
  Custom transition
</div>
```

### 10. 애니메이션

```svelte
<script>
  import { flip } from 'svelte/animate';
  let items = [1, 2, 3, 4, 5];
</script>

{#each items as item (item)}
  <div animate:flip={{ duration: 300 }}>
    {item}
  </div>
{/each}
```

### 11. 슬롯 (Slots)

```svelte
<!-- Card.svelte -->
<div class="card">
  <slot name="header">
    <h2>Default Header</h2>
  </slot>
  
  <slot>
    <p>Default content</p>
  </slot>
  
  <slot name="footer">
    <p>Default footer</p>
  </slot>
</div>

<!-- 사용 -->
<Card>
  <h2 slot="header">Custom Header</h2>
  <p>Custom content</p>
  <button slot="footer">Action</button>
</Card>
```

### 12. 컨텍스트 (Context)

```svelte
<!-- Parent.svelte -->
<script>
  import { setContext } from 'svelte';
  import Child from './Child.svelte';
  
  setContext('theme', 'dark');
  setContext('user', { name: 'John' });
</script>

<Child />

<!-- Child.svelte -->
<script>
  import { getContext } from 'svelte';
  import Grandchild from './Grandchild.svelte';
  
  const theme = getContext('theme');
  const user = getContext('user');
</script>

<p>Theme: {theme}</p>
<p>User: {user.name}</p>

<Grandchild />
```

### 13. SvelteKit 기본 (라우팅)

```svelte
<!-- src/routes/+page.svelte -->
<script>
  import { page } from '$app/stores';
</script>

<h1>Home Page</h1>
<p>Current route: {$page.url.pathname}</p>

<!-- src/routes/about/+page.svelte -->
<h1>About Page</h1>

<!-- src/routes/users/[id]/+page.svelte -->
<script>
  export let data;  // +page.server.js 또는 +page.js에서
</script>

<h1>User {data.id}</h1>
<p>Name: {data.name}</p>

<!-- src/routes/users/[id]/+page.server.js -->
export async function load({ params }) {
  const user = await fetchUser(params.id);
  return {
    id: params.id,
    name: user.name
  };
}
```

---

## 학습 순서 추천

1. **기초**: 컴포넌트, Props, 이벤트 처리
2. **반응성**: `$:` 문법, 반응형 선언
3. **조건부/리스트**: `{#if}`, `{#each}` 블록
4. **바인딩**: 양방향 바인딩, 클래스 바인딩
5. **Stores**: 상태 관리 기본
6. **생명주기**: onMount, onDestroy
7. **트랜지션**: fade, slide, fly
8. **슬롯**: 컴포넌트 컴포지션
9. **컨텍스트**: 컴포넌트 간 데이터 공유
10. **SvelteKit**: 라우팅, SSR, API 라우트
11. **고급 주제**: 액션, 애니메이션, 커스텀 트랜지션

---

## 참고 자료

* [Svelte 공식 문서](https://svelte.dev/)
* [Svelte 튜토리얼](https://svelte.dev/tutorial/basics)
* [SvelteKit 문서](https://kit.svelte.dev/)
* [Svelte 공식 예제](https://svelte.dev/examples)
* [Svelte 커뮤니티](https://svelte.dev/community)

---

## 참고

* Svelte는 컴파일러 기반이므로 런타임 오버헤드가 없습니다
* 작은 번들 크기와 빠른 성능이 주요 장점입니다
* 간결한 문법으로 빠르게 개발할 수 있습니다
* 커뮤니티가 작지만 활발히 성장하고 있습니다
* SvelteKit을 사용하면 풀스택 애플리케이션을 만들 수 있습니다
* TypeScript 지원이 계속 개선되고 있습니다

