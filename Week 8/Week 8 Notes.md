# 📘 MAD 2 Week 8 — Notes


## Table of Contents

0. [Week 8 Overview](#0-week-8-overview)
1. [Web Application Architecture Review](#1-web-application-architecture-review)
2. [APIs — Purpose & Design Philosophy](#2-apis--purpose--design-philosophy)
3. [Data-Oriented API Design](#3-data-oriented-api-design)
4. [URL & Resource Design](#4-url--resource-design)
5. [HTTP Methods for APIs](#5-http-methods-for-apis)
6. [JSON & API Response Design](#6-json--api-response-design)
7. [Authentication & Authorization](#7-authentication--authorization)
8. [Token-Based Authentication & JWT — Concepts](#8-token-based-authentication--jwt--concepts)
9. [Flask API + Authentication — Course Evolution](#9-flask-api--authentication--course-evolution)
10. [Flask-JWT-Extended — Practical Implementation](#10-flask-jwt-extended--practical-implementation)
11. [Vue.js 2 + Flask Integration](#11-vuejs-2--flask-integration)
12. [REST — Architecture Style & Its Limitations](#12-rest--architecture-style--its-limitations)
13. [GraphQL](#13-graphql)
14. [REST vs GraphQL](#14-rest-vs-graphql)
15. [API Versioning](#15-api-versioning)
16. [Markup Alternatives](#16-markup-alternatives)
17. [HTML, Markdown, XML — Structured & Text Markup](#17-html-markdown-xml--structured--text-markup)
18. [Pandoc](#18-pandoc)
19. [JAM Approach / JAMstack](#19-jam-approach--jamstack)
20. [Static Site Generators & Hydration](#20-static-site-generators--hydration)
21. [⚠️ 2022 → 2026 Modern Update Boxes](#21-2022--2026-modern-update-boxes)
22. [⚠️ Common Confusions](#22-common-confusions)
23. [🔥 Week 8 Exam Revision](#23-week-8-exam-revision)

---

## 0. Week 8 Overview

Week 8 do bade halves mein divide hota hai:

1. **Backend/API design half:** API design principles, REST, HTTP verbs, JSON, authentication, JWT, Flask implementation.
2. **Frontend integration + architecture half:** Vue.js 2 ko Flask ke saath integrate karna, GraphQL (REST ka alternative), Markup alternatives, aur JAM/JAMstack approach — jo pura web app development ko ek unified story mein pirota hai: **Storage (API) + Logic (JS) + Presentation (Markup)**.

```text
Web Application
      ↓
Client + Server
      ↓
API Design (REST conventions)
      ↓
HTTP Verbs & Status Codes
      ↓
Authentication (Token/JWT)
      ↓
Flask API implementation
      ↓
Vue.js 2 + Flask integration
      ↓
REST limitations → GraphQL
      ↓
Markup alternatives (HTML/Markdown)
      ↓
JAM Approach → JAMstack → SSG → Hydration
```

---

## 1. Web Application Architecture Review

**Definition:**
A web application architecture is the structural design that defines how the client (browser), server, database, and business logic communicate with one another to deliver a working application.



### Big-Picture Architecture Diagram

```text
┌───────────────┐        HTTP Request        ┌───────────────┐
│   Client       │ ─────────────────────────▶ │   Server       │
│ (Browser/Vue)  │                              │ (Flask API)    │
│                │ ◀───────────────────────── │                │
└───────────────┘        JSON Response        └───────┬────────┘
                                                        │
                                                        │ Business Logic
                                                        ▼
                                                ┌───────────────┐
                                                │   Database     │
                                                │ (SQL/NoSQL)    │
                                                └───────────────┘
```

**Diagram Explanation:** Client (browser ya Vue app) HTTP request bhejta hai → Flask server request receive karta hai, business logic execute karta hai → zaroorat padne par database se data fetch/update karta hai → aakhir mein client ko JSON response wapas milta hai. 

---

## 2. APIs — Purpose & Design Philosophy

**Definition:**
An API (Application Programming Interface) is a set of functions exposed by a server that can be called remotely, typically over HTTP, to retrieve or modify data.


### Why?

API ka purpose khud ek complete application hona nahi hai — API sirf ek **set of functions** hai jise **developers** use karenge apni khud ki application banane ke liye. Isliye API design **developer-centric** hona chahiye, end-user-centric nahi.

**Historical root:** API modern **RPC (Remote Procedure Call)** ka evolution hai — jisme client A remotely server B pe koi function call karta hai. Web APIs ne HTTP ko RPC transport ke roop mein choose kiya kyunki HTTP text-based, simple, aur widely supported hai.

### RPC → Web API Diagram

```text
Client A                                   Server B
   │                                            │
   │  1. Call function on B with arguments      │
   │ ────────────────────────────────────────▶ │
   │     (encoded as HTTP request:               │
   │      URL + method + params/body)            │
   │                                            │
   │                                       2. B executes
   │                                          the actual
   │                                          function
   │                                            │
   │  3. Response with result                   │
   │ ◀──────────────────────────────────────── │
   │     (encoded as HTTP response:               │
   │      JSON body + status code)                │
```



> 🎯 **Exam Point:** API ka fundamental purpose = "run a function on a remote server with parameters, and get a response back" — ye definition hamesha yaad rakhna.

---

## 3. Data-Oriented API Design

**Explanation:**
Ek accha approach hai **data-oriented thinking**: apni application ko **entities**, **actions**, aur **summaries** mein todo.

| Category | Meaning | Example (Student Grade Book) |
|---|---|---|
| **Entities** | Nouns/objects in the system | Students, Courses, Grades |
| **Actions** | CRUD operations that modify entities | Add, Edit, Delete |
| **Summaries** | Read-only aggregated queries | List of students, GPA, Top students |
| **Exotic queries** | Complex, unplanned, ad-hoc queries | "Students whose name starts with 'A' who scored >85%..." |

> 💡 **Easy Way to Remember:** Entities = **nouns**, Actions = CRUD **verbs**, Summaries = read-only derived data, Exotic = unpredictable complex queries (jo REST ke liye hard hote hain — GraphQL ki motivation yahin se aati hai).

---

## 4. URL & Resource Design

### Historical / Naive Approach (function-name-mapped URLs)

```text
http://localhost/getListOfStudents
http://localhost/getStudent?id=xyz
http://localhost/createNewStudent
http://localhost/editStudent?id=xyz
```

**Definition:** Ye approach directly backend function names ko URL endpoints mein map karta hai. Ye "fundamentally wrong" nahi hai, lekin **remember karna, document karna aur understand karna hard** hota hai — especially jab functions complex ho jaate hain (`getTopStudents`, `createStudentAndAddToCourse`).

### Convention-Based (RESTful) Approach — Recommended

| Bad (verb in URL) | Good (noun + HTTP method) |
|---|---|
| `GET /getStudents` | `GET /students` |
| `POST /createStudent` | `POST /students` |
| `PATCH /editStudent/123` | `PATCH /students/123` |

### URL Conventions

**Nouns good, verbs bad:**
> **Definition:** In REST-style URL design, resource paths should represent entities (nouns), while the intended action is conveyed through the HTTP method (verb), not embedded in the URL text.

- `/student`, `PATCH /student/123` ✅
- `/create/student`, `/edit/student/123` ❌

**Well-known / discoverable URLs:** API ko crawl karke discover kiya ja sake — jaise `GET /` par ek base response mile jo aage ke useful endpoints ki links de (GitHub API isi pattern ka example hai).

**Permalinks:**
> **Definition:** A permalink is a unique, stable identifier-based URL for a resource that does not change even if the resource's human-readable attributes (like username or title) change.

Example: Google Docs URLs (`docs.google.com/presentation/d/1r3scu9...`) human-readable nahi hote, lekin **permanent** rehte hain chahe document ka title/owner change ho jaaye.

### Path Parameter vs Query Parameter

| Feature | Path Parameter | Query Parameter |
|---|---|---|
| Example | `/students/123` | `/students?course=123` |
| Use case | Identify a specific resource | Filter/search across a collection |
| Style preference | Structured, developer-preferred | Also valid, but less "readable" for nesting |

```text
GET /students/123          → individual student
GET /students?course=123   → filtered list of students
POST /students             → create new student
PATCH /students/123        → update existing student
DELETE /students/123       → delete student
```

> 🎯 **Exam Point:** `/course-123-students` bhi technically kaam karega (string as-is server ko parse karna hi hota hai), lekin structured (`/course/123/students`) developer experience ke liye better convention hai — ye "correctness" ka issue nahi, "convention" ka issue hai.

### URL Anatomy Diagram

```text
   https://api.example.com / students / 123 ? course=45
   └──────────┬───────────┘└────┬────┘└─┬─┘ └────┬─────┘
        Base/Server URL      Resource   Path    Query
                              (noun)   Parameter Parameter
                                        (which     (filter/
                                         student)   search)
```

**Diagram Explanation:** Base URL server ko identify karta hai; resource path entity/noun batata hai (`students`); path parameter (`123`) specific resource identify karta hai; aur query parameter (`?course=45`) filtering/searching ke liye use hota hai. 

---

## 5. HTTP Methods for APIs

| Method | Purpose | Cacheable? | Idempotent? | Notes |
|---|---|---|---|---|
| **GET** | Read data/lists | ✅ Yes (data URL mein hoti hai) | ✅ Yes | Should never modify server state (convention) |
| **POST** | Create new object/data | ❌ No (data request body mein hoti hai, cache index ka part nahi) | ❌ No | Can technically also be used for reads, but convention says avoid |
| **PUT** | Replace resource with entirely new data | — | ✅ Yes | Full update |
| **PATCH** | Partial/incremental update | — | Usually treated as idempotent | **Preferred** over PUT for partial updates |
| **DELETE** | Remove resource | — | ✅ Yes | |



### GET Caching vs POST (Diagram)

```text
GET /students/123  (1st time)
   Client ──────────────▶ Server ──▶ DB ──▶ Response
                                              │
                                              ▼
                                       Cached by URL

GET /students/123  (2nd time, same URL)
   Client ──────────────▶ Cache ──▶ Response (DB not hit again!)

POST /students  (create — always hits server + DB)
   Client ──────────────▶ Server ──▶ DB ──▶ Response
                          (never served from cache)
```

**Diagram Explanation:** GET requests same URL ke liye repeatable/predictable hoti hain (agar DB nahi badla), isliye cache directly serve kar sakta hai — DB tak jaane ki zaroorat nahi. POST ka data request body mein hota hai (URL mein nahi), isliye cache index ka hissa nahi ban sakta — har POST hamesha server/DB tak jaata hai.

---

## 6. JSON & API Response Design

### Output Format Comparison

| Format | Structure Handling | Human Readability | Verdict (per source) |
|---|---|---|---|
| **XML** | Very good — hierarchies, types clearly expressible | Verbose | Often overkill for simple APIs |
| **JSON** | Limited data types, no native type system | High (human-readable, easy to write/parse) | **Preferred format at present** — but "not necessarily best possible" (source explicitly notes this may change over time) |



```json
{
  "login": "nchandra75",
  "id": 3119001,
  "url": "https://api.github.com/users/nchandra75",
  "followers_url": "https://api.github.com/users/nchandra75/followers",
  "gists_url": "https://api.github.com/users/nchandra75/gists/{gist_id}"
}
```

> 🎯 **Exam Point:** Curly braces (`{gist_id}`) ka matlab hai ye ek **template parameter** hai jo URL ka literal part nahi — client ko usko fill karke actual request banani hoti hai.

---

## 7. Authentication & Authorization

### Authentication vs Authorization

| Feature | Authentication | Authorization |
|---|---|---|
| Question | "Who are you?" | "What are you allowed to do?" |
| Example | Login with username/password | Admin-only access to a route |
| Comes first? | ✅ Always first | Depends on authentication result |

**Definition (Authentication):** The process of verifying the identity of a user or system.
**Definition (Authorization):** The process of determining whether an authenticated identity has permission to perform a specific action or access a resource.

### Standard Techniques (per source)

- **OAuth2** — widely-used standard for token-based authentication/authorization.
- **JWT (JSON Web Token)** — a specific, self-contained token format that can carry identity + claims.

> ⚠️ **Important (source's own advice):** "Use standard techniques where possible" — rolling your own authentication scheme is risky; well-studied standards like OAuth2/JWT have had extensive security review.

> ⚠️ **Common Confusion:** JWT ≠ Authentication itself. JWT sirf ek **token format/mechanism** hai jisse authentication state ko carry kiya jaata hai — authentication ka broader process (credential verification, session/token issuance) alag concept hai.

### Authentication → Authorization Flow

```text
User submits credentials
         │
         ▼
┌─────────────────────┐
│  AUTHENTICATION       │   "Who are you?"
│  (verify identity)    │
└─────────┬─────────────┘
          │ Identity confirmed
          ▼
┌─────────────────────┐
│  AUTHORIZATION        │   "What can you do?"
│  (check permissions)  │
└─────────┬─────────────┘
          │
   ┌──────┴──────┐
   ▼             ▼
Allowed       Denied
(200 OK)      (403 Forbidden)
```



---

## 8. Token-Based Authentication & JWT — Concepts

### Why Token-Based over Cookie-Based?

**Explanation:**
Cookie-based authentication tab tak simple hai jab tak frontend aur backend **same domain/origin** pe hain. Agar Vue frontend aur Flask backend **alag domains** pe deploy ho rahe hain (jaisa Week 8 ke Vue+Flask Part II mein dikhaya gaya), to cookies cross-origin properly kaam nahi karti — isliye **Authorization header mein token bhejna** zyada reliable aur flexible approach hai.

```text
Authorization: Bearer <token>
```

### JWT Structure

**Definition:** A JWT (JSON Web Token) is a compact, URL-safe token format consisting of three Base64Url-encoded parts separated by dots: Header, Payload, and Signature.

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9   ← Header (algorithm + type)
.eyJmcmVzaCI6ZmFsc2UsImlhdCI6...        ← Payload (claims: identity, expiry etc.)
.QId_dQSzyTZ146-_2qFDVC6f_iovvzeVDNc    ← Signature (verifies integrity)
```



- **Header:** Algorithm (e.g., HS256) aur token type specify karta hai.
- **Payload:** Claims hoti hain — jaise `sub` (subject/identity), `exp` (expiry), `iat` (issued at).
- **Signature:** Server ke secret key se generate hoti hai; isse verify hota hai ki token tamper nahi hua.

### Login → Protected Request Flow

```text
User
  |
  | Login (username + password)
  ↓
Flask
  |
  | Verify credentials against DB
  ↓
Create JWT (access token)
  |
  ↓
Client (stores token — e.g., localStorage/memory)
  |
  | Authorization: Bearer <token>
  ↓
Protected API
  |
  | Verify JWT signature + expiry
  ↓
Allow (return data) / Reject (401)
```

> 🎯 **Exam Point:** JWT **stateless** hai — server ko session store maintain karne ki zaroorat nahi, kyunki poori identity information token ke andar hi encoded hoti hai (signed, encrypted nahi — isliye sensitive data payload mein directly mat daalo).

---

## 9. Flask API + Authentication — Course Evolution





Is lecture (`Flask API – Token Based Authentication`) mein professor **Flask-Security-Too** library use karte hain (Flask-Security ka maintained fork, kyunki original Flask-Security ab maintain nahi ho raha tha):

- Login pe cookie-based session milti hai by default (`login_required` decorator).
- Token-based auth ke liye `fs_uniquifier` column add karna padta hai `User` model mein.
- Endpoint: `POST /login?include_auth_token=true` — response mein ek opaque `authentication_token` milta hai.
- Protected routes pe `login_required` ki jagah `auth_token_required` decorator use hota hai.
- **Important self-observation from the lecture itself:** Professor khud clarify karte hain ki ye ek **simple/opaque token** hai, JWT nahi — aur JWT tab useful hota hai jab **alag systems** token generate aur validate karte hon (distributed systems). Single-system case (jahan same Flask app token generate bhi karta hai aur validate bhi) ke liye simple token "more than enough" hai.

### Current Course/Live-Class Approach: Flask-JWT-Extended — real JWT

Aapki **live class material** (`app.py`, `flask-jwt.md`, `requirements.txt`) is se aage badh kar gaya hai aur **`flask_jwt_extended`** library use karta hai — jo real, standards-compliant JWT implement karta hai. Ye zyada modern aur widely-adopted approach hai.

| Feature | Flask-Security-Too (lecture) | Flask-JWT-Extended (live class) |
|---|---|---|
| Token type | Custom opaque token (`fs_uniquifier`-based) | Real JWT (header.payload.signature) |
| Decorator | `@auth_token_required` | `@jwt_required()` |
| Identity access | Manual query using token | `current_user` (via `user_lookup_loader`) |
| Library maintenance status | Flask-Security discontinued → forked as Flask-Security-Too | Actively maintained, very popular for pure API/JWT use cases |



**Exam Focus:** Agar exam mein "token based auth in Flask" pucha jaaye, dono approaches ka concept samajhna zaroori hai, lekin **`flask_jwt_extended` wala flow practically zyada important hai** kyunki wahi aapke live-class code mein implement hua hai.

---

## 10. Flask-JWT-Extended — Practical Implementation

### Request Flow

```text
INPUT (username, password)
  ↓
REQUEST → POST /api/login
  ↓
ROUTE → login()
  ↓
BACKEND LOGIC → verify credentials against DB
  ↓
DATABASE → User table (SQLAlchemy)
  ↓
RESPONSE → { "access_token": "<jwt>" }
  ↓
FRONTEND → stores token, sends in future requests
```

### Code Walkthrough (from `app.py` / `flask-jwt.md`)

```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_jwt_extended import JWTManager, create_access_token, jwt_required, current_user

app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///jwt-db.sqlite3"
app.config["JWT_SECRET_KEY"] = "jwt-secret-key-for-flask"

db = SQLAlchemy(app)
jwt = JWTManager(app)
app.app_context().push()
```

**Line-by-Line Explanation:**
- `JWT_SECRET_KEY` — server ki private key jisse tokens **sign** (encrypt nahi, sign) kiye jaate hain. Isse guess karna asaan nahi hona chahiye.
- `JWTManager(app)` — Flask app ke andar JWT support enable karta hai.

```python
@jwt.user_identity_loader
def load(user):
    return user.username
```
**Explanation:** Ye decorator define karta hai ki JWT ke `sub` (subject) claim mein **kya store hoga** jab token banaya jaata hai — yahan `username` store hota hai (poora user object nahi, kyunki JSON-serializable value chahiye).

```python
@jwt.user_lookup_loader
def user_lookup(_jwt_header, jwt_data):
    identity = jwt_data["sub"]
    return User.query.filter_by(username=identity).one_or_none()
```
**Explanation:** Jab bhi koi protected route hit hoti hai valid token ke saath, ye function token ke `sub` claim se username nikaal kar **database se poora User object fetch** karta hai — isi se `current_user` variable populate hoti hai.

```python
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(300), unique=True, nullable=False)
    password = db.Column(db.String(300), nullable=False)
```



```python
@app.route("/api/login", methods=['POST'])
def login():
    creds = request.get_json()
    user = User.query.filter_by(username=creds["username"]).one_or_none()

    if not user or not creds["password"] == user.password:
        return {"message": "Invalid Credentials"}, 401
    else:
        access_token = create_access_token(identity=user)
        return jsonify(access_token=access_token)
```

**Request:**
```http
POST /api/login
Content-Type: application/json

{
  "username": "student",
  "password": "..."
}
```

**Response (success):**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs..."
}
```

**Response (failure):** `401 Unauthorized`, `{ "message": "Invalid Credentials" }`

```python
@app.route("/api/dashboard")
@jwt_required()
def home():
    return jsonify(username=current_user.username, password=current_user.password)
```

**Explanation:**
- `@jwt_required()` — decorator jo check karta hai ki incoming request ke `Authorization: Bearer <token>` header mein ek **valid, non-expired JWT** hai; agar nahi, to automatically `401` return karta hai.
- Valid hone par `current_user` (jo `user_lookup_loader` se populate hoti hai) route function ke andar directly accessible ho jaata hai.

### Dry Run — Full Flow

| Step | Action | Result |
|---|---|---|
| 1 | Client → `POST /api/login` with `{username, password}` | — |
| 2 | Server queries DB for matching user | User found / not found |
| 3 | Password match check | Pass → generate JWT / Fail → 401 |
| 4 | `create_access_token(identity=user)` calls `user_identity_loader` | Token's `sub` = `user.username` |
| 5 | Client stores token | e.g. in memory / localStorage |
| 6 | Client → `GET /api/dashboard` with `Authorization: Bearer <token>` | — |
| 7 | `@jwt_required()` verifies signature + expiry | Valid → proceed / Invalid → 401 |
| 8 | `user_lookup_loader` extracts `sub`, queries DB | `current_user` populated |
| 9 | Route returns `current_user.username`, etc. | 200 OK with JSON |

### Security Considerations

> 🎯 **Exam Focus:**
- `JWT_SECRET_KEY` production mein environment variable se aani chahiye, code mein hardcoded nahi.
- Passwords hamesha hashed store hone chahiye.
- CORS (`flask_cors`) enable karna zaroori hai jab frontend aur backend alag origins pe ho (jaise `flask-jwt.md` mein `CORS(app)` import hua hai).
- Token expiry (`exp` claim) set karna zaroori hai taaki stolen token forever valid na rahe.

---

## 11. Vue.js 2 + Flask Integration



Course do alag architectural patterns dikhata hai:

### Pattern A: Vue as a library inside Flask/Jinja (hybrid rendering)

**Definition:** In this pattern, Vue.js 2 is loaded as a regular static JS library alongside a server-rendered Jinja template, and is used only for specific interactive "islands" of the page — not the entire application.

```text
Flask App
  ├── Jinja Templates  → rendered server-side, sent as HTML
  ├── Static Files      → JS, CSS, images + Vue.js 2 library
  └── APIs              → called via fetch/AJAX from within Vue components
```

**Setup steps (per Vue 2 official documentation pattern):**

1. Vue 2 ko `static/vue/` folder mein CDN se download karke ya `<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js"></script>` se load karo.
2. Jinja template ke andar Vue mount-point banao:

```html
<div id="app">
  <p>{{ '{{ msg }}' }}</p>
</div>
<script src="{{ url_for('static', filename='vue/vue.min.js') }}"></script>
<script src="{{ url_for('static', filename='js/app.js') }}"></script>
```

3. `app.js` mein actual Vue 2 instance banao:

```javascript
new Vue({
  el: '#app',
  data: {
    msg: 'Hello World from Vue'
  }
})
```

### The Jinja vs Vue Delimiter Conflict

**Problem:** Dono Jinja aur Vue 2 same delimiter use karte hain: `{{ variable }}`. Isliye jab Flask server pehle Jinja template render karta hai, wo `{{ msg }}` ko **pehle hi** apni khud ki (null) value se replace karne ki koshish karta hai — result: Vue ko kabhi actual template text milta hi nahi.

**Solution 1 — Jinja `raw`/`endraw` escaping:**

```html
{% raw %}
<p>{{ msg }}</p>
{% endraw %}
```

Ye Jinja ko batata hai ki is block ko **as-is, unprocessed** client ko bhej do — sirf tab Vue 2 ise apni templating engine se process karega.

**Solution 2 — Custom Vue 2 delimiters (Recommended, cleaner):**

Per **Vue 2 official documentation**, `delimiters` option `Vue.config` ya component-level pe set kiya ja sakta hai:

```javascript
Vue.config.delimiters = ['${', '}']

new Vue({
  el: '#app',
  data: {
    msg: 'Hello World from Vue'
  }
})
```

```html
<p>${ msg }</p>
```

**Explanation:** Kyunki `${ }` Jinja ke liye koi special meaning nahi rakhta, Jinja ise as-is pass kar deta hai, aur Vue 2 apne custom delimiter se ise correctly interpolate kar leta hai — bina `raw`/`endraw` ke bhi.

> 💡 **Easy Way to Remember:** Delimiters approach zyada **readable** hai — jab bhi aap `$` se shuru hone wali variable dekhoge, aap turant jaan jaoge ki ye **client-side (Vue) rendered** hai, aur normal `{{ }}` **server-side (Jinja) rendered** hai.

### Advantages / Disadvantages of Pattern A (per source)

| Advantages | Disadvantages |
|---|---|
| Simple — Vue sirf jahan zaroori ho waha use hota hai | Har Jinja template mein manually Vue import karna padta hai (ya base template banani padti hai) |
| Deployment simple — sab kuch ek hi project mein | Vue CLI tooling (build optimization, hot reload) available nahi |
| Development ek hi codebase mein — chhoti team ke liye easy | — |
| **Cookie-based authentication** use kar sakte ho (kyunki same-origin) | — |
| Server-side aur client-side rendering mix-and-match kar sakte ho | — |

**Best suited for:** Simple apps, small teams, gradually migrating a traditional Jinja app, ya jahan sirf kuch widgets/parts ko interactive banana ho (poori app ko SPA banane ki zaroorat nahi).

### Pattern B: Fully Separated Frontend (Vue 2 SPA) + Backend (Flask API)

**Definition:** In this pattern, the Vue.js 2 frontend and Flask backend are built, run, and deployed as completely independent applications, communicating purely through a JSON REST API — typically across different domains/origins.

```text
Frontend (Vue.js 2 SPA)        Backend (Flask REST API)
  api.example.com  ←────JSON────→  example.com (or reverse)
```

**Key characteristics:**
- Server-side rendering **nahi** hoti — sab kuch client-side render hota hai.
- Cookie-based auth kaam nahi karta reliably (different domains), isliye **token-based (JWT) authentication** use hoti hai.
- Backend sirf JSON data deta hai — koi HTML render nahi karta.

**Setting up (per Vue 2 CLI conventions, matching the lecture's demo):**

```bash
vue create my-first-app     # scaffold using Vue CLI (Vue 2 template)
cd my-first-app
npm run serve                # dev server, e.g. localhost:8080
```

**Fetching data from Flask backend inside a Vue 2 component (Options API `mounted` lifecycle hook):**

```javascript
export default {
  name: 'HelloWorld',
  data () {
    return {
      message: ''
    }
  },
  mounted () {
    fetch('http://localhost:8080/api/test')
      .then(res => res.json())
      .then(data => {
        this.message = data.message
      })
  }
}
```

**Line-by-Line Explanation:**
- `mounted()` — Vue 2 ka ek **lifecycle hook** hai jo component ke DOM mein mount hone ke turant baad chalta hai. API calls yahan karna standard practice hai kyunki component ready hota hai.
- `fetch(...)` — browser ka native Fetch API; Vue-specific nahi hai, lekin Vue ke reactive `data` properties ke saath easily combine ho jaata hai.
- `this.message = data.message` — response aane par reactive `message` property update hoti hai, jisse template automatically re-render ho jaata hai (Vue 2 ki reactivity system ki wajah se).

### Build & Deployment (per Vue 2 CLI conventions)

```bash
vue build main.js     # (historical alias used in lecture)
# Modern equivalent per Vue CLI docs:
npm run build          # generates production build in dist/
```



**Deploying the built SPA from within Flask (merging the two patterns):**

```python
app = Flask(__name__, static_folder="dist", static_url_path="/app")

from flask import redirect

@app.route("/")
def home():
    return redirect("/app/index.html")
```

**Explanation:** Vue 2 build ke `dist/` output ko Flask ke static folder ke roop mein serve kiya jaa sakta hai — is case mein wapas se cookie-based auth possible ho jaata hai (kyunki dono same server/origin pe aa jaate hain), lekin code abhi bhi **separately develop** hota hai.

> ⚠️ **Important:** Agar `dist` ka default public path change karna ho (jaise `/app` sub-path se serve karna), to Vue CLI project ki `vue.config.js` mein `publicPath` option set karna padta hai — warna built CSS/JS files galat relative paths se load hone ki koshish karenge.

```javascript
// vue.config.js
module.exports = {
  publicPath: '/app/'
}
```

### Comparison: Hybrid (Pattern A) vs Fully Separated (Pattern B)

| Feature | Pattern A: Vue-in-Jinja | Pattern B: Separate Vue 2 SPA + Flask API |
|---|---|---|
| Rendering | Hybrid (server + client) | Pure client-side |
| Auth | Cookie-based (simpler) | Token/JWT-based (required cross-origin) |
| Deployment | Single deployable unit | Can be separate, or merged (dist → Flask static) |
| Tooling | No Vue CLI benefits | Full Vue CLI (build optimization, hot reload, npm ecosystem) |
| Best for | Small teams, partial interactivity, gradual migration | Larger teams, full SPA, independent frontend/backend scaling |

> 🎯 **Exam Focus:** Ye do patterns ka **trade-off table** likhna aana chahiye — likely conceptual/long question.

**Source Coverage:** How to integrate Vue with Flask – Part I & II.

---

## 12. REST — Architecture Style & Its Limitations

**Definition:**
REST (Representational State Transfer) is an architectural style for designing networked applications, characterized by principles such as statelessness, a uniform interface, cacheability, and a client-server separation — it is not a rigid API specification.

**Explanation:**
Bahut saari "RESTful" APIs actually REST ke kisi na kisi constraint ko violate karti hain — kyunki **REST ek architecture style hai, ek exact rulebook nahi**. Sometimes rules thoda "bend" karna hi practically better API design deta hai.

### Problems with REST (per source)

1. **"Chatty" APIs:** Ek complete view banane ke liye multiple sequential requests lagti hain — jaise pehle student details, phir unke courses ki list, phir har course ki details, phir aggregate marks.
2. **No general query language:** REST sirf specific, predefined types ki requests allow karta hai — client "what is needed" directly specify nahi kar sakta; usse har step manually construct karna padta hai.

### "Chatty" REST Problem — Diagram

```text
Client                                            Server
  │──── GET /students/123 ─────────────────────▶  │  (1) get student
  │◀─────────── student details ─────────────────  │
  │                                                 │
  │──── GET /students/123/courses ─────────────▶   │  (2) get course list
  │◀─────────── list of courses ─────────────────  │
  │                                                 │
  │──── GET /courses/45 ───────────────────────▶   │  (3) get course #1 details
  │◀─────────── course details ──────────────────  │
  │                                                 │
  │──── GET /courses/45/marks ─────────────────▶   │  (4) get aggregate marks
  │◀─────────── marks data ──────────────────────  │
```

**Diagram Explanation:** Ek hi "student profile with GPA" view banane ke liye **4 alag sequential round-trips** lag rahe hain — har request ka response agla request decide karta hai. Yehi "chattiness" hai jo REST ka practical limitation hai, aur GraphQL isi problem ko **ek hi request** mein solve karne ki koshish karta hai (dekhein Section 13).

> ⚠️ **Common Confusion:** REST ≠ CRUD. REST sirf CRUD operations tak limited nahi hai — ye ek broader architectural philosophy hai (statelessness, uniform interface, caching, layered system) jo CRUD se bhi bade concepts cover karti hai.

---

## 13. GraphQL

**Definition:**
GraphQL is a query language for APIs (and a server-side runtime for executing those queries) that lets clients request exactly the data they need — no more, no less — through a single, declarative query, typically sent as a POST request.

### Why GraphQL? (motivations, per source)

1. **REST is endpoint-based** — complex/arbitrary queries (jaise "students named 'A%' aged >25") ko multiple GET requests ya special-character-heavy URLs mein force karna padta hai, jo fragile hota hai.
2. **Multiple data sources** — modern apps ko ek saath kai sources se data chahiye hota hai (DB + weather API + user auth service, etc.) — client ya server, kaun fuse kare?
3. **Declarative programming** — jaise Vue mein hum "what" specify karte hain "how" nahi, waise hi data-fetching mein bhi declarative approach useful ho sakta hai.

### How GraphQL Works

```text
Client
   |
   | GraphQL Query (POST body)
   ↓
GraphQL Server (e.g., Apollo Server)
   |
   +---- Resolver → Database
   |
   +---- Resolver → REST API
   |
   +---- Resolver → Other Service
   |
   ↓
Fused, filtered data
   |
   ↓
Client (gets exactly what it asked for)
```

**Definition (Resolver):** A resolver is a function on the GraphQL server responsible for fetching the actual data for a specific field in a query, from whatever backend source is appropriate.

### Type System

**Explanation:**
GraphQL ek explicit **type system** deta hai — String, Int, Collections, aur relationships bhi specify kar sakte ho (e.g., `Student -> [Course]`, matlab ek student ke paas courses ki ek list ho sakti hai). Ye Python jaisi dynamically-typed languages se contrast karta hai jahan types runtime pe determine hoti hain.

**Advantage:** Server queries ko validate kar sakta hai type mismatch ke liye, before hi execution ke.

### Mutation

**Definition:** In GraphQL, a mutation is the mechanism for performing Create, Update, or Delete operations — i.e., any query that alters the underlying data store.

### GraphQL API Evolution / Versioning

**Explanation:**
GraphQL queries JSON-jaisi hoti hain, jinhe object mein naye keys add karna ya purane keys deprecate karna easy hota hai — isliye **naya API version banane ki zaroorat aksar nahi padti**. Purane clients purani information continue receive karte rehte hain, naye clients naya field request kar sakte hain.

### Tools

- **Apollo Server** — JavaScript-based system to build a GraphQL server, multiple backends se connect karne ke liye.
- **Explorers** (GitHub GraphQL Explorer, graphql.org playground) — dynamically queries construct/test karne ke liye.

### GraphQL — Important Caveats (source explicitly emphasizes)

> ⚠️ **Important:** GraphQL **backend complexity automatically kam nahi karta** — actually kabhi-kabhi server-side complexity **badh** sakti hai, kyunki resolvers likhna, multiple sources se data fuse karna, aur filtering server pe implement karna padta hai. Ye trade-off hai: **network traffic kam, server load potentially zyada.**

> ⚠️ **Caching Challenge:** URL-level HTTP caching GraphQL mein possible nahi hai (kyunki sab kuch ek hi POST endpoint pe jaata hai) — caching ke liye custom, application-level strategies chahiye hoti hain.

**Source Coverage:** Introduction to GraphQL.

---

## 14. REST vs GraphQL

| Feature | REST | GraphQL |
|---|---|---|
| Endpoint structure | Multiple endpoints (one per resource) | Single endpoint, flexible query |
| Data fetching | Fixed shape per endpoint | Client specifies exact fields needed |
| Over-fetching | Common (get all fields even if unneeded) | Avoided by design |
| Under-fetching | Common (multiple requests for related data) | Avoided — nested queries in one request |
| Request count for complex views | Often many ("chatty") | Usually one |
| Type system | Not built-in | Built-in, strongly typed schema |
| Caching | Native HTTP caching via GET + URLs | Harder — no URL-based caching |
| Backend complexity | Comparatively simpler | Can increase (resolvers, fusion logic) |
| Query language | No — fixed set of endpoints | Yes — declarative query language |

### When REST is better
Simple CRUD apps, jab caching important ho, jab endpoints predictable ho aur query flexibility ki zaroorat na ho.

### When GraphQL is better
Complex, nested, multi-source data requirements; jab clients (mobile vs web) ko different subsets of data chahiye ho; jab over/under-fetching ek real performance problem ban raha ho.

### When NOT to use GraphQL
Simple apps jinhe sirf basic CRUD chahiye — GraphQL ka setup overhead (schema, resolvers) unnecessary complexity add kar dega.

### Visual Comparison — Same Task (Get student + their courses + marks)

```text
REST (multiple round-trips — "chatty")
Client ─▶ GET /students/123          ─▶ Server
Client ─▶ GET /students/123/courses  ─▶ Server
Client ─▶ GET /courses/45            ─▶ Server
Client ─▶ GET /courses/45/marks      ─▶ Server
   (4 separate requests, over-fetches unneeded fields each time)

GraphQL (single round-trip — declarative)
Client ─▶ POST /graphql
          {
            student(id: 123) {
              name
              courses { title, marks }
            }
          }
      ◀── exactly the requested fields, nothing more
   (1 request, server internally fuses everything)
```

**Diagram Explanation:** REST mein client ko pata hona chahiye ki kaunse endpoints kis order mein call karne hain; GraphQL mein client sirf "mujhe ye shape ka data chahiye" bolta hai aur server internally saara data-fetching/fusion khud sambhalta hai — lekin is convenience ki keemat hai server-side complexity (jaise Section 13 mein discuss kiya gaya).

---

## 15. API Versioning

**Explanation (REST context):**
REST APIs mein versioning aksar zaroori hoti hai jab **breaking changes** (jo purane clients ko todh de) karne ho. Common strategies: URL versioning (`/v1/students`), header-based versioning, query-parameter versioning. Non-breaking changes (jaise naya optional field add karna) ke liye usually version badalne ki zaroorat nahi.

**GraphQL context (per source):** Kyunki GraphQL queries JSON-object-jaisi flexible hoti hain, naye fields add karna aur purane ko "deprecated" mark karna easily possible hai bina naya version banaye — ye claim source mein explicitly documented hai aur GraphQL ki design philosophy se consistent hai.

---

## 16. Markup Alternatives

### Why HTML?

**Explanation:**
HTML ek **"living standard"** hai — matlab continuously evolve hoti rehti hai, extensible hai (Web Components, custom JS-enabled tags), aur "semantic" content pe focus karta hai jabki styling CSS ko sonp deta hai.

### Why Not HTML?

- **Structured data communication** ke liye HTML designed nahi hai — isliye JSON/XML use hote hain.
- Naye environments (VR/AR) ke liye HTML best fit nahi ho sakta — jaise historical **VRML** (1995) aur uska successor **X3D**.
- Humans ke likhne ke liye still relatively **verbose** hai.

---

## 17. HTML, Markdown, XML — Structured & Text Markup

### Text-Based Markup (e.g., Markdown)

**Definition:** Text-based markup is a lightweight markup style where formatting is indicated using simple inline characters (like `**bold**`, `_italic_`, `# Heading`) within otherwise normal, readable text.

```markdown
# This is a heading
And this is a regular paragraph.
* A bullet list
* Another bullet
1. A numbered list
2. More numbers
[links](http://www.example.com/link)
```

**Alternatives:** Markdown, reStructuredText (RST — often used in Python documentation), AsciiDoc.

### Why Text-Based Formats?

- **Longevity:** ASCII/Unicode standard hai, isliye purane text files **future mein bhi readable** rehte hain — proprietary binary formats (jaise purane word-processor formats) ke opposite, jo **obsolete** ho sakte hain.
- **Compact & human-readable.**

### Limitations of Text-Based Markup

- **Structure encode karna hard** hai (e.g., tables not natively well-supported).
- **Ambiguity possible** — different parsers different interpretation kar sakte hain (unlike explicit opening/closing tags in HTML/XML jahan nesting rules clear hoti hain).
- Roman alphabet/English-centric conventions (`#`, `*`, numbered lists) — dusri languages/scripts mein directly translate nahi hote.

### HTML vs Markdown vs XML

| Feature | HTML | Markdown | XML |
|---|---|---|---|
| Purpose | Web page structure + semantics | Lightweight human-friendly text formatting | Structured data representation |
| Readability (raw) | Moderate (tags visible) | High (looks like plain text) | Low (verbose tags) |
| Ambiguity | Low (explicit tags) | Possible | Low (strict schema) |
| Typical use | Web pages | Docs, README, blogs | Data interchange, configs |

---

## 18. Pandoc

**Definition:** Pandoc is a general-purpose document conversion tool — often described as a "Swiss Army Knife" — capable of converting between a wide variety of markup and document formats (Markdown, HTML, LaTeX, PDF, DOCX, and more).

**Explanation:** Jaise ek compiler ek programming language ko dusri mein convert karta hai, Pandoc markup/document formats ke beech systematic conversion karta hai. Structured languages (jaise XML/SGML) ke beech convert karna easier hota hai unstructured text ke muqable.

---

## 19. JAM Approach / JAMstack

### What does an app need? (Foundational framing)

| Need | Handled by |
|---|---|
| **Data store** | APIs (SQL, NoSQL, GraphQL...) |
| **User Interface** | Vanilla HTML + forms, or JavaScript for interactivity |
| **Business logic** | Backend (Python, Go, NodeJS) or Frontend (JS) computation |

### JAM = JavaScript + APIs + Markup

**Definition:** JAM (later popularized as "JAMstack") refers to an architectural approach where JavaScript handles all dynamic logic/interactivity, APIs handle all data access, and Markup (pre-built HTML/Markdown) handles the presentation layer — without a traditional server-side application logic layer coupling everything together.

```text
Storage      → API (SQL / NoSQL / GraphQL)
Logic        → JavaScript
Presentation → Markup (HTML / Markdown, compiled ahead of time)
```

### JAMstack Layered Architecture Diagram

```text
┌─────────────────────────────────────────────┐
│              PRESENTATION (Markup)            │
│   Pre-built HTML / Markdown, served via CDN   │
└───────────────────┬───────────────────────────┘
                     │ hydrated by
                     ▼
┌─────────────────────────────────────────────┐
│                LOGIC (JavaScript)              │
│   Runs in browser — interactivity, fetches     │
└───────────────────┬───────────────────────────┘
                     │ calls
                     ▼
┌─────────────────────────────────────────────┐
│               STORAGE (APIs)                   │
│   REST / GraphQL / Headless CMS / any backend  │
└─────────────────────────────────────────────┘
```

**Diagram Explanation:** JAMstack mein teeno layers **decoupled** hoti hain — Markup pehle se compiled/static hoti hai aur CDN se fast serve hoti hai, JavaScript is static markup ko interactive banati hai (hydration), aur JavaScript hi APIs (kisi bhi backend — REST, GraphQL, headless CMS) se data fetch karti hai. Teeno layers independently develop/deploy/scale ho sakti hain — yehi is architecture ki flexibility hai.

### Content Management Systems (CMS) and Headless CMS

**Explanation:** Ek blog application ka CRUD, ratings, user management, analytics — sab **data manipulation** hai jo **UI se independent** ho sakta hai. **WordPress** (backend + frontend dono handle karta hai, PHP mein likha) ne apna **REST API** bhi provide karna shuru kiya — jisse aap sirf uske data-management backend ko use karke apna khud ka custom frontend (Vue.js 2 jaisa) bana sakte ho.

**Definition (Headless CMS):** A headless CMS is a content management system that provides only data storage and an API for accessing/managing content, without dictating or providing any specific frontend presentation layer.

### Static Site Generators (SSGs)

**Examples per source:** NextJS (React-based), NuxtJS (Vue-based, "like Next but with Vue"), Gatsby (JS-based, interactive) vs Jekyll, Hugo (primarily text-oriented, blogs/homepages).

**Why SSGs?**
- Server sirf **static files serve** karta hai — fetch karne mein fast.
- **"First Contentful Paint"** improve hoti hai (pure HTML jaldi parse/display ho jaata hai).
- "Compile time" optimizations transferred data kam kar dete hain.

### Hydration

**Definition:** Hydration is the process of attaching JavaScript event handlers and interactivity to static, server-rendered HTML after the initial page has already been displayed to the user.

```text
Static HTML sent → displayed fast (no interactivity yet)
        ↓
JS bundle loads (delayed)
        ↓
"Hydrates" the HTML → event handlers attached
        ↓
Page becomes interactive
```

**Advantage:** Speed (initial paint fast) + eventual interactivity — best of both worlds.

### JAMstack: "Pinnacle" of Web Development? (Source's own framing + caveats)

**Explanation:** Source khud is claim ko qualify karta hai — JAM approach **storage + logic + presentation** type apps ke liye kaafi general hai (kyunki APIs kisi bhi backend ko handle kar sakte hain, markup easily change/compile ho sakta hai, aur JS bahut powerful hai). Lekin:

- **Real-time communication** (chat, video/audio) jaise use-cases ke liye extra techniques chahiye (WebRTC, WebSockets).
- Naye interfaces (holographic displays, haptic feedback, VR) ke liye JAM approach ka sufficiency clear nahi hai.
- **Source's own conclusion:** JAM approach tab tak general/useful rahega **jab tak performance issues na aaye** — uske baad "Next Big Thing" ka wait karna padega. (Ye source ki khud ki honest, non-dogmatic framing hai — isko "JAMstack sabse best hai forever" jaise absolute claim ke roop mein present nahi karna chahiye.)

> ⚠️ **Modern Update — 2022 → 2026 (industry-terminology note):** Industry mein **"JAMstack" term ka standalone usage 2022 ke baad se kam hua hai** — kaafi modern frameworks (jaise Next.js, Nuxt) ab **hybrid rendering** (SSR + SSG + ISR + streaming, sab ek hi framework mein configurable) support karte hain, jisse "static-first" JAMstack philosophy aur traditional server-rendering ke beech ka strict distinction blur ho gaya hai. **Verification unavailable — source material does not establish current (2026) industry terminology trends explicitly; is observation ko general technical-ecosystem awareness ke roop mein treat karein, na ki source-verified fact ke roop mein.** Course ke concepts (data/logic/presentation separation, static generation, hydration) still fundamentally valid hain — sirf naming convention/marketing terminology ka emphasis shift hua hai.

**Source Coverage:** JAM Approach, Web Apps Overview.

---

## 20. Static Site Generators & Hydration

*(See Section 19 above — SSG aur Hydration dono JAM/JAMstack context ka hi hissa hain, isliye combined coverage upar di gayi hai taaki concept flow tuta na ho.)*

---

## 21. ⚠️ 2022 → 2026 Modern Update Boxes

### Vue.js version
**Old lecture idea:** "Vue" generically reference hota hai bina version specify kiye.
**Current reality:** Course project explicitly Vue 2 (`^2.6.14`) use karta hai; Vue 3 (Composition API) is course ke scope mein nahi hai.
**What you should learn:** Options API syntax (`new Vue({ el, data, methods, mounted })`), jo pure notes mein consistently use kiya gaya hai.
**Exam perspective:** Agar exam mein Vue syntax likhna ho, **Options API + Vue 2 conventions** use karo, Composition API nahi.

### Flask + Authentication library
**Old lecture idea:** Flask-Security-Too ke saath opaque tokens.
**Current reality:** Live-class material already **Flask-JWT-Extended** (real JWT) pe migrate ho chuka hai, with current-generation package versions (`requirements.txt` mein Flask 3.1.3, Flask-JWT-Extended 4.7.4).
**What you should learn:** `flask_jwt_extended` ka flow (`create_access_token`, `@jwt_required()`, `user_identity_loader`, `user_lookup_loader`) — ye zyada relevant/current hai.
**Exam perspective:** Dono approaches ka **conceptual difference** samajhna chahiye (opaque token vs standards-based JWT), lekin practical coding questions ke liye Flask-JWT-Extended zyada likely hai.

### JAMstack terminology
**Old lecture idea:** JAM/JAMstack as ek clean, distinct architectural category.
**Current reality:** Verification unavailable in source material for exact 2026 industry status — general awareness suggests hybrid-rendering frameworks ne is strict boundary ko blur kiya hai.
**What you should learn:** Underlying concepts (separation of storage/logic/presentation, static generation, hydration) — ye timeless hain, chahe marketing terminology badal jaaye.
**Exam perspective:** Core JAM concepts (kya store karte hain APIs mein, kya JS handle karta hai, kya markup handle karta hai) exam-relevant rahenge.

---

## 22. ⚠️ Common Confusions

- **REST ≠ HTTP** — REST HTTP ke bina bhi theoretically implement ho sakta hai (different protocol pe), though practically HTTP ke saath hi popular hai.
- **REST ≠ CRUD** — REST ek broader architectural style hai, sirf Create/Read/Update/Delete tak limited nahi.
- **API ≠ REST API** — har API REST follow nahi karti (GraphQL bhi ek API hai, but not REST-style).
- **Authentication ≠ Authorization** — pehle "who are you", phir "what can you do".
- **JWT ≠ Authentication itself** — JWT sirf ek token format/mechanism hai.
- **PUT ≠ PATCH** — PUT full replace karta hai, PATCH incremental/partial update.
- **POST ≠ hamesha "create"** — convention hai, but koi bhi POST endpoint sirf-create ho, guarantee nahi.
- **GraphQL ≠ database** — GraphQL ek query language/layer hai, database replace nahi karta.
- **GraphQL ≠ automatically faster/simpler than REST** — server complexity kam nahi hoti, kabhi badh sakti hai.
- **HTML ≠ CSS**, **HTML ≠ JavaScript** — HTML structure/semantics, CSS presentation, JS behavior.
- **Markdown ≠ HTML** — Markdown lightweight text markup hai jo compile hokar HTML ban sakta hai, but khud HTML nahi.
- **Static HTML ≠ non-interactive forever** — hydration ke through baad mein interactive banaya jaa sakta hai.
- **Hydration ≠ Server-Side Rendering** — SSR HTML generate karne ka process hai; hydration us HTML ko baad mein interactive banane ka process hai — dono alag steps hain.
- **Vue ≠ backend framework**, **Flask ≠ frontend framework** — Vue frontend/presentation layer ke liye, Flask backend/API layer ke liye.

---

## 23. 🔥 Week 8 Exam Revision

### Most Important Definitions
- **API:** A set of remotely-callable functions, designed primarily for developers to build applications with.
- **REST:** An architectural style (not a rigid spec) for networked applications.
- **JWT:** A compact, self-contained, signed token format (Header.Payload.Signature) used to carry identity claims statelessly.
- **GraphQL:** A declarative query language + server layer that lets clients request exactly the data they need.
- **Hydration:** Attaching JS interactivity to already-displayed static HTML.
- **Headless CMS:** Data + API only, no bundled frontend.

### Most Important Differences
- Authentication vs Authorization
- PUT vs PATCH
- REST vs GraphQL
- Path parameter vs Query parameter
- SSG vs SSR
- Hybrid Vue-in-Jinja vs Fully-separated Vue SPA + Flask API

### Important HTTP Verbs Recap
`GET` (read, cacheable) · `POST` (create, not cacheable) · `PUT` (full update) · `PATCH` (partial update, preferred) · `DELETE` (remove)

### REST Key Points
- Architectural style, not a spec.
- Most "RESTful" APIs violate some constraint somewhere.
- Chatty & non-general-query-language are its two big practical limitations (motivates GraphQL).

### GraphQL Key Points
- Single endpoint, client-specified fields, type system, resolvers connect to multiple backends.
- Does not reduce backend complexity by default; caching is harder.

### JWT / Flask API Key Points
- Stateless; identity lives in the signed token.
- `create_access_token`, `@jwt_required()`, `user_identity_loader`, `user_lookup_loader`, `current_user` — core Flask-JWT-Extended building blocks.
- Passwords must be hashed in production (lecture code was simplified).

### Vue.js 2 + Flask Key Points
- **Vue.js 2 only** (Options API: `new Vue({ el, data, mounted, methods })`) — confirmed by course's own `vue@^2.6.14` dependency.
- Two integration patterns: **Vue-in-Jinja (hybrid, cookie-auth)** vs **Separate SPA + API (token/JWT-auth)**.
- Jinja/Vue delimiter clash solved via `{% raw %}...{% endraw %}` or custom Vue `delimiters` config (`${ }`).
- Built Vue `dist/` output can be served from Flask's static folder with `publicPath` configured correctly.

### Markup / JAM / SSG / Hydration Key Points
- HTML = living standard, semantic + extensible, but verbose/not ideal for structured data.
- Text markup (Markdown etc.) = human-friendly but structure-ambiguous.
- JAM = JavaScript (logic) + APIs (storage) + Markup (presentation).
- SSGs (Next/Nuxt/Gatsby/Jekyll/Hugo) prioritize fast "First Contentful Paint" via static delivery.
- Hydration = static HTML shown fast, then made interactive.

---

## One-Page Ultra-Fast Revision Table

| Topic | One-liner |
|---|---|
| API purpose | Functions for developers, not the end product itself |
| Good URL design | Nouns for resources, HTTP verbs for actions |
| GET vs POST | GET reads & caches; POST creates & isn't cached |
| PATCH vs PUT | PATCH = partial (preferred), PUT = full replace |
| JSON | Simple, human-readable, but weak typing |
| Auth vs Authz | Identity vs Permission |
| JWT | Stateless, signed, 3-part token |
| REST limitation | Chatty + no arbitrary query language |
| GraphQL fix | Single endpoint, client picks fields, but adds server-side complexity |
| Vue.js 2 + Flask (hybrid) | Vue as static lib inside Jinja, cookie-auth, delimiter clash needs fixing |
| Vue.js 2 + Flask (separate) | Vue CLI SPA + Flask API, token/JWT auth, CORS needed |
| JAM | JS (logic) + API (storage) + Markup (presentation) |
| SSG | Pre-built static HTML → fast First Contentful Paint |
| Hydration | Static HTML shown fast, then JS attaches interactivity |
