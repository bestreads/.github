# Bestreads

Ziel ist es, eine echte Alternative zu Goodreads zu entwickeln.
Aktuell leidet Goodreads unter einer unübersichtlichen Oberfläche, einem unvollständigen Buchindex – und die Performance ist schlicht katastrophal.

Als großes Feature kommen grundlegende Social-Media-Funktionen hinzu.
Aktivitäten wie Lesefortschritte oder Statusupdates sollen mit anderen Nutzerinnen und Nutzern geteilt werden können.


| Sprint | Zeitraum             |
| ------ | -------------------- |
| 1      | 24.11 – 05.12        |
| 2      | 08.12 – 19.12        | 
| 3      | 22.12 – 02.01  Projektpause      |
| 4      | 05.01 – 16.01        |
| 5      | 19.01 – 30.01        |
| 6      | 02.02 – 13.02 <-- Derzeit hier        |
| 7      | 16.02 – 27.02        |
| 8      | 02.03 – 12.03        |

---
12.03.2026 Abgabe und Präsentation
---
Hochschulphase: 12.01 - 6.02

---

# Aktueller Stand

> **Stand:** 20.01.2026

### **Legende**

**Status:**
- ✅ = Final vorhanden 
- 🟡 = Im Frontend vorbereitet, aber noch nicht final umgesetzt
- ❌ = Fehlt komplett

**Priorität:**
- 1 = Hoch
- 2 = Mittel
- 3 = Niedrig

## Backend-Endpunkte

### **1. Authentifizierung (JWT in HttpOnly-Cookies)**

| Feature | Prio | Frontend | Backend | API-Endpunkt | Datenformat (Request → Response) |
|---------|------|----------|---------|--------------|----------------------------------|
| **Login** | 1 | ✅ | ✅ | `POST /auth/login` | `{email, password}` → Cookie: `jwt=...` + `{user}` |
| **Registrierung** | 1 | ✅ | ✅ | `POST /auth/register` | `{username, email, password}` → Cookie + `{user}` |
| **Logout** | 1 | ✅ | ✅ | `POST /auth/logout` | - → Cookie löschen |
| **Session prüfen** | 1 | ✅ | ✅ | `GET /auth/me` | Cookie automatisch → `{user}` oder `401` |


### **2. User-Daten (ein Endpunkt für alle Änderungen)**

```typescript
interface User {
  id: string
  username: string
  email: string
  profilePictureURL: string
}
```

| Feature | Prio | Frontend | Backend | API-Endpunkt | Datenformat |
|---------|------|----------|---------|--------------|-------------|
| **User-Daten ändern** | 3 | ✅ | ✅ | `PATCH /users/me` | `{username?, email?, password?, profilePicture?}` → `{user}` |



### **3. Profil-Seite (UserProfile)**

| Feature | Prio | Frontend | Backend | API-Endpunkt | Datenformat (Response) |
|---------|------|----------|---------|--------------|------------------------|
| **Profil-Header Daten** | 1 | ✅ | ✅ | `GET /user/profile/:userId` | siehe unten |

**Profil-Header Response:**
```typescript
interface UserProfile {
  userId: string
  username: string
  profilePictureURL: string
  accountCreatedAtYear: number
  booksInLibrary: number
  posts: number
}
```



### **4. Bibliothek (Library)**

| Feature | Prio | Frontend | Backend | API-Endpunkt | Datenformat |
|---------|------|----------|---------|--------------|-------------|
| **Eigene Bücher abrufen** | 1 | ✅ | ✅ | `GET /users/me/library` | → `BookWithUserData[]` |
| **Fremde Bibliothek** | 1 | ✅ | ✅ | `GET /users/:userId/library` | → `BookWithUserData[]` |
| **Buch zur Bibliothek hinzufügen** | 1 | ✅ | ✅ |  | `POST /users/me/library` | `{isbn, state}` → `{userBook}` |
| **Buch aus Bibliothek löschen** | 1 | ✅ | ✅ | `DELETE /users/me/library/:isbn` | `{}` → `{success}` |
| **Buch-Status ändern** | 2 | ✅ | ✅ | `PUT /users/me/library/:isbn/status` | `{state}` → `{userBook}` |
| **Buch bewerten** | 3 | ✅ | ✅ | `PUT /users/me/library/:isbn/rating` | `{rating}` → `{userBook}` |

**Buch-Datenstrukturen:**
```typescript
interface Book {
  ISBN: string
  title: string
  author: string
  coverurl: string
  ratingavg: number
  description: string
  releasedate: number
  genre?: string
}

interface UserBook {
  state: "read" | "reading" | "want-to-read"
  rating: number
}

interface BookWithUserData extends Book {
  userBook: UserBook
}
```



### **5. Buchsuche**

| Feature | Prio | Frontend | Backend | API-Endpunkt | Datenformat |
|---------|------|----------|---------|--------------|-------------|
| **Bücher suchen** | 1 | ✅ | ✅ | `GET /books/search?q=<query>` | → `Book[]` |
| **Buch-Details** | 1 | ✅ | ✅ | `GET /books/:isbn` | → `Book` |



### **6. Posts/Feed**

| Feature | Prio | Frontend | Backend | API-Endpunkt | Datenformat |
|---------|------|----------|---------|--------------|-------------|
| **Feed abrufen (Home)** | 1 | ✅ | ✅ | `GET /feed` | → `Post[]` (paginiert) |
| **User-Posts abrufen (Profil)** | 1 | ✅ | ✅ | `GET /users/:userId/posts` | → `Post[]` |
| **Post erstellen** | 1 | ✅ | ✅ | `POST /user/id/post` | `{bookId, content, b64Image}` → `{post}` |


**Post-Datenstrukturen:**
```typescript
interface PostAuthor {
  userId: string
  username: string
  profilePictureURL?: string
}

interface Post {
  id: string
  author: PostAuthor
  book: BookWithUserData
  content: string
  createdAt: string
  likes?: number
  commentCount?: number
  comments?: Comment[]
}

interface Comment {
  id: string
  author: PostAuthor
  content: string
  createdAt: string
  likes?: number
}
```
