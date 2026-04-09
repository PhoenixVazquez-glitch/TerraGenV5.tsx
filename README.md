#!/usr/bin/env bash
# create-files.sh
# Single copy-paste script: creates a complete Vite + React + TypeScript + Tailwind + PWA scaffold
# with the TerraGen V5 UI, GitHub Actions workflows (CI + Pages), Capacitor hooks, Stripe example,
# LICENSE, README, and test stub. Run in an empty directory.
#
# Usage:
#   1) Save this as create-files.sh
#   2) chmod +x create-files.sh
#   3) ./create-files.sh
#   4) Follow the README.md steps: git init, commit, create repo, push, add GitHub Secrets, etc.

set -e
echo "Creating TerraGen v5 project files..."

# package.json
cat > package.json <<'EOF'
{
  "name": "terra-gen-v5",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview --port 4173",
    "lint": "eslint --ext .ts,.tsx src || true",
    "test": "vitest",
    "format": "prettier --write .",
    "cap:sync:android": "npx cap sync android",
    "cap:add:android": "npx cap add android",
    "cap:add:ios": "npx cap add ios"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "lucide-react": "^0.281.0"
  },
  "devDependencies": {
    "@types/react": "^18.0.28",
    "@types/react-dom": "^18.0.11",
    "typescript": "^5.1.3",
    "vite": "^5.1.0",
    "@vitejs/plugin-react": "^4.0.0",
    "tailwindcss": "^4.0.0",
    "postcss": "^8.4.23",
    "autoprefixer": "^10.4.14",
    "vitest": "^0.32.0",
    "@testing-library/react": "^14.0.0",
    "@testing-library/jest-dom": "^6.0.0",
    "eslint": "^8.45.0",
    "prettier": "^3.0.0"
  }
}
EOF

# vite.config.ts
cat > vite.config.ts <<'EOF'
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: { port: 5173 }
});
EOF

# tsconfig.json
cat > tsconfig.json <<'EOF'
{
  "compilerOptions": {
    "target": "ES2021",
    "lib": ["DOM", "ES2021"],
    "jsx": "react-jsx",
    "module": "ESNext",
    "moduleResolution": "Node",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true
  },
  "include": ["src"]
}
EOF

# tailwind.config.cjs
cat > tailwind.config.cjs <<'EOF'
module.exports = {
  content: ['./index.html', './src/**/*.{ts,tsx,js,jsx}'],
  theme: { extend: {} },
  plugins: []
};
EOF

# postcss.config.cjs
cat > postcss.config.cjs <<'EOF'
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {}
  }
};
EOF

# index.html
cat > index.html <<'EOF'
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>TERRA-GEN v5</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
EOF

# directories
mkdir -p src/components src/styles src/components/__tests__ public .github/workflows capacitor react-native

# main.tsx
cat > src/main.tsx <<'EOF'
import React from 'react';
import { createRoot } from 'react-dom/client';
import TerraGenV5 from './components/TerraGenV5';
import './styles/tailwind.css';

createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <TerraGenV5 />
  </React.StrictMode>
);
EOF

# tailwind.css
cat > src/styles/tailwind.css <<'EOF'
@tailwind base;
@tailwind components;
@tailwind utilities;

html, body, #root { height: 100%; }
body { margin: 0; font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; background: #05070A; }
EOF

# TerraGenV5.tsx
cat > src/components/TerraGenV5.tsx <<'EOF'
import React, { memo } from 'react';
import { ShieldAlert, Globe, TrendingUp, DollarSign, Lock } from 'lucide-react';
import TerraGenFutureUI from './TerraGenV5.future.ui';

const brand = "TERRA-GEN v5";
const owner = "Hugo Vazquez";

export default function TerraGenV5(): JSX.Element {
  const leads = [
    { id: 'naples', area: 'Naples', project: 'Luxury Hardscape', fee: '$25' },
    { id: 'ftmyers', area: 'Ft. Myers', project: 'Commercial Irrigation', fee: '$15' },
    { id: 'sarasota', area: 'Sarasota', project: 'Estate Softscape', fee: '$40' },
  ];

  return (
    <div className="min-h-screen bg-[#05070A] text-slate-100 font-sans">
      <div className="bg-amber-500/10 border-b border-amber-500/20 px-6 py-2 flex justify-center items-center gap-3">
        <ShieldAlert className="w-4 h-4 text-amber-500" aria-hidden="true" />
        <p className="text-[10px] font-black text-amber-500 uppercase tracking-widest">
          Legal Status: Marketing & Lead Brokerage Platform — No Contractor License Required
        </p>
      </div>

      <main className="max-w-[1400px] mx-auto p-8">
        <header className="flex flex-col md:flex-row justify-between items-end gap-6 mb-12">
          <div>
            <div className="flex items-center gap-3 mb-2">
              <div className="bg-emerald-500 w-3 h-3 rounded-full animate-pulse shadow-[0_0_10px_#10b981]" aria-hidden="true" />
              <p className="text-xs font-bold text-emerald-500 tracking-[0.3em] uppercase">System Live</p>
            </div>
            <h1 className="text-6xl font-black italic tracking-tighter text-white">{brand}</h1>
            <p className="text-slate-500 font-bold text-sm mt-2 uppercase tracking-widest">Master Command: {owner}</p>
          </div>

          <div className="flex gap-4">
            <div className="bg-slate-900 border border-slate-800 p-6 rounded-3xl">
              <p className="text-[10px] text-slate-500 font-black uppercase mb-1">Terra-Coin Reserve</p>
              <p className="text-3xl font-black text-emerald-400">🪙 1,250.00</p>
            </div>
          </div>
        </header>

        <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-12">
          <StatBox label="Total Ad Revenue" value="$12.4k" icon={<TrendingUp />} color="text-blue-400" />
          <StatBox label="Active Connections" value="84" icon={<Globe />} color="text-emerald-400" />
          <StatBox label="Lead Payouts" value="$5,290" icon={<DollarSign />} color="text-cyan-400" />
        </div>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
          <div className="lg:col-span-2">
            <div className="flex justify-between items-center mb-6">
              <h2 className="text-xl font-black italic uppercase text-white tracking-tight">Available Lead Inventory</h2>
              <span className="text-[10px] bg-slate-800 px-3 py-1 rounded-full font-bold">UPDATED: JUST NOW</span>
            </div>

            <div className="space-y-4">
              {leads.map(lead => (
                <LeadCard key={lead.id} area={lead.area} project={lead.project} fee={lead.fee} />
              ))}
            </div>
          </div>

          <div className="bg-gradient-to-b from-slate-900 to-black border border-slate-800 rounded-[2.5rem] p-8">
            <h3 className="text-lg font-black italic uppercase text-emerald-500 mb-6 flex items-center gap-2">
              Coin Utility
            </h3>
            <div className="space-y-6">
              <div className="p-4 bg-emerald-500/5 border border-emerald-500/20 rounded-2xl">
                <p className="text-xs font-bold text-slate-400 mb-1 uppercase text-center tracking-widest">Market Price</p>
                <p className="text-2xl font-black text-white text-center italic">1 🪙 = $1.12 USD</p>
              </div>
              <button className="w-full bg-white text-black font-black py-4 rounded-xl hover:bg-emerald-400 transition-colors uppercase italic text-sm tracking-widest">
                Stake Coins
              </button>
              <div className="pt-4 border-t border-slate-800 italic text-[10px] text-slate-500 text-center uppercase tracking-widest leading-relaxed">
                Tokens used for priority bidding and multi-branch scaling.
              </div>
            </div>
          </div>
        </div>

        <TerraGenFutureUI />

        <footer className="mt-20 pt-8 border-t border-slate-900 text-center opacity-40">
          <p className="text-[9px] font-medium tracking-[0.2em] leading-loose text-slate-500">
            © {new Date().getFullYear()} {brand} SYSTEMS. ALL RIGHTS RESERVED. <br />
            TERRA-GEN OPERATES EXCLUSIVELY AS A LEAD REFERRAL AND DIGITAL ADVERTISING INTERMEDIARY. <br />
            WE DO NOT PERFORM LICENSED CONTRACTING SERVICES.
          </p>
        </footer>
      </main>
    </div>
  );
}

type StatBoxProps = {
  label: string;
  value: string;
  icon?: React.ReactNode;
  color?: string;
};

const StatBox = memo(function StatBox({ label, value, icon, color = 'text-white' }: StatBoxProps) {
  return (
    <div className="bg-slate-900/40 border border-slate-800 p-8 rounded-[2rem] hover:border-slate-700 transition-all">
      <div className={`${color} mb-4`}>{icon}</div>
      <p className="text-[10px] font-black text-slate-500 uppercase tracking-widest mb-1">{label}</p>
      <p className="text-4xl font-black italic text-white">{value}</p>
    </div>
  );
});

type LeadCardProps = {
  area: string;
  project: string;
  fee: string;
};

const LeadCard = memo(function LeadCard({ area, project, fee }: LeadCardProps) {
  return (
    <div className="bg-slate-900 border border-slate-800 p-6 rounded-2xl flex justify-between items-center group hover:border-emerald-500/50 transition-all">
      <div>
        <p className="text-[10px] font-black text-emerald-500 uppercase tracking-widest mb-1">{area}</p>
        <h4 className="text-xl font-black italic uppercase tracking-tight text-white group-hover:text-emerald-400 transition-colors">{project}</h4>
      </div>
      <div className="flex items-center gap-6">
        <div className="text-right">
          <p className="text-xs font-black text-white">FEE: {fee}</p>
          <p className="text-[9px] font-bold text-slate-500 uppercase italic">Referral Cost</p>
        </div>
        <button
          type="button"
          className="bg-emerald-600 text-white font-black px-6 py-3 rounded-xl hover:bg-emerald-500 transition-all text-[10px] uppercase flex items-center gap-2 italic focus:outline-none focus:ring-2 focus:ring-emerald-400"
          aria-label={`Unlock lead for ${project} in ${area} (${fee})`}
        >
          UNLOCK LEAD <Lock className="w-3 h-3" aria-hidden="true" />
        </button>
      </div>
    </div>
  );
});
EOF

# TerraGenV5.future.ui.tsx
cat > src/components/TerraGenV5.future.ui.tsx <<'EOF'
import React from 'react';
import { WifiOff, Bluetooth, Phone, ServerCog, TrendingUp, Sparkles, ShieldAlert } from 'lucide-react';

export default function TerraGenFutureUI(): JSX.Element {
  const brand = "TERRA-GEN v5";
  return (
    <section className="mt-10">
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
        <div className="lg:col-span-2">
          <div className="bg-slate-900/40 border border-slate-800 rounded-2xl p-6">
            <h2 className="text-xl font-black italic uppercase tracking-tight text-white mb-4">Marketplace & Roadmap</h2>
            <p className="text-slate-300 mb-4">Offline-first PWA, BluTube local networking (BLE mesh), VoIP/phone service via SIP/VoIP provider, and token-driven priority bidding (TERRA-COIN).</p>
            <div className="space-y-4">
              <div className="p-4 bg-slate-800 rounded-xl">
                <div className="flex justify-between">
                  <div>
                    <p className="text-xs font-black text-slate-400 uppercase tracking-widest">Lead Inventory</p>
                    <p className="font-black text-white text-lg">Dynamic marketplace, token-weighted auctions</p>
                  </div>
                  <div className="text-emerald-400"><TrendingUp /></div>
                </div>
              </div>
              <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                <FeatureCard title="Offline PWA" desc="Local queue + auto-sync" icon={<WifiOff />} />
                <FeatureCard title="BluTube (BLE)" desc="Local discovery & proximity bidding" icon={<Bluetooth />} />
                <FeatureCard title="Phone Service" desc="VoIP + SIP gateway (PSTN)" icon={<Phone />} />
                <FeatureCard title="Network Mode" desc="Promote devices as local cache/relay" icon={<ServerCog />} />
              </div>
            </div>
          </div>
        </div>

        <aside className="bg-gradient-to-b from-slate-900 to-black border border-slate-800 rounded-2xl p-6">
          <h3 className="text-lg font-black italic text-emerald-500">Token & Security</h3>
          <p className="text-slate-300 my-3">TERRA-COIN powers priority bidding, staking, and instant revenue routing. Consult counsel before issuing tokens; handle KYC/AML for payouts.</p>

          <div className="space-y-3 mt-4">
            <InfoTile title="Priority Bidding" value="Token-weighted auctions" icon={<Sparkles />} />
            <InfoTile title="Scaling" value="Multi-branch orchestration" icon={<TrendingUp />} />
            <InfoTile title="Security" value="E2E + encrypted local cache" icon={<ShieldAlert />} />
          </div>
        </aside>
      </div>
    </section>
  );
}

function FeatureCard({ title, desc, icon }: any) {
  return (
    <div className="bg-slate-800/50 border border-slate-700 p-4 rounded-xl">
      <div className="flex items-start gap-3">
        <div className="text-cyan-400">{icon}</div>
        <div>
          <div className="font-black uppercase tracking-widest text-xs">{title}</div>
          <div className="text-sm text-slate-300">{desc}</div>
        </div>
      </div>
    </div>
  );
}

function InfoTile({ title, value, icon }: any) {
  return (
    <div className="bg-slate-900/40 border border-slate-800 p-4 rounded-xl flex items-center gap-3">
      <div className="text-emerald-400">{icon}</div>
      <div>
        <div className="text-xs font-black uppercase tracking-widest text-slate-400">{title}</div>
        <div className="font-black text-white">{value}</div>
      </div>
    </div>
  );
}
EOF

# service worker stub
cat > src/serviceWorker.ts <<'EOF'
/**
 * serviceWorker.ts
 * Minimal Service Worker registration stub for PWA; implement caching strategies as needed.
 */

export function registerServiceWorker() {
  if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
      navigator.serviceWorker.register('/sw.js').then(reg => {
        console.log('ServiceWorker registered: ', reg);
      }).catch(err => {
        console.warn('ServiceWorker registration failed: ', err);
      });
    });
  }
}
EOF

# public/manifest.json
cat > public/manifest.json <<'EOF'
{
  "name": "TERRA-GEN v5",
  "short_name": "TERRA-GEN",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#05070A",
  "theme_color": "#10b981",
  "icons": [
    {
      "src": "/icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
EOF

# placeholder sw.js
cat > public/sw.js <<'EOF'
// Basic service worker placeholder. Customize caching as needed.
self.addEventListener('install', event => {
  self.skipWaiting();
  console.log('SW installed');
});

self.addEventListener('activate', event => {
  clients.claim();
  console.log('SW activated');
});

self.addEventListener('fetch', event => {
  // Default: network-first for dynamic resources; add caching rules as required.
});
EOF

# Simple Vitest test
cat > src/components/__tests__/TerraGenV5.test.tsx <<'EOF'
import React from 'react';
import { render, screen } from '@testing-library/react';
import TerraGenV5 from '../TerraGenV5';

test('renders brand heading', () => {
  render(<TerraGenV5 />);
  const heading = screen.getByText(/TERRA-GEN v5/i);
  expect(heading).toBeInTheDocument();
});
EOF

# .gitignore
cat > .gitignore <<'EOF'
node_modules
dist
.env
.vscode
.vite
android
ios
build
.gradle
*.keystore
EOF

# LICENSE (Apache-2.0)
cat > LICENSE <<'EOF'
Apache License
Version 2.0, January 2004
http://www.apache.org/licenses/
...
(Full Apache-2.0 text should be placed here before publishing. Replace this placeholder with the full license text.)
EOF

# README.md
cat > README.md <<'EOF'
# TERRA-GEN v5

TERRA-GEN v5 — AI engine connecting premium leads to contractors. This repository is a starter scaffold:
- Vite + React + TypeScript
- Tailwind CSS
- PWA service-worker stub
- Capacitor helpers (for web → native wrapper)
- React Native guidance (for advanced native features)
- GitHub Actions (CI + Pages deploy)
- Stripe serverless example and instructions (README section)
- Apache-2.0 license (placeholder — replace with full text)

Quick start:
1. Install dependencies:
   - npm install
2. Run dev server:
   - npm run dev
3. Build:
   - npm run build
4. Test:
   - npm run test

Create repo and push (example using gh CLI):
1. git init
2. git add .
3. git commit -m "Initial commit — TerraGen v5 scaffold"
4. gh repo create phoenixvazquez-glitch/terra-gen-v5 --public --source=. --remote=origin --push

Secrets you will add to GitHub (Actions -> Secrets):
- STRIPE_SECRET_KEY
- STRIPE_PUBLISHABLE_KEY
- STRIPE_WEBHOOK_SECRET
- STRIPE_CONNECT_CLIENT_ID
- ANDROID_KEYSTORE_BASE64
- ANDROID_KEYSTORE_PASSWORD
- ANDROID_KEY_ALIAS
- ANDROID_KEY_PASSWORD
- APP_STORE_CONNECT_ISSUER_ID
- APP_STORE_CONNECT_KEY_ID
- APP_STORE_PRIVATE_KEY

Stripe & Payout notes:
- Use Stripe Connect (Express recommended) for contractor payouts.
- Implement server endpoints to create Checkout sessions and handle webhooks server-side.
- Do not expose secret keys in the client.

Mobile build notes:
- Capacitor: quick wrapper. Run:
  - npm run build
  - npx cap add android
  - npx cap open android
- React Native: use the react-native directory guide (react-native/README.md) included for next steps.

Legal & Compliance:
- This code contains a presentational UI and placeholder logic. You must provide Terms of Service, Privacy Policy, and review local regulations (telecom, payments, tokens).
- If issuing tokens (TERRA-COIN), consult legal counsel re securities, AML/KYC.

See .github/workflows for CI & deployment workflows.

EOF

# GitHub Actions: ci.yml
cat > .github/workflows/ci.yml <<'EOF'
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Use Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - name: Install dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Run tests
        run: npm run test --if-present
EOF

# GitHub Actions: pages-deploy.yml
cat > .github/workflows/pages-deploy.yml <<'EOF'
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pages: write
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - name: Install
        run: npm ci
      - name: Build
        run: npm run build
      - name: Deploy to Pages
        uses: actions/configure-pages@v3
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./dist
EOF

# Android build stub
cat > .github/workflows/android-build.yml <<'EOF'
name: Android Build (AAB) - stub

on:
  workflow_dispatch:

jobs:
  build-android:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'
      - name: Install Node
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - name: Install deps
        run: npm ci
      - name: 