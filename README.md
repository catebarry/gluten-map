# Crowdsourced Gluten Sensitivity Map

An interactive, self-updating web map visualizing patient-reported gluten tolerance experiences across restaurants, cafés, and cities worldwide. Built with Leaflet.js and powered by a moderated Google Forms + Google Sheets pipeline.

- **Live Standalone Map:** `https://catebarry.github.io/gluten-map/`
- **Embeddable Map (Header Hidden):** `https://catebarry.github.io/gluten-map/?embed=1`
- **Submission Form:** `https://docs.google.com/forms/d/e/1FAIpQLSfaPLZ9hZyHF7ltlOxbigcDf0xU8wInOh7pZye_mi765eKnOQ/viewform?usp=header`
- **Moderation:** Managed via private Google Sheet

---

## Architecture & Data Pipeline

1. **Data Collection (Google Forms):** Users submit the restaurant/café name, city/region, country, experience rating (`Symptom-Free`, `Mild Symptoms`, `Bad Symptoms`), and optional notes.
2. **Automated Geocoding (Google Apps Script):** An `On form submit` trigger runs `Maps.newGeocoder()` on new submissions to populate `Latitude` and `Longitude` columns automatically.
3. **Moderation Queue (Google Sheets):**
   - New submissions land in the `Form Responses 1` tab with an unchecked `Approved` checkbox.
   - The `Public Map Data` tab uses `=IFERROR(FILTER(...), "")` to surface **only** rows where `Approved = TRUE`.
   - `Public Map Data` is published to the web as a live CSV feed.
4. **Frontend Map (`index.html`):**
   - Fetches the published CSV on page load via **PapaParse**.
   - Renders color-coded SVG pins on keyless **OpenStreetMap** tiles via **Leaflet.js** without clustering so individual locations remain distinct at every zoom level.
---

## Moderating New Entries

1. Open the private moderation Google Sheet (`Form Responses 1` tab).
2. Review new rows for spam, duplicate submissions, and valid coordinates.
3. Check the **Approved** box in the final column—the pin will appear automatically on the live map within a few minutes.

---

## Embedding on a Website

Paste the following snippet into any HTML/Embed block on the target website (uses `?embed=1` to hide the standalone page header):

```html
<iframe
  src="[https://catebarry.github.io/gluten-map/?embed=1](https://catebarry.github.io/gluten-map/?embed=1)"
  title="Crowdsourced Gluten Sensitivity Map"
  width="100%"
  height="620"
  style="border: 0; border-radius: 8px;"
  loading="lazy">
</iframe>