---
publish: true
tags: [지식, 디자인패턴]
---

# MVC 패턴

MVC 패턴에서 각 구성 요소가 어떤 역할을 하고 어떻게 연결되는지 명확히 이해하면 개발 구조가 더 명확해집니다.

---

## MVC 각 구성 요소

### M - Model

* **데이터 및 비즈니스 로직을 담당**
* DB와의 통신, 데이터 검증, 상태 관리 등
* 예: 사용자 정보, 게시글, 댓글 등 실제 "내용"

### V - View

* **사용자에게 보여지는 화면 (UI)**
* 템플릿, HTML, 프론트 컴포넌트 등

### C - Controller

* **요청을 받아 로직을 처리하고, 결과를 View에 전달**
* 사용자 입력 → Model → View로 연결되는 **중개자**

---

## 흐름 구조 (요청 흐름 기준)

```
사용자 요청 (예: URL 클릭, 버튼 클릭)
        ↓
   Controller가 요청 받음
        ↓
   필요한 데이터는 Model에서 조회
        ↓
   가공된 데이터 → View로 전달
        ↓
   View는 사용자에게 화면으로 렌더링
```

즉,

> **C → M → V**
> Controller가 Model을 통해 데이터를 가져오고, 그 결과를 View에 전달합니다.

---

## 그림으로 표현하면

```
[User Action]
      ↓
[Controller] ──> [Model]
      ↓              ↓
   [View]   <── Data 처리
      ↓
[Browser Output]
```

---

## 예시 (Express.js 기준)

```js
// Controller
exports.showProfile = async (req, res) => {
  const user = await UserModel.findById(req.user.id); // ← Model
  res.render('profile', { user });                    // ← View
};
```

---

## 정리

* **Model은 DB, 상태, 로직을 포함하는 진짜 '데이터 핵심'**
* **Controller는 데이터 요청·가공·View 전달 역할**
* **View는 사용자에게 보여주는 최종 결과**

즉, **C는 V로 보내지만, 그 전에 M에서 데이터를 가져옵니다.**
M은 "백엔드 깊숙한 곳에" 있는 데이터 계층이라고 보면 됩니다.
