# MEMORANDUM OF UNDERSTANDING (MOU) + TECHNICAL DOCUMENTATION

## JELL IT — Interactive 3D Brand World ("TURN YOUR WORLD INTO JELLY")

| Field | Detail |
|---|---|
| Document version | 1.0 |
| Date | 17 September 2026 |
| Project codename | jelly-world |
| Live production URL | https://jelly-world-seven.vercel.app |
| Source repository | https://github.com/manankadel/jelly-world (`main` branch) |
| Status | Live in production, active iteration |

---

## 1. PARTIES & PURPOSE

**1.1 Studio (Executing party).** Blueblood Studio — design, engineering, 3D, deployment, and iteration of the experience end to end.

**1.2 Brand (Client party).** JELL IT (TIC Beverages Pvt. Ltd.) — sugar-free jelly confectionery; 90 g packs (5 × 18 g pieces); flavours: Espresso Martini, Lemon & Lime, Passionfruit & Peach, Pina Colada.

**1.3 Purpose.** This MOU records what is being built, how it is built, what has been delivered, what is pending, and the terms under which both parties proceed — so there is zero ambiguity about scope, ownership, cost drivers, and acceptance.

---

## 2. OBJECTIVES

1. A single-screen, mobile-responsive, interactive 3D brand world where everything obeys jelly physics.
2. A signature interaction: one droplet falls → a shockwave converts the solid world into translucent jelly.
3. A working product layer: the in-world vending machine is the catalog (camera flies in, shopper picks minis off its shelf, product drawer + bag + checkout flow).
4. Production-grade performance: 60–120 FPS on modern phones and desktops.
5. Brand fidelity: real catalog data (pack size, nutrition, zero-sugar positioning), brand voice (playful, premium, Gen-Z), and the official jelly logo engraved on the hero.

---

## 3. SCOPE OF WORK

### 3.1 In scope (agreed build)

**A. 3D environment (Three.js, single `index.html`, ~1,190 lines)**
- Floating voxel island (rounded pixel-glass voxels, seamless instancing), central hero plaza.
- Shaped architecture district: 5 detailed buildings (windows, cornices, parapets, rooftop tanks, doors, awnings), jelly arch gate, bending pond bridge, street lamps, vending machine.
- Hero: 3.6-unit rounded liquid-glass cube with engraved JELL IT logo on 4 faces + orbiting mini-jellies.
- Life: walking robot on a collision-free ring road, falling under-island drips, drifting clouds, starfield, two orbiting jelly satellite rings, 5 flavour portals.
- Jelly material system: `MeshPhysicalMaterial` (transmission ≈ 1.0, thickness 1–4, IOR 1.38–1.52, clearcoat 1) + layered-noise vertex-wobble shader + volume-style squash & stretch.

**B. Signature conversion sequence**
- Idle solid world → click droplet / CTA / `?drop=1` → droplet falls → impact ring + camera kick + radial solid→translucent→jelly wave → `WORLD JELLIED` → full explore mode (poke, drag, fling, ripples, particles, WebAudio blips).
- 5 flavour worlds (Coffee, Mango, Lime, Berry, Cocktail) re-light the entire scene; deep-linkable via `?flavour=` / `?drop=`.

**C. Commerce layer (demo checkout)**
- Vending machine = the only shop trigger. Tap → camera flies into product frame → 4 real-flavour minis on glass shelves → tap mini → right-side product drawer (visual, pack facts, nutrition, price in ₹, Add to Bag).
- Bag persisted in `localStorage`; bag panel overlay with quantities, totals, demo checkout; toast notifications.

**D. Presentation & sound**
- Floating dark-glass navbar (white logo, live links: home reset, flavour worlds, gallery orbit tour, material specs, bag, drop CTA).
- Loader with official jelly logo (PNG when supplied, canvas fallback).
- Procedural 122 BPM techno-house loop (WebAudio, no audio files), autoplay-on-first-gesture with pause/play toggle.

**E. Responsive & deployment**
- Mobile-first responsive pass (compact nav, full-width drawer, touch drag + pinch, `touch-action:none`); desktop unchanged.
- GitHub repo + Vercel production deploys (`vite build` → `dist/`); `vercel.json` pinned (Vite framework).

### 3.2 Out of scope (requires separate written addendum)

1. Real payment gateway / order management / backend / user accounts (current checkout is an explicit demo stub).
2. CMS or self-serve catalog editing (products are currently code constants).
3. Native mobile apps; app-store submission.
4. Additional flavours, languages, or accessibility certification beyond best-effort responsive + semantic HTML.
5. Paid media, SEO retainer, analytics dashboard (no tracking is currently installed).

---

## 4. DELIVERABLES & ACCEPTANCE

| # | Deliverable | Location | Acceptance test |
|---|---|---|---|
| D1 | Production 3D experience | https://jelly-world-seven.vercel.app | Loads < 3 s on 4G; loader clears; 60+ FPS counter; no console errors |
| D2 | Source code + history | https://github.com/manankadel/jelly-world | 22 commits on `main`; builds with `npm run build` with zero errors |
| D3 | Conversion sequence | `?drop=1` on live URL | Droplet falls, shockwave spreads, world jellies, `WORLD JELLIED` shows |
| D4 | Commerce demo | Tap vending machine on live URL | Camera focuses; drawer opens per mini; bag persists after reload; checkout toast shows totals |
| D5 | Brand assets wired | `jell-it-pink.png` in repo root | Loader + hero show official jelly logo (currently pending client file) |
| D6 | This document | `MOU-JELL-IT-3D-WORLD.md` in repo | Signed by both parties |

Acceptance = client confirms D1–D4 on the live URL; D5 closes when the logo file is supplied and deployed.

---

## 5. TECHNICAL ANNEX

**5.1 Stack.** Three.js 0.160.1 · GSAP 3.12 · Vite 5.4 (build only) · vanilla JS/CSS, no framework runtime · zero backend · `localStorage` for bag.

**5.2 Architecture (single-file app).** `index.html` (~1,190 lines) contains all CSS, UI overlays (nav, drawer, bag panel, toast, loader), and one ES module: scene setup → environment/lights → voxel island (instanced per colour) → shaped city → hero + logo decals → robot/clouds/drips/orbiters/portals → interaction (raycast tap-vs-drag, pinch zoom) → conversion state machine → shop/bag/drawer controllers → procedural audio → single `requestAnimationFrame` loop with adaptive pixel ratio.

**5.3 Key engineering decisions.**
- RoundedBox voxels + whole-island breathing (no per-voxel drift) so faces never separate.
- No post-processing bloom and no shadow maps (removed for the 120 FPS budget); glow faked with emissive + additive sprites.
- Terrain stays voxel, buildings are shaped meshes — the "world-to-jelly" read.
- Tap-vs-drag disambiguation (< 7 px, < 450 ms) so orbit/drag never misfires the shop.
- Shop restricted to the vending machine only; obstacle registry + ring-road robot path so nothing walks through walls.

**5.4 Performance budget.** Bundle ≈ 600 KB JS (gzip ≈ 167 KB) + 13 KB HTML; DPR capped 1.75 with adaptive step-down; instanced meshes throughout; target 60 FPS mobile / 90–120 FPS desktop (live FPS meter in bottom bar).

**5.5 Environments.** Production: Vercel (`blueblood-main/jelly-world`, alias `jelly-world-seven.vercel.app`). Preview: `npm run dev` → `http://localhost:5173`. No environment variables required.

**5.6 Known limitations.** Checkout is a stub; catalog edits need a code change + redeploy; autoplay audio waits for first user gesture (browser policy); hero logo uses a canvas fallback until the official PNG is supplied.

---

## 6. COMMERCIAL TERMS (to be countersigned)

6.1 **Fees & milestones.** [To be filled — e.g., 40% on MOU sign, 40% on acceptance of D1–D4, 20% on D5 close.]
6.2 **Change control.** Anything outside §3.1 is a paid change request: estimated in writing, approved before work starts; §§ history shows prior pivots (voxel↔shaped, shop UX) were absorbed pre-signing and are baselined here.
6.3 **IP.** On full payment, all project IP (code, scene, copy written for the build) assigns to the brand; studio retains a portfolio/showcase licence. Third-party libs remain under their OSS licences (Three.js MIT, GSAP, Vite).
6.4 **Brand assets.** Brand warrants it owns the logo/pack artwork and grants studio a licence to use them in the build.
6.5 **Support.** 30 days of bug fixes post-acceptance included (breakage vs. agreed acceptance tests); new features billed separately.
6.6 **Termination.** Either party may terminate with 14 days' written notice; work completed to date is billed pro-rata and the repo state at termination is handed over as-is.

---

## 7. SIGN-OFF

| | Studio (Blueblood) | Brand (JELL IT) |
|---|---|---|
| Name | | |
| Signature | | |
| Date | | |

*Counterparts/e-signatures valid. This MOU becomes binding on both signatures; §§6.1 figures must be completed before or at signing.*
