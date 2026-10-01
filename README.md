# FixIt Home — Android app

The **homeowner** app for [FixIt Home](https://fixit-web-rom.vercel.app), a live-bidding
marketplace for home repairs. Post a job, watch verified local contractors bid on it in real
time, and pay only once the work is confirmed.

- **Android package:** `com.fixit.app`
- **Expo slug:** `fixit`
- **Role:** homeowner, fixed in `src/brand.ts`

The role is hard-coded rather than read from `EXPO_PUBLIC_ROLE`, so this repository can only
ever build a homeowner APK. There is no way to accidentally ship the contractor build from here.

## Stack

Expo SDK 51 · React Native 0.74 · expo-router 3 · NativeWind 4 (Tailwind) · Supabase ·
React Stripe Native · react-native-maps / camera / location

## Getting started

```bash
npm install --legacy-peer-deps
cp .env.example .env        # fill in Supabase URL + anon key
npm run dev                 # then press 'a' for the Android emulator
```

## Scripts

| Command | What it does |
| --- | --- |
| `npm run dev` | Start the Expo dev server |
| `npm run android` | Build and run on a connected device/emulator |
| `npm run prebuild` | Generate the native `android/` project |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run lint` | ESLint over `src/` |

## Building the APK

Push to `main`, or run the workflow manually. GitHub Actions builds a release APK with
Java 21 and uploads it as the `fixit-home-apk` artifact.

The workflow reads three repository secrets:

| Secret | Purpose |
| --- | --- |
| `EXPO_PUBLIC_SUPABASE_URL` | Supabase project URL |
| `EXPO_PUBLIC_SUPABASE_ANON_KEY` | Supabase anon key (safe to ship) |
| `EXPO_PUBLIC_API_URL` | Web API base URL |

## Structure

```
app/                expo-router screens
  (auth)/           login, signup, role gate
  (homeowner)/      job feed, broadcast, live radar
  chat/             per-job messaging
  bid.tsx           bid review
src/
  brand.ts          fixed homeowner role + role routing
  shared/           constants, types and helpers (vendored, no workspace needed)
  components/       shared UI
  lib/              supabase client, api helpers
  theme.ts          colours and spacing
assets/             icon, adaptive icon, splash
```

`src/shared/` is a vendored copy of the monorepo's `packages/shared`, so this repo installs,
builds and releases entirely on its own.

## Related repositories

- [`fixit-landing`](https://github.com/mhklogs/fixit-landing) — the website this app ships against
- [`profixit-android`](https://github.com/mhklogs/profixit-android) — the contractor app