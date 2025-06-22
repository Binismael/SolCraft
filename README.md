
# SolCraft - Trading Infrastructure for the Next Era of Solana

SolCraft is a Next.js web application designed to provide a comprehensive platform for interacting with the Solana blockchain, focusing on trading, token launches, staking, and community engagement. It leverages Layer 2 solutions for enhanced scalability and security.

## Table of Contents

- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Firebase Setup](#firebase-setup)
  - [Environment Variables](#environment-variables)
  - [Installation](#installation)
  - [Running the Development Server](#running-the-development-server)
  - [Running Genkit Development Server](#running-genkit-development-server)
- [Key Features](#key-features)
- [Folder Structure](#folder-structure)
- [Styling](#styling)
- [AI Integration (Genkit)](#ai-integration-genkit)
- [Firebase Integration](#firebase-integration)
- [Deployment](#deployment)
- [Available Scripts](#available-scripts)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

SolCraft aims to be a user-friendly yet powerful platform for both new and experienced users in the Solana ecosystem. It offers tools for:

*   **Dashboard:** A central hub to view portfolio performance, key metrics, and recent activity.
*   **Token Swapping:** Seamlessly exchange cryptocurrencies.
*   **Deposit & Send:** Manage crypto assets securely.
*   **Launchpad:** Discover and participate in new token launches.
*   **Staking:** Grow crypto assets by staking in various pools.
*   **Profile Management:** Customize user profiles and connect wallets.
*   **Community Hub:** Connect with other investors and follow top players.
*   **Tournament Investment:** A unique feature allowing investment in poker tournament participation, with AI-powered risk assessment.

## Tech Stack

*   **Framework:** Next.js (App Router)
*   **Language:** TypeScript
*   **UI Components:** ShadCN UI
*   **Styling:** Tailwind CSS
*   **AI Integration:** Genkit (for Google AI models like Gemini)
*   **Backend/Database:** Firebase (Authentication, Firestore, Storage)
*   **State Management:** React Context API, `useState`, `useEffect`
*   **Forms:** React Hook Form with Zod for validation
*   **Charting:** Recharts (via ShadCN Charts)
*   **Icons:** Lucide React

## Getting Started

### Prerequisites

*   Node.js (v18 or later recommended)
*   npm or yarn
*   Firebase Account & Project

### Firebase Setup

1.  **Create a Firebase Project:** Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project (or use an existing one).
2.  **Register Web App:** In your Firebase project, add a new Web app.
    *   Give it a nickname (e.g., "SolCraft Web").
    *   Copy the Firebase configuration object provided during setup.
3.  **Enable Firebase Services:**
    *   **Authentication:** Enable Email/Password sign-in method.
    *   **Firestore:** Create a Firestore database in Native mode. Set up security rules (start with test mode for development, then move to production rules).
    *   **Storage:** Enable Firebase Storage.
4.  **Update Firebase Config:** Replace the placeholder configuration in `src/lib/firebase.ts` with the config object you copied in step 2.
5.  **Service Account (for Admin SDK - Optional for frontend, required for some backend/Genkit operations):**
    *   In Firebase Console: Project settings > Service accounts.
    *   Generate a new private key and download the JSON file.
    *   Store this file securely (e.g., in the root of your project, **ensure it's added to `.gitignore`**).
    *   Set the `GOOGLE_APPLICATION_CREDENTIALS` environment variable to the path of this JSON file (see `.env` setup below).
    *   Set the `FIREBASE_PROJECT_ID` environment variable to your Firebase Project ID.

### Environment Variables

Create a `.env` file in the root of the project. This file is ignored by Git.
Add your Firebase project ID and the path to your service account key:

```env
# .env

# Required for Firebase Admin SDK (used by Genkit locally, and potentially backend functions)
GOOGLE_APPLICATION_CREDENTIALS=./path-to-your-service-account-key.json
FIREBASE_PROJECT_ID=your-firebase-project-id

# Optional: If you have Google AI API keys for Genkit directly
# GOOGLE_API_KEY=your_google_ai_api_key
```

**Note:** The `firebaseConfig` in `src/lib/firebase.ts` is for client-side SDK and is usually not considered secret. However, service account keys are highly sensitive and **must not be committed to version control.**

### Installation

Clone the repository and install the dependencies:

```bash
git clone <repository-url>
cd solcraft-project
npm install
# or
# yarn install
```

### Running the Development Server

To run the Next.js development server:

```bash
npm run dev
# or
# yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the application.

### Running Genkit Development Server

Genkit flows are used for AI functionalities. To run the Genkit development server (which usually includes a local UI for testing flows):

```bash
npm run genkit:dev
# or for watching changes
npm run genkit:watch
```

This will typically start the Genkit developer UI on a different port (e.g., `http://localhost:4000`).

## Key Features

*   **User Authentication:** Email/password signup and login using Firebase Auth.
*   **Profile Management:** Editable user profiles, avatar uploads, wallet connection status.
*   **Dashboard:** Overview of key metrics, balance, portfolio allocation, recent activity.
*   **Token Swapping:** UI for swapping tokens (backend integration pending).
*   **Deposit/Send:** UI for depositing and sending crypto (backend integration pending).
*   **Launchpad:** Discover new token launches (featured, live, upcoming, past).
*   **Staking:** View staking summary and available staking pools.
*   **Tournament Investment:** Browse poker tournaments and view details, including AI-powered risk assessment.
    *   The AI risk assessment uses a Genkit flow (`tournament-risk-assessment.ts`).
*   **Responsive Design:** UI adapts to different screen sizes.
*   **Theming:** Light and Dark mode support using Tailwind CSS and CSS variables.

## Folder Structure

A brief overview of the main directories:

```
/
├── public/                     # Static assets (images, fonts if not from CDN)
├── src/
│   ├── ai/                     # Genkit AI flows and configuration
│   │   ├── flows/              # Specific AI flow implementations
│   │   ├── dev.ts              # Genkit development server entry point
│   │   └── genkit.ts           # Genkit global initialization
│   ├── app/                    # Next.js App Router (pages, layouts)
│   │   ├── (app)/              # Route group for authenticated app sections
│   │   │   ├── dashboard/
│   │   │   ├── profile/
│   │   │   └── ... (other authenticated pages)
│   │   ├── about/
│   │   ├── launchtoken/
│   │   ├── login/
│   │   ├── signup/
│   │   ├── staking/
│   │   ├── tournaments/
│   │   │   └── [id]/           # Dynamic route for tournament details
│   │   ├── globals.css         # Global styles and Tailwind CSS theme
│   │   ├── layout.tsx          # Root layout
│   │   └── page.tsx            # Landing page
│   ├── components/             # Reusable UI components
│   │   ├── dashboard/
│   │   ├── deposit-send/
│   │   ├── launchtoken/
│   │   ├── layout/             # App shell, sidebar, site header
│   │   ├── shared/             # Components shared across multiple features
│   │   ├── social/
│   │   ├── staking/
│   │   ├── swap/
│   │   ├── tournaments/
│   │   └── ui/                 # ShadCN UI components (Button, Card, etc.)
│   ├── docs/                   # Project documentation (like pool architecture)
│   ├── hooks/                  # Custom React hooks (e.g., useToast, useMobile)
│   ├── lib/                    # Utility functions, Firebase config, types, mock data
│   │   ├── firebase.ts
│   │   ├── firebaseAdmin.ts
│   │   ├── mock-data.ts
│   │   ├── types.ts
│   │   └── utils.ts
├── .env                        # Environment variables (DO NOT COMMIT SENSITIVE DATA)
├── .gitignore
├── apphosting.yaml             # Firebase App Hosting configuration
├── components.json             # ShadCN UI configuration
├── next.config.ts              # Next.js configuration
├── package.json
├── tailwind.config.ts          # Tailwind CSS configuration
└── tsconfig.json               # TypeScript configuration
```

## Styling

*   **Tailwind CSS:** Used for utility-first styling. Configuration is in `tailwind.config.ts`.
*   **ShadCN UI:** Provides a set of beautifully designed, accessible components built on Radix UI and Tailwind CSS. Theme variables are defined in `src/app/globals.css`.
*   **CSS Variables:** Used extensively for theming (light/dark mode) in `src/app/globals.css`.
*   **Fonts:** Inter (body) and Poppins (headlines) are imported from Google Fonts in `src/app/layout.tsx`.

## AI Integration (Genkit)

Genkit is used for integrating AI capabilities, primarily leveraging Google AI models (Gemini).

*   **Configuration:** `src/ai/genkit.ts` initializes the global `ai` object.
*   **Flows:** Business logic involving AI is encapsulated in flows (e.g., `src/ai/flows/tournament-risk-assessment.ts`). These flows define input/output schemas (using Zod) and prompts for the AI models.
*   **Development:** The Genkit development server (`npm run genkit:dev`) provides a UI for testing flows locally.

## Firebase Integration

Firebase is used for backend services:

*   **Authentication:** Handles user sign-up and login (`src/app/login/page.tsx`, `src/app/signup/page.tsx`).
*   **Firestore:** Used as the primary database for storing user profiles and potentially other application data. User profiles are created/updated in `src/app/signup/page.tsx` and `src/app/profile/page.tsx`.
*   **Storage:** Used for storing user-uploaded files like avatars (`src/app/profile/page.tsx`).
*   **Client SDK:** Configured in `src/lib/firebase.ts`.
*   **Admin SDK:** Configured in `src/lib/firebaseAdmin.ts` (mainly for backend/Genkit operations).

## Deployment

The project is configured for deployment using **Firebase App Hosting**. The configuration can be found in `apphosting.yaml`.

Firebase App Hosting handles building and deploying the Next.js application.

## Available Scripts

In the project directory, you can run:

*   `npm run dev`: Runs the Next.js app in development mode with Turbopack.
*   `npm run genkit:dev`: Starts the Genkit development server.
*   `npm run genkit:watch`: Starts the Genkit development server and watches for file changes.
*   `npm run build`: Builds the Next.js app for production.
*   `npm run start`: Starts the Next.js production server.
*   `npm run lint`: Lints the project using Next.js's built-in ESLint configuration.
*   `npm run typecheck`: Runs TypeScript to check for type errors.

## Contributing

(Placeholder) Contributions are welcome! Please follow the existing code style and ensure all tests pass before submitting a pull request.

## License

(Placeholder) This project is licensed under the MIT License.
