# Parikshan (Edunexi) Frontend

The Next.js frontend for **Parikshan / Edunexi**, a platform for universities to verify and track student achievements, community service, and events. Students build a portfolio of verified accomplishments; universities review, approve, and manage them. This repo is the web client. It talks to a separate Strapi-based backend API (likely the sibling `parikshan-api` repo).

## Features

- **Dual role experience**: separate `student` and `university` areas (achievements, portfolio, community service, events, notifications for students; student management, event management, approvals, and notifications for universities)
- **AI-assisted certificate evaluation**: uploaded certificates are analyzed via the Gemini API (`app/api/evaluate`) to score authenticity based on issuer reputation, verifiable elements, and document consistency, with extra weight when a faculty member has verified it
- **Community-service analysis endpoint**: `app/api/analyze-service` for AI-assisted review of community-service submissions
- **Student achievement portfolio**: students add achievements/certificates and build a shareable portfolio; QR codes (`qrcode.react` / `react-qr-reader`) support certificate verification flows
- **IPFS storage**: achievement/certificate metadata can be pinned to IPFS via Pinata (`lib/ipfs.ts`)
- **Events & notifications**: university-created events and an in-app notification system for both roles
- **Authentication**: JWT-based auth against a Strapi backend (`lib/auth.ts`), with a React context (`contexts/AuthContext.tsx`) managing session state
- **Rich, animated UI**: custom WebGL/Three.js background effects (`LiquidBackground`, `DotGrid`, `LightRays`), a full shadcn/Radix UI component set, and Tailwind CSS styling

## Tech Stack

- **Framework**: Next.js 14 (App Router), React 18, TypeScript
- **Styling**: Tailwind CSS, shadcn/ui, Radix UI primitives, tailwindcss-animate
- **Animation/Graphics**: Framer Motion, GSAP, Three.js, ogl
- **AI**: Google Generative AI (Gemini) for certificate/service evaluation
- **Storage**: Pinata (IPFS)
- **Forms & validation**: React Hook Form, Zod
- **OCR**: Tesseract.js
- **Package manager**: pnpm

## Project Structure

```
parikshan-frontend/
├── app/
│   ├── student/        # Student dashboard: achievements, portfolio, community-service, events, notifications
│   ├── university/     # University dashboard: students, events, approvals, notifications
│   ├── login/           # Auth page
│   ├── about/            # Team/about page
│   └── api/
│       ├── evaluate/              # AI certificate authenticity scoring (Gemini)
│       └── analyze-service/       # AI community-service analysis
├── components/         # UI components (cards, forms, navigation) + components/ui (shadcn)
├── contexts/           # AuthContext
├── hooks/              # use-api, use-loading
├── lib/                # auth, achievement, event, evaluation, scoring, ipfs, notification-service, role-utils, utils
└── types/              # Shared TypeScript types (auth, student, university, events, notifications, etc.)
```

## Getting Started

### Prerequisites
- Node.js 18+
- pnpm
- A running Strapi (or compatible) backend API
- API keys: Gemini (`GEMINI_API_KEY`), Pinata (`NEXT_PUBLIC_PINATA_JWT`)

### Installation

```bash
pnpm install
```

Create a `.env.local` with the required variables, e.g.:

```bash
NEXT_PUBLIC_STRAPI_URL=http://localhost:1337
NEXT_PUBLIC_PINATA_JWT=your_pinata_jwt
NEXT_PUBLIC_PINATA_GATEWAY_URL=https://gateway.pinata.cloud
GEMINI_API_KEY=your_gemini_api_key
```

### Development

```bash
pnpm dev
```

Visit `http://localhost:3000`.

### Build

```bash
pnpm build
pnpm start
```

## Usage

Students log in to add achievements and community-service records (with optional certificate uploads that get AI-scored for authenticity), browse/register for university events, and track notifications. Universities log in to manage their student roster, create events, review pending approvals, and send notifications.
