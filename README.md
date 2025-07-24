
# OpenLibrary Vue Search App

A responsive Vue.js + Tailwind CSS web app that allows users to search for books using the OpenLibrary API. Includes features like dark mode, infinite scroll or pagination, and detail previews for selected books.

---

## 🚀 Features

* 🔍 Live search using OpenLibrary API
* 🌗 Light/Dark theme toggle
* 📚 Responsive book cards with animation
* 📄 Book details: description, subjects, and useful links
* ♾️ Infinite scroll or 📑 Pagination (configurable)
* 🎨 Tailwind CSS styled layout

---

## 📁 Project Structure

```
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── SearchBar.vue
│   │   ├── SearchItem.vue
│   │   ├── SearchItemList.vue
│   │   ├── LoadingView.vue
│   │   ├── EmptySearch.vue
│   │   ├── AppHeader.vue
│   │   └── AppFooter.vue
│   ├── layouts/
│   │   └── Layout.vue
│   ├── App.vue
│   └── main.js
├── tailwind.config.js
├── vite.config.js
└── index.html
```

---

## 🔧 Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/ayansh-srivastava/Saras-assignment
cd my-vue-app
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Run the App

```bash
npm run dev
```

Visit: [http://localhost:5173](http://localhost:5173)

---

## 🌐 Deployment Options

### Recommended: [Vercel](https://vercel.com)

```bash
# install CLI if needed
npm install -g vercel
vercel
```

### Others:

* GitHub Pages (via `vite-plugin-gh-pages`)
* Netlify
* Firebase Hosting

---

## 📦 Tech Stack

* **Vue 3** + `<script setup>`
* **Tailwind CSS**
* **OpenLibrary REST API**
* **Vite** as build tool

---

## 🧠 Future Enhancements

* Add bookmarking or favorite list
* Search suggestions with debounce
* Support author/subject filters

---

## 🤝 License

MIT License — Feel free to use, modify, and distribute.

---

## 📬 Contact

Feel free to connect via \[[ayansh.shrivastava.007@gmail.com](mailto:ayansh.shrivastava.007@gmail.com)] or GitHub issues for questions and feedback.
