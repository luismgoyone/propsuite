# Baguio ↔ Bataan trip cost splitter

A single-file web app that splits gas + tolls fairly between passengers on a
Baguio ↔ Morong, Bataan road trip. Models the different unleaded prices in
Baguio vs. Bataan and the different fuel economy going down vs. climbing back
up to Baguio.

## How to run it

It's a fully static, self-contained HTML file (Tailwind via CDN, vanilla JS).
No build step, no server, no dependencies.

**Option A — just double-click it.** Open `trip-calc/index.html` in any modern
browser (Chrome, Safari, Firefox, Edge). Done.

**Option B — serve it locally** if your browser blocks `file://` features:

```bash
cd trip-calc
python3 -m http.server 8000
# then open http://127.0.0.1:8000 in your browser
```

## Inputs

- **Trip type** — round trip or one way (Baguio → Bataan)
- **Number of persons** sharing the cost
- **Gas price in Baguio** (₱/L, unleaded)
- **Gas price in Bataan** (₱/L, unleaded)
- **Mileage going down** (km/L, Baguio → Bataan — usually better, lots of descents)
- **Mileage going up** (km/L, Bataan → Baguio — worse, the climb back to the mountains)
- **Distance one way** (km) — default 280
- **Toll one way** (₱) — default 932 (TPLEX + NLEX + SCTEX, Class 1)
- **Driver pays?** — toggle to exclude the driver from the split

## Math

Assumption: you fuel up in Baguio before heading down, and fuel up in Bataan
before heading back up — so you take advantage of the cheaper Bataan pump
price on the harder, fuel-hungry leg.

- Down-leg fuel cost = `(distance ÷ mileageDown) × priceBaguio`
- Up-leg fuel cost = `(distance ÷ mileageUp) × priceBataan` (round trip only)
- Toll cost = `toll × (1 if one-way else 2)`
- Per person = `total ÷ payers`, where `payers = persons − 1` if driver is excluded

## Defaults reference

Sourced from a Google AI Overview for "im from baguio and im driving to morong bataan":

- Distance Baguio → Morong, Bataan ≈ 280 km
- TPLEX (Sison → La Paz) ≈ ₱290 (Class 1)
- NLEX (SCTEX interchange segment) ≈ ₱42 (Class 1)
- SCTEX (Tarlac → SBMA Tipo) ≈ ₱600 (Class 1)
- **One-way toll total ≈ ₱932** (Class 1)

Mileage defaults (14 km/L down, 9 km/L up) are a ballpark for a sedan — tune
them to your own vehicle's real numbers.

All math runs in the browser. Nothing is uploaded anywhere.
