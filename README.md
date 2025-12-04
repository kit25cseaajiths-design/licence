# 1. initialize
git init
gh repo create codecraft-merge --public --description "CodeCraft Merge - merge puzzle game teaching programming (PWA)" || true

# 2. create files (you can also copy the file contents below into files)
# 3. add & commit
git add .
git commit -m "Initial scaffold: Vite + React + TypeScript + Tailwind + PWA + Apache-2.0"

# 4. push (if gh created repo remotes are set)
git branch -M main
git remote add origin git@github.com:YOUR_GITHUB_USERNAME/codecraft-merge.git
git push -u origin main
Replace YOUR_GITHUB_USERNAME with your username if you added remote manually. If you used gh repo create, gh sets the remote for you.

Recommended repository file tree
arduino
Copy code
codecraft-merge/
├─ .gitignore
├─ LICENSE
├─ README.md
├─ package.json
├─ vite.config.ts
├─ tailwind.config.cjs
├─ postcss.config.cjs
├─ tsconfig.json
├─ public/
│  └─ manifest.webmanifest
├─ src/
│  ├─ main.tsx
│  ├─ index.css
│  ├─ App.tsx
│  ├─ components/
│  │  ├─ ui/
│  │  │  ├─ Button.tsx
│  │  │  └─ Card.tsx
│  │  └─ GameBoard/
│  │     ├─ GameBoard.tsx
│  │     └─ Tile.tsx
│  ├─ pages/
│  │  ├─ Home.tsx
│  │  └─ Profile.tsx
│  ├─ hooks/
│  └─ utils/
└─ tailwind.css
Key files (copy into your repo)
package.json
json
Copy code
{
  "name": "codecraft-merge",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext .ts,.tsx",
    "format": "prettier --write ."
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.14.1",
    "@supabase/supabase-js": "^2.0.0",
    "framer-motion": "^10.12.5",
    "react-dnd": "^16.0.1",
    "react-dnd-html5-backend": "^16.0.1"
  },
  "devDependencies": {
    "typescript": "^5.6.0",
    "vite": "^5.1.0",
    "tailwindcss": "^4.0.0",
    "postcss": "^8.4.0",
    "autoprefixer": "^10.4.0",
    "eslint": "^8.0.0",
    "prettier": "^2.0.0"
  }
}
vite.config.ts
ts
Copy code
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  server: { port: 5173 }
})
tailwind.config.cjs
js
Copy code
module.exports = {
  content: ['./index.html', './src/**/*.{ts,tsx,js,jsx}'],
  theme: {
    extend: {
      colors: {
        primary: '#0EA5A4', // example, replace with PRD palette
        accent: '#7C3AED',
        bg: '#0F172A'
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
        mono: ['JetBrains Mono', 'ui-monospace', 'monospace']
      }
    }
  },
  plugins: []
}
postcss.config.cjs
js
Copy code
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {}
  }
}
src/main.tsx
tsx
Copy code
import React from 'react'
import { createRoot } from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import App from './App'
import './index.css'

createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
)
src/index.css
css
Copy code
@tailwind base;
@tailwind components;
@tailwind utilities;

/* app-level css */
html,body,#root { height:100%; }
body { @apply bg-[#0f172a] text-white antialiased; }
src/App.tsx
tsx
Copy code
import React from 'react'
import { Routes, Route, Link } from 'react-router-dom'
import Home from './pages/Home'
import Profile from './pages/Profile'

export default function App() {
  return (
    <div className="min-h-screen">
      <header className="p-4 shadow-md bg-gradient-to-r from-primary to-accent">
        <div className="container mx-auto flex justify-between items-center">
          <h1 className="text-xl font-bold">CodeCraft Merge</h1>
          <nav>
            <Link to="/" className="mr-4">Home</Link>
            <Link to="/profile">Profile</Link>
          </nav>
        </div>
      </header>

      <main className="container mx-auto p-4">
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/profile" element={<Profile />} />
        </Routes>
      </main>
    </div>
  )
}
src/pages/Home.tsx
tsx
Copy code
import React from 'react'
import GameBoard from '../components/GameBoard/GameBoard'

export default function Home() {
  return (
    <div>
      <h2 className="text-2xl mb-4">Play</h2>
      <GameBoard />
    </div>
  )
}
src/components/GameBoard/GameBoard.tsx
tsx
Copy code
import React from 'react'

const GRID_SIZE = 6

export default function GameBoard() {
  const tiles = Array.from({ length: GRID_SIZE * GRID_SIZE })
  return (
    <div className="grid gap-2" style={{ gridTemplateColumns: `repeat(${GRID_SIZE}, minmax(0,1fr))` }}>
      {tiles.map((_, i) => (
        <div key={i} className="aspect-square bg-slate-800 rounded flex items-center justify-center">
          <span className="text-sm opacity-70">Tile {i+1}</span>
        </div>
      ))}
    </div>
  )
}
.gitignore
bash
Copy code
node_modules
dist
.env
.vscode
.DS_Store
public/manifest.webmanifest (basic PWA)
json
Copy code
{
  "short_name": "CodeCraft",
  "name": "CodeCraft Merge",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#0f172a",
  "theme_color": "#0EA5A4",
  "icons": []
}
README.md (starter)
md
Copy code
# CodeCraft Merge

CodeCraft Merge is a PWA that teaches programming through merge-puzzle mechanics.

## Tech stack
- Vite + React + TypeScript
- Tailwind CSS
- Framer Motion
- React DnD
- Supabase (Auth, Storage, DB)
- OpenAI (optional for hints/explanations)

## Setup
1. `npm install`
2. Create `.env` with SUPABASE keys (see `.env.example`)
3. `npm run dev`

## Project structure
(see repo tree in project root)

## License
This project is licensed under the Apache License 2.0 — see `LICENSE`.
Add the Apache License 2.0
Create a LICENSE file at the repo root containing the Apache License 2.0 text. I’ll paste the standard Apache-2.0 text here so you can copy it exactly into LICENSE:

arduino
Copy code
Apache License
Version 2.0, January 2004
http://www.apache.org/licenses/

TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

1. Definitions.
