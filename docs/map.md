# Map

Interactive geographic view of key locations tied to the COVID origins knowledge base (labs, organizations, early outbreak sites).

<div id="truth-map" style="height: 620px; width: 100%; border-radius: 8px; border: 1px solid var(--md-default-fg-color--lightest); margin: 1.2rem 0; z-index: 1;"></div>

<div style="display: flex; flex-wrap: wrap; gap: 1rem; margin-bottom: 1.5rem; font-size: 0.9rem;">
  <span><span style="display:inline-block;width:14px;height:14px;border-radius:50%;background:#c62828;margin-right:6px;vertical-align:middle;"></span> COVID Core</span>
  <span><span style="display:inline-block;width:14px;height:14px;border-radius:50%;background:#1565c0;margin-right:6px;vertical-align:middle;"></span> High-Containment Labs</span>
</div>

**Data source:** [`maps/covid-origins-layered-v7.csv`](https://github.com/BnBCleaners/truth-map/blob/main/maps/covid-origins-layered-v7.csv) (Kepler.gl ready)

### Advanced exploration
For full layer control, time filtering, and custom styling:

1. Open [kepler.gl/demo](https://kepler.gl/demo)
2. Upload the CSV above
3. Color by `layer_group`, enable the time filter on the `date` field

---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<script>
(function() {
  const points = [
    {id:1, name:"Wuhan Institute of Virology (WIV)", lat:30.3764, lng:114.2625, category:"Lab", group:"COVID Core", desc:"Premier Chinese SARS-related coronavirus research center. BSL-4 facility. Extensive bat CoV work including RaTG13 and chimeric constructs under EcoHealth collaboration.", conf:"High"},
    {id:2, name:"Huanan Seafood Wholesale Market", lat:30.6196, lng:114.2576, category:"Location", group:"COVID Core", desc:"Early case cluster location. Environmental samples later tested positive for SARS-CoV-2. Live mammals present.", conf:"High"},
    {id:3, name:"EcoHealth Alliance (NY HQ)", lat:40.7484, lng:-73.9857, category:"Organization", group:"COVID Core", desc:"U.S. nonprofit that received NIH funding and subawarded bat coronavirus work to WIV. Later debarred by HHS.", conf:"High"},
    {id:4, name:"NIH / NIAID (Bethesda)", lat:39.0028, lng:-77.1031, category:"Government", group:"COVID Core", desc:"Primary U.S. funder of the EcoHealth–WIV bat coronavirus work under Fauci.", conf:"High"},
    {id:5, name:"USAMRIID / Fort Detrick", lat:39.4350, lng:-77.4270, category:"Lab", group:"High-Containment Labs", desc:"U.S. Army BSL-4. Defensive biodefense mission. Comparative node to WIV. 2019 CDC select-agent suspension.", conf:"High"},
    {id:6, name:"Galveston National Laboratory", lat:29.3100, lng:-94.7750, category:"Lab", group:"High-Containment Labs", desc:"NIAID National Biocontainment Laboratory (BSL-4). Documented formal collaboration/training relationship with WIV.", conf:"High"},
    {id:7, name:"NEIDL (Boston University)", lat:42.3370, lng:-71.1000, category:"Lab", group:"High-Containment Labs", desc:"Second NIAID National Biocontainment Laboratory. BSL-4 approved 2017.", conf:"High"},
    {id:8, name:"CDC Atlanta High-Containment", lat:33.7490, lng:-84.3880, category:"Lab", group:"High-Containment Labs", desc:"CDC high-containment laboratories.", conf:"High"},
    {id:9, name:"NIAID IRF-Frederick", lat:39.4350, lng:-77.4100, category:"Lab", group:"High-Containment Labs", desc:"NIAID Integrated Research Facility at Fort Detrick. Part of intramural high-containment cluster.", conf:"High"},
    {id:10, name:"Rocky Mountain Laboratories (RML)", lat:46.4000, lng:-114.1500, category:"Lab", group:"High-Containment Labs", desc:"NIAID Rocky Mountain Laboratories (Hamilton, MT). High-containment research.", conf:"High"}
  ];

  // Wait for DOM + Leaflet
  function initMap() {
    if (typeof L === 'undefined') {
      setTimeout(initMap, 50);
      return;
    }
    const el = document.getElementById('truth-map');
    if (!el || el._leaflet_id) return;

    const map = L.map('truth-map', {
      scrollWheelZoom: false,
      worldCopyJump: true
    }).setView([35, 0], 2);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>',
      maxZoom: 18
    }).addTo(map);

    // Enable scroll after click (common pattern)
    map.on('click', function() { map.scrollWheelZoom.enable(); });

    const colors = {
      'COVID Core': '#c62828',
      'High-Containment Labs': '#1565c0'
    };

    points.forEach(function(p) {
      const color = colors[p.group] || '#666';
      const marker = L.circleMarker([p.lat, p.lng], {
        radius: 9,
        fillColor: color,
        color: '#fff',
        weight: 2,
        opacity: 1,
        fillOpacity: 0.9
      }).addTo(map);

      const popup = '<div style="min-width:220px;font-family:system-ui,sans-serif;line-height:1.4">' +
        '<strong style="font-size:1.05em">' + p.name + '</strong><br>' +
        '<span style="color:#666;font-size:0.85em">' + p.category + ' · ' + p.group + '</span><br><br>' +
        '<div style="font-size:0.9em">' + p.desc + '</div>' +
        '<div style="margin-top:8px;font-size:0.8em;color:#555">Confidence: ' + p.conf + '</div></div>';

      marker.bindPopup(popup);
    });

    // Fit bounds with padding
    const group = L.featureGroup(points.map(function(p) {
      return L.marker([p.lat, p.lng]);
    }));
    map.fitBounds(group.getBounds().pad(0.25));
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', initMap);
  } else {
    initMap();
  }
})();
</script>
