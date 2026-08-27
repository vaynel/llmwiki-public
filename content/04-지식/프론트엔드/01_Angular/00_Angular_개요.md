---
publish: true
tags: [지식, 프론트엔드]
---

# Angular 개요

---

## Angular란?

> **Angular**는 구글에서 개발하고 유지보수하는 TypeScript 기반의 오픈소스 웹 애플리케이션 프레임워크입니다. 엔터프라이즈급 애플리케이션 개발을 위한 완전한 기능을 갖춘 프레임워크로, 단일 페이지 애플리케이션(SPA) 개발을 위한 강력한 도구입니다.

---

## 목차

1. [Angular의 역사](#angular의-역사)
2. [장단점](#장단점)
3. [폴더 구조](#폴더-구조)
4. [기본적으로 알아야 할 것](#기본적으로-알아야-할-것)

---

## Angular의 역사

### 1. AngularJS (Angular 1.x)
* **2010년**: 구글의 Miško Hevery와 Adam Abrons가 AngularJS 개발 시작
* **2012년**: AngularJS 1.0 릴리즈
* **특징**: JavaScript 기반, 양방향 데이터 바인딩, DI(Dependency Injection) 도입
* **문제점**: 성능 이슈, 복잡한 구조, 스케일링 어려움

### 2. Angular 2 (완전한 재작성)
* **2016년 9월**: Angular 2.0 릴리즈
* **주요 변화**:
  * TypeScript 기반으로 전환
  * 컴포넌트 기반 아키텍처
  * 성능 대폭 개선 (Zone.js, AOT 컴파일)
  * 모바일 우선 설계
* **Breaking Changes**: AngularJS와 호환되지 않음

### 3. Angular 4~8 (점진적 개선)
* **2017년 3월**: Angular 4.0 (버전 3 생략)
* **주요 기능**:
  * Angular CLI 강화
  * Ivy 렌더러 (렌더링 엔진) 개발 시작
  * 서버 사이드 렌더링 (Angular Universal)
  * 동적 컴포넌트 로딩

### 4. Angular 9~15 (Ivy 렌더러)
* **2020년 2월**: Angular 9.0, Ivy 렌더러 기본 활성화
* **주요 개선사항**:
  * 번들 크기 감소 (최대 40% 감소)
  * 빌드 시간 단축
  * 트리 셰이킹 개선
  * 컴포넌트 테스트 쉬워짐

### 5. Angular 16+ (최신 버전)
* **2023년 5월**: Angular 16.0
* **주요 기능**:
  * Signals (반응형 상태 관리)
  * 서버 사이드 렌더링 개선
  * 하이드레이션 지원
  * 독립형 컴포넌트 (Standalone Components)

### 버전 관리 전략
* **6개월 주기**: 새로운 주요 버전 릴리즈
* **LTS (Long Term Support)**: 12개월 지원 기간
* **하위 호환성**: 주요 버전 간 마이그레이션 가이드 제공

---

## 장단점

### 장점

#### 1. 완전한 프레임워크
* **의존성 주입 (DI)**: 내장된 강력한 DI 시스템
* **라우팅**: Angular Router 제공
* **HTTP 클라이언트**: HttpClient 모듈 내장
* **폼 관리**: Reactive Forms와 Template-driven Forms 지원
* **애니메이션**: Angular Animations API 제공
* **테스트**: Karma와 Jasmine 통합 지원

#### 2. TypeScript 기반
* **타입 안정성**: 컴파일 타임 오류 검출
* **IDE 지원**: 자동완성, 리팩토링 강화
* **코드 품질**: 대규모 프로젝트에 적합
* **유지보수성**: 코드 가독성 향상

#### 3. 엔터프라이즈급 기능
* **모듈화**: NgModules를 통한 코드 구조화
* **지연 로딩 (Lazy Loading)**: 성능 최적화
* **AOT 컴파일**: Ahead-of-Time 컴파일로 런타임 성능 향상
* **서버 사이드 렌더링**: Angular Universal 지원

#### 4. 강력한 CLI 도구
* **프로젝트 생성**: `ng new` 명령어로 빠른 시작
* **코드 생성**: 컴포넌트, 서비스, 모듈 자동 생성
* **빌드 및 배포**: 프로덕션 빌드 최적화
* **테스트 실행**: 유닛 테스트 및 E2E 테스트 지원

#### 5. 풍부한 생태계
* **Material Design**: Angular Material UI 컴포넌트 라이브러리
* **PrimeNG, NgBootstrap**: 다양한 UI 라이브러리
* **NgRx**: 상태 관리 라이브러리 (Redux 패턴)
* **RxJS**: 반응형 프로그래밍 지원

#### 6. 대규모 팀 협업
* **명확한 구조**: 컨벤션과 베스트 프랙티스 제공
* **코드 스타일**: TypeScript와 ESLint 통합
* **문서화**: 공식 문서가 매우 상세함

### 단점

#### 1. 높은 학습 곡선
* **복잡한 개념**: 모듈, 컴포넌트, 서비스, 의존성 주입 등
* **TypeScript 필요**: JavaScript만으로는 부족
* **RxJS 학습**: Observable, Operator 개념 이해 필요
* **초기 학습 시간**: 다른 프레임워크 대비 더 많은 시간 소요

#### 2. 번들 크기
* **큰 초기 번들**: 프레임워크 자체가 큼
* **최적화 필요**: 트리 셰이킹, 코드 분할 필요
* **느린 초기 로딩**: 작은 프로젝트에는 과할 수 있음

#### 3. 보일러플레이트 코드
* **많은 설정**: 컴포넌트 생성 시 많은 코드 필요
* **반복적인 코드**: 데코레이터, 임포트 등
* **설정 복잡도**: 모듈, 라우팅, 서비스 설정 등

#### 4. 업데이트 복잡도
* **Breaking Changes**: 버전 업데이트 시 마이그레이션 필요
* **의존성 관리**: 많은 패키지 의존성 관리 어려움
* **업그레이드 시간**: 프로젝트 규모에 따라 오래 걸림

#### 5. 성능
* **초기 렌더링**: 큰 애플리케이션에서 느릴 수 있음
* **변경 감지**: 기본 Change Detection이 모든 컴포넌트 체크
* **최적화 필요**: OnPush 전략 등 수동 최적화 필요

#### 6. 커뮤니티
* **React 대비 작음**: React에 비해 커뮤니티 규모 작음
* **라이브러리 선택**: 특정 라이브러리의 Angular 버전 부족 가능

---

## 폴더 구조

### 기본 Angular 프로젝트 구조

```
my-angular-app/
├── e2e/                          # E2E 테스트 파일
│   ├── src/
│   └── protractor.conf.js
├── node_modules/                 # 의존성 패키지
├── src/                          # 소스 코드
│   ├── app/                      # 애플리케이션 코드
│   │   ├── core/                 # 핵심 기능 (싱글톤 서비스)
│   │   │   ├── services/
│   │   │   ├── guards/
│   │   │   ├── interceptors/
│   │   │   └── models/
│   │   ├── shared/               # 공유 모듈 및 컴포넌트
│   │   │   ├── components/
│   │   │   ├── directives/
│   │   │   ├── pipes/
│   │   │   └── shared.module.ts
│   │   ├── features/             # 기능 모듈
│   │   │   ├── users/
│   │   │   │   ├── components/
│   │   │   │   ├── services/
│   │   │   │   ├── models/
│   │   │   │   └── users.module.ts
│   │   │   ├── products/
│   │   │   └── orders/
│   │   ├── layout/               # 레이아웃 컴포넌트
│   │   │   ├── header/
│   │   │   ├── footer/
│   │   │   └── sidebar/
│   │   ├── app.component.ts      # 루트 컴포넌트
│   │   ├── app.component.html
│   │   ├── app.component.scss
│   │   ├── app.module.ts         # 루트 모듈
│   │   └── app-routing.module.ts # 라우팅 설정
│   ├── assets/                   # 정적 파일
│   │   ├── images/
│   │   ├── fonts/
│   │   └── styles/
│   ├── environments/             # 환경 설정
│   │   ├── environment.ts
│   │   └── environment.prod.ts
│   ├── styles.scss               # 전역 스타일
│   ├── main.ts                   # 애플리케이션 진입점
│   ├── index.html                # HTML 템플릿
│   └── polyfills.ts              # 폴리필
├── angular.json                  # Angular CLI 설정
├── package.json                  # 프로젝트 의존성
├── tsconfig.json                 # TypeScript 설정
├── tsconfig.app.json
├── tsconfig.spec.json
└── README.md
```

### 주요 폴더 설명

#### 1. `app/core/`
* **목적**: 애플리케이션 전역에서 사용하는 싱글톤 서비스
* **포함**: 인증 서비스, HTTP 인터셉터, 가드, 유틸리티 함수
* **규칙**: AppModule에서만 임포트, 한 번만 제공

#### 2. `app/shared/`
* **목적**: 여러 기능 모듈에서 재사용하는 컴포넌트, 디렉티브, 파이프
* **포함**: 버튼, 입력, 모달, 공용 컴포넌트
* **규칙**: SharedModule로 구성하여 다른 모듈에 공유

#### 3. `app/features/`
* **목적**: 기능별 모듈 분리 (Feature Module)
* **구조**: 각 기능은 독립적인 폴더로 구성
* **예시**: 사용자 관리, 제품 관리, 주문 관리 등

#### 4. `app/layout/`
* **목적**: 애플리케이션 레이아웃 컴포넌트
* **포함**: 헤더, 푸터, 사이드바, 네비게이션
* **특징**: 여러 페이지에서 공통으로 사용

### 기능 모듈 상세 구조

```
features/users/
├── components/
│   ├── user-list/
│   │   ├── user-list.component.ts
│   │   ├── user-list.component.html
│   │   └── user-list.component.scss
│   ├── user-detail/
│   └── user-form/
├── services/
│   └── user.service.ts
├── models/
│   └── user.model.ts
├── guards/
│   └── user.guard.ts
├── users-routing.module.ts       # 기능별 라우팅
└── users.module.ts               # 기능 모듈
```

### 폴더 구조 베스트 프랙티스

#### 1. Flat Structure (평면 구조)
```
components/
├── user-list.component.ts
├── user-detail.component.ts
└── user-form.component.ts
```

#### 2. Grouped Structure (그룹 구조) - 권장
```
components/
├── user-list/
│   ├── user-list.component.ts
│   ├── user-list.component.html
│   └── user-list.component.scss
└── user-detail/
    ├── user-detail.component.ts
    └── ...
```

#### 3. Feature-based Structure (기능 기반 구조)
```
features/
├── users/
│   ├── components/
│   ├── services/
│   └── models/
└── products/
    ├── components/
    └── services/
```

---

## 기본적으로 알아야 할 것

### 1. 핵심 개념

#### 컴포넌트 (Component)
* **역할**: UI를 구성하는 기본 단위
* **구성**: TypeScript 클래스 + HTML 템플릿 + CSS 스타일
* **생명주기**: ngOnInit, ngOnChanges, ngOnDestroy 등

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.scss']
})
export class UserComponent {
  title = 'User Component';
}
```

#### 모듈 (Module)
* **역할**: 관련된 컴포넌트, 서비스, 디렉티브를 그룹화
* **NgModule**: @NgModule 데코레이터로 정의
* **의존성**: imports, declarations, providers, bootstrap

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

#### 서비스 (Service)
* **역할**: 비즈니스 로직과 데이터 처리
* **의존성 주입**: @Injectable 데코레이터로 제공
* **싱글톤**: 기본적으로 애플리케이션 전체에서 하나의 인스턴스

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class UserService {
  constructor(private http: HttpClient) { }
  
  getUsers() {
    return this.http.get('/api/users');
  }
}
```

#### 디렉티브 (Directive)
* **구조 디렉티브**: DOM 구조를 변경 (*ngIf, *ngFor, *ngSwitch)
* **속성 디렉티브**: 요소의 동작이나 모양 변경 ([ngClass], [ngStyle])
* **커스텀 디렉티브**: 직접 만드는 디렉티브

```typescript
<div *ngIf="isVisible">Visible Content</div>
<div *ngFor="let item of items">{{ item.name }}</div>
<div [ngClass]="{'active': isActive}">Content</div>
```

#### 파이프 (Pipe)
* **역할**: 데이터 변환 및 포맷팅
* **내장 파이프**: DatePipe, CurrencyPipe, JsonPipe 등
* **커스텀 파이프**: @Pipe 데코레이터로 정의

```typescript
{{ date | date:'short' }}
{{ price | currency:'KRW' }}
{{ user | json }}
```

### 2. 데이터 바인딩

#### 문자열 보간 (Interpolation)
```typescript
// TypeScript
title = 'Angular';

// Template
<h1>{{ title }}</h1>
```

#### 속성 바인딩 (Property Binding)
```typescript
// TypeScript
imageUrl = 'https://example.com/image.jpg';
isDisabled = false;

// Template
<img [src]="imageUrl">
<button [disabled]="isDisabled">Click</button>
```

#### 이벤트 바인딩 (Event Binding)
```typescript
// TypeScript
onClick() {
  console.log('Clicked!');
}

// Template
<button (click)="onClick()">Click</button>
```

#### 양방향 바인딩 (Two-way Binding)
```typescript
// TypeScript
name = '';

// Template
<input [(ngModel)]="name">
<p>{{ name }}</p>
```

### 3. 의존성 주입 (Dependency Injection)

#### 기본 사용
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // 싱글톤으로 제공
})
export class DataService {
  getData() {
    return 'Data';
  }
}

// 컴포넌트에서 사용
export class MyComponent {
  constructor(private dataService: DataService) { }
  
  ngOnInit() {
    const data = this.dataService.getData();
  }
}
```

#### Provider 설정
```typescript
// 모듈 레벨
@NgModule({
  providers: [DataService]
})

// 컴포넌트 레벨
@Component({
  providers: [DataService]  // 컴포넌트별 인스턴스
})
```

### 4. 라우팅 (Routing)

#### 기본 설정
```typescript
// app-routing.module.ts
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: '**', redirectTo: '' }  // 404 처리
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

#### 라우터 링크
```html
<a routerLink="/about">About</a>
<a [routerLink]="['/user', userId]">User</a>
```

#### 라우트 파라미터
```typescript
// 라우트 정의
{ path: 'user/:id', component: UserComponent }

// 컴포넌트에서 받기
import { ActivatedRoute } from '@angular/router';

constructor(private route: ActivatedRoute) { }

ngOnInit() {
  const id = this.route.snapshot.paramMap.get('id');
  
  // 또는 Observable 사용
  this.route.paramMap.subscribe(params => {
    const id = params.get('id');
  });
}
```

### 5. 폼 처리

#### 템플릿 기반 폼 (Template-driven Forms)
```typescript
// app.module.ts에 FormsModule 추가
import { FormsModule } from '@angular/forms';

// Template
<form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)">
  <input name="name" ngModel required>
  <input name="email" ngModel email>
  <button type="submit" [disabled]="!userForm.valid">Submit</button>
</form>
```

#### 반응형 폼 (Reactive Forms)
```typescript
// app.module.ts에 ReactiveFormsModule 추가
import { ReactiveFormsModule } from '@angular/forms';

// Component
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

export class UserFormComponent {
  userForm: FormGroup;
  
  constructor(private fb: FormBuilder) {
    this.userForm = this.fb.group({
      name: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]]
    });
  }
  
  onSubmit() {
    if (this.userForm.valid) {
      console.log(this.userForm.value);
    }
  }
}

// Template
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <input formControlName="name">
  <input formControlName="email">
  <button type="submit" [disabled]="!userForm.valid">Submit</button>
</form>
```

### 6. HTTP 통신

#### HttpClient 사용
```typescript
// app.module.ts
import { HttpClientModule } from '@angular/common/http';

// Service
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  constructor(private http: HttpClient) { }
  
  getUsers(): Observable<User[]> {
    return this.http.get<User[]>('/api/users');
  }
  
  createUser(user: User): Observable<User> {
    return this.http.post<User>('/api/users', user);
  }
  
  updateUser(id: number, user: User): Observable<User> {
    return this.http.put<User>(`/api/users/${id}`, user);
  }
  
  deleteUser(id: number): Observable<void> {
    return this.http.delete<void>(`/api/users/${id}`);
  }
}
```

### 7. RxJS 기본

#### Observable과 Subscribe
```typescript
import { Observable } from 'rxjs';

// 서비스에서 Observable 반환
getData(): Observable<Data> {
  return this.http.get<Data>('/api/data');
}

// 컴포넌트에서 구독
ngOnInit() {
  this.apiService.getData().subscribe(
    data => {
      console.log('Success:', data);
    },
    error => {
      console.error('Error:', error);
    }
  );
}
```

#### 주요 Operators
```typescript
import { map, filter, catchError } from 'rxjs/operators';
import { of } from 'rxjs';

this.apiService.getUsers().pipe(
  map(users => users.filter(user => user.active)),
  catchError(error => {
    console.error(error);
    return of([]);
  })
).subscribe(users => {
  this.users = users;
});
```

### 8. 생명주기 훅 (Lifecycle Hooks)

```typescript
import { 
  OnInit, OnDestroy, OnChanges, 
  AfterViewInit, AfterContentInit 
} from '@angular/core';

export class MyComponent implements OnInit, OnDestroy {
  ngOnInit() {
    // 컴포넌트 초기화 시 실행 (한 번)
    // 데이터 로딩, 구독 시작
  }
  
  ngOnDestroy() {
    // 컴포넌트 파괴 시 실행
    // 구독 해제, 타이머 정리
  }
  
  ngOnChanges(changes: SimpleChanges) {
    // 입력 속성 변경 시 실행
  }
  
  ngAfterViewInit() {
    // 뷰 초기화 완료 후 실행
  }
}
```

### 9. 변경 감지 전략 (Change Detection)

#### 기본 전략
* **Default**: 모든 컴포넌트를 매번 체크
* **성능**: 작은 앱에서는 문제없음, 큰 앱에서는 느림

#### OnPush 전략
```typescript
import { ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-user',
  changeDetection: ChangeDetectionStrategy.OnPush,
  // ...
})
export class UserComponent {
  // 참조가 변경될 때만 감지
}
```

### 10. 테스트

#### 컴포넌트 테스트
```typescript
import { TestBed, ComponentFixture } from '@angular/core/testing';
import { UserComponent } from './user.component';

describe('UserComponent', () => {
  let component: UserComponent;
  let fixture: ComponentFixture<UserComponent>;
  
  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [UserComponent]
    });
    fixture = TestBed.createComponent(UserComponent);
    component = fixture.componentInstance;
  });
  
  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

### 11. Angular CLI 주요 명령어

```bash
# 프로젝트 생성
ng new my-app

# 개발 서버 실행
ng serve

# 프로덕션 빌드
ng build --prod

# 컴포넌트 생성
ng generate component user

# 서비스 생성
ng generate service user

# 모듈 생성
ng generate module users

# 테스트 실행
ng test

# E2E 테스트
ng e2e

# 라이브러리 생성
ng generate library my-lib
```

---

## 학습 순서 추천

1. **기초 개념**: 컴포넌트, 모듈, 서비스
2. **데이터 바인딩**: 보간, 속성, 이벤트, 양방향
3. **디렉티브**: 구조 디렉티브, 속성 디렉티브
4. **의존성 주입**: 서비스 생성 및 주입
5. **라우팅**: 라우터 설정 및 네비게이션
6. **폼 처리**: 템플릿 기반 폼, 반응형 폼
7. **HTTP 통신**: HttpClient 사용
8. **RxJS**: Observable, Operators
9. **고급 주제**: 모듈 지연 로딩, 가드, 인터셉터
10. **상태 관리**: NgRx 또는 서비스 기반 상태 관리

---

## 참고 자료

* [Angular 공식 문서](https://angular.io/docs)
* [Angular 한국어 공식 문서](https://angular.kr/)
* [Angular Material](https://material.angular.io/)
* [RxJS 문서](https://rxjs.dev/)
* [Angular Style Guide](https://angular.io/guide/styleguide)

---

## 참고

* Angular는 대규모 엔터프라이즈 애플리케이션에 적합한 프레임워크입니다
* TypeScript와 RxJS 학습이 필수입니다
* 모듈 기반 구조를 이해하면 프로젝트 관리가 쉬워집니다
* 공식 문서가 매우 상세하므로 참고하세요
* 정기적인 업데이트에 주의하며 마이그레이션 가이드를 확인하세요

