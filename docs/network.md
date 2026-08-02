# Network Graph

Interactive relationship graph for the early-origins scientist / funding / documentation cluster.

<div id="truth-network" style="height: 680px; width: 100%; border-radius: 8px; border: 1px solid var(--md-default-fg-color--lightest); margin: 1.2rem 0; background: #fafafa; position: relative;">
  <div id="truth-network-status" style="position:absolute; inset:0; display:flex; align-items:center; justify-content:center; color:#666; font-size:0.95rem;">
    Loading network graph…
  </div>
</div>

<p style="font-size: 0.85rem; color: var(--md-default-fg-color--light); margin-top: -0.5rem;">Drag nodes · Scroll to zoom · Click a node or edge for details.</p>

**Data files:**
- [`graphs/covid-network-v2-nodes.csv`](https://github.com/Truth-Map/truth-map/blob/main/graphs/covid-network-v2-nodes.csv)
- [`graphs/covid-network-v2-edges.csv`](https://github.com/Truth-Map/truth-map/blob/main/graphs/covid-network-v2-edges.csv)
- [`graphs/covid-network-v2.graphml`](https://github.com/Truth-Map/truth-map/blob/main/graphs/covid-network-v2.graphml) (Gephi / yEd / Cytoscape ready)

### What the edges capture
- Funding relationships (NIH → EcoHealth → WIV)
- 31 Jan / 1 Feb 2020 call participation (from Fauci diary)
- Proximal Origin co-authorship
- Diary documentation links
- Lancet letter organization
- Later document releases (Gabbard / DNI materials)
- 51-letter organizing cluster

---

## Static layouts & Gephi export (V2)

For publication-ready static images or deeper layout work:

1. Download the node and edge CSVs (or the GraphML file) linked above.  
2. Open in **[Gephi](https://gephi.org/)** (File → Open → select the `.graphml` or import the two CSVs as nodes + edges).  
3. Run a layout (ForceAtlas2, Fruchterman-Reingold, or Kamada-Kawai).  
4. Export as PNG or SVG for reports, README, or slides.

The interactive view above is the live exploration surface. The GraphML + CSVs are the durable export layer for static publication layouts.

---

<script src="https://cdn.jsdelivr.net/npm/vis-network@9.1.9/standalone/umd/vis-network.min.js" crossorigin="anonymous"></script>

<script>
(function () {
  var statusEl = document.getElementById('truth-network-status');
  var container = document.getElementById('truth-network');
  if (!container) return;
  if (container.dataset.initialized === '1') return;
  container.dataset.initialized = '1';

  function setStatus(msg, isError) {
    if (statusEl) {
      statusEl.textContent = msg;
      statusEl.style.color = isError ? '#c62828' : '#666';
    }
  }

  function buildGraph() {
    if (typeof vis === 'undefined' || !vis.Network) {
      setStatus('Failed to load graph library. Try refreshing the page.', true);
      return;
    }

    try {
      var nodes = new vis.DataSet([
        {id: 'fauci', label: 'Anthony Fauci', group: 'Government', title: 'Former NIAID Director. Oversaw grants to EcoHealth. Key participant in early 2020 calls and public messaging.'},
        {id: 'daszak', label: 'Peter Daszak', group: 'Organization', title: 'President of EcoHealth Alliance. PI on NIH bat-coronavirus grant with WIV subawards. Organized early Lancet letter.'},
        {id: 'shi', label: 'Shi Zhengli', group: 'Lab', title: 'Leading coronavirus researcher at Wuhan Institute of Virology. Bat CoV sampling and experimental work.'},
        {id: 'ecohealth', label: 'EcoHealth Alliance', group: 'Organization', title: 'U.S. nonprofit that received NIH funding and subawarded to WIV. Later debarred.'},
        {id: 'wiv', label: 'Wuhan Institute of Virology', group: 'Lab', title: 'Chinese lab that received U.S. subawards via EcoHealth for bat coronavirus research. BSL-4 facility.'},
        {id: 'nih', label: 'NIH / NIAID', group: 'Government', title: 'Primary U.S. funder of the EcoHealth–WIV bat coronavirus work under Fauci.'},
        {id: 'farrar', label: 'Jeremy Farrar', group: 'Organization', title: 'Then Director of Wellcome Trust. Participant in early Feb 2020 calls on origins.'},
        {id: 'andersen', label: 'Kristian Andersen', group: 'Science', title: 'Scripps researcher. Lead author of Proximal Origin paper. Private communications showed higher lab-origin probability than published paper.'},
        {id: 'collins', label: 'Francis Collins', group: 'Government', title: 'Former NIH Director. Participant in early 2020 calls.'},
        {id: 'holmes', label: 'Edward Holmes', group: 'Science', title: 'Evolutionary biologist (Sydney). Participant on 31 Jan / 1 Feb 2020 calls; co-author Proximal Origin.'},
        {id: 'drosten', label: 'Christian Drosten', group: 'Science', title: 'German virologist. Participant on 1 Feb 2020 call; sided with natural-origin view on that call.'},
        {id: 'fouchier', label: 'Ron Fouchier', group: 'Science', title: 'Dutch virologist (Erasmus). Participant on 1 Feb 2020 call; argued natural origin and against pursuing lab scenario. H5N1 GOF researcher.'},
        {id: 'garry', label: 'Robert Garry', group: 'Science', title: 'Tulane virologist. Participant on 1 Feb 2020 call; co-author Proximal Origin.'},
        {id: 'rambaut', label: 'Andrew Rambaut', group: 'Science', title: 'Edinburgh evolutionary biologist. Participant on 1 Feb 2020 call; co-author Proximal Origin.'},
        {id: 'wellcome', label: 'Wellcome Trust', group: 'Organization', title: 'Major UK biomedical foundation. Jeremy Farrar was Director during the early 2020 origins discussions.'},
        {id: 'lancet', label: 'Lancet letter (2020)', group: 'Event', title: 'Early letter organized by Peter Daszak that helped frame lab-origin discussion as fringe / conspiracy territory.'},
        {id: 'proximal_origin', label: 'Proximal Origin paper', group: 'Event', title: 'Nature Medicine paper (Mar 2020) concluding virus not laboratory construct. Same scientist cluster as 1 Feb call.'},
        {id: 'fauci_diary', label: 'Fauci Diary (2026)', group: 'Document', title: 'Private contemporaneous notes (esp. 26 Jan–1 Feb 2020 and 2021 defensiveness entries). Primary evidence of knowledge and framing.'},
        {id: 'gabbard', label: 'Tulsi Gabbard (DNI)', group: 'Government', title: 'Director of National Intelligence. Released COVID / Fauci / WIV and related materials 2025–2026.'},
        {id: 'ukraine_labs', label: 'Ukraine labs (BTRP)', group: 'Location', title: 'U.S.-supported laboratory capacity in Ukraine under DTRA Biological Threat Reduction Program.'},
        {id: 'dtra', label: 'DTRA', group: 'Government', title: 'Defense Threat Reduction Agency – funded Biological Threat Reduction Program sites.'},
        {id: 'c03_claim', label: 'C-03 (No GOF at WIV)', group: 'Claim', title: 'Claim: NIH has not ever and does not now fund gain-of-function research at the Wuhan Institute of Virology (Fauci Senate testimony, May 2021).'}
      ]);

      var edges = new vis.DataSet([
        {from: 'fauci', to: 'ecohealth', label: 'funded', title: 'funded (2014-2020) · High confidence · NIH grant records'},
        {from: 'ecohealth', to: 'wiv', label: 'subawarded', title: 'subawarded_funds (2014-2020) · High · NIH grant documents'},
        {from: 'fauci', to: 'daszak', label: 'oversaw', title: 'oversaw_grant (2014-2020) · High'},
        {from: 'daszak', to: 'shi', label: 'collaborated', title: 'collaborated (2010s-2020) · High · Joint papers, grant reports'},
        {from: 'fauci', to: 'farrar', label: 'call', title: 'participated_in_call (2020-02) · High · Fauci diary, FOIA'},
        {from: 'fauci', to: 'andersen', label: 'call', title: 'participated_in_call (2020-02) · High'},
        {from: 'fauci', to: 'collins', label: 'call', title: 'participated_in_call (2020-02) · High'},
        {from: 'andersen', to: 'fauci', label: 'Proximal Origin', title: 'proximal_origin_paper (2020-02) · High · Nature Medicine, FOIA Slack'},
        {from: 'fauci', to: 'nih', label: 'led', title: 'led (1984-2022) · High'},
        {from: 'nih', to: 'ecohealth', label: 'funded', title: 'funded (2014-2020) · High'},
        {from: 'daszak', to: 'lancet', label: 'organized', title: 'organized (2020-02) · High'},
        {from: 'farrar', to: 'wellcome', label: 'directed', title: 'directed (2013-2023) · High'},
        {from: 'collins', to: 'nih', label: 'led', title: 'led (2009-2021) · High'},
        {from: 'fauci', to: 'holmes', label: 'call', title: 'participated_in_call (2020-01-31) · High · Fauci diary'},
        {from: 'farrar', to: 'andersen', label: 'convened', title: 'convened_call (2020-01-31) · High · Fauci diary, FOIA'},
        {from: 'farrar', to: 'holmes', label: 'call', title: 'participated_in_call (2020-01-31) · High'},
        {from: 'fauci', to: 'drosten', label: 'call', title: 'participated_in_call (2020-02-01) · High · Diary 1 Feb'},
        {from: 'fauci', to: 'fouchier', label: 'call', title: 'participated_in_call (2020-02-01) · High'},
        {from: 'fauci', to: 'garry', label: 'call', title: 'participated_in_call (2020-02-01) · High'},
        {from: 'fauci', to: 'rambaut', label: 'call', title: 'participated_in_call (2020-02-01) · High'},
        {from: 'andersen', to: 'holmes', label: 'co-authored', title: 'co_authored (2020-02/03) · High · Proximal Origin'},
        {from: 'andersen', to: 'garry', label: 'co-authored', title: 'co_authored (2020-02/03) · High'},
        {from: 'andersen', to: 'rambaut', label: 'co-authored', title: 'co_authored (2020-02/03) · High'},
        {from: 'holmes', to: 'rambaut', label: 'co-authored', title: 'co_authored (2020-02/03) · High'},
        {from: 'proximal_origin', to: 'andersen', label: 'authored by', title: 'authored_by (2020-03) · High'},
        {from: 'proximal_origin', to: 'fauci', label: 'promoted by', title: 'promoted_by (2020-03) · High · Public statements'},
        {from: 'fauci_diary', to: 'fauci', label: 'authored by', title: 'authored_by (2019-2022) · High · Paul Senate release July 2026'},
        {from: 'fauci_diary', to: 'farrar', label: 'documents', title: 'documents_call_with (2020-01-31) · High'},
        {from: 'fauci_diary', to: 'andersen', label: 'documents', title: 'documents_call_with (2020-01-31) · High'},
        {from: 'shi', to: 'fauci', label: 'referenced', title: 'referenced_in_diary (2020-02-01) · High · Diary notes Shi GOF work'},
        {from: 'gabbard', to: 'fauci', label: 'released docs', title: 'released_documents_on (2025-2026) · High · DNI releases'},
        {from: 'gabbard', to: 'wiv', label: 'released mats', title: 'released_materials_on (2025-2026) · High'},
        {from: 'gabbard', to: 'ecohealth', label: 'released mats', title: 'released_materials_on (2026-06) · High'},
        {from: 'gabbard', to: 'ukraine_labs', label: 'released mats', title: 'released_materials_on (2026-06-12) · High · ODNI biolabs release'},
        {from: 'dtra', to: 'ukraine_labs', label: 'funded', title: 'funded (2005-2022) · High · DTRA BTRP ~$200M / ~46 sites'},
        {from: 'fauci', to: 'c03_claim', label: 'subject of', title: 'subject_of (2021-05) · High · Senate testimony 11 May 2021'},
        {from: 'drosten', to: 'lancet', label: 'signed', title: 'signed (2020-02) · High'},
        {from: 'farrar', to: 'lancet', label: 'signed', title: 'signed (2020-02) · High'}
      ]);

      var options = {
        nodes: {
          shape: 'dot',
          size: 18,
          font: { size: 13, face: 'system-ui, -apple-system, sans-serif', color: '#222' },
          borderWidth: 2,
          shadow: true
        },
        edges: {
          width: 1.5,
          color: { color: '#888', highlight: '#c62828', hover: '#c62828' },
          arrows: { to: { enabled: true, scaleFactor: 0.55 } },
          font: { size: 10, align: 'middle', color: '#555', strokeWidth: 3, strokeColor: '#fff' },
          smooth: { type: 'continuous' }
        },
        groups: {
          Government:  { color: { background: '#1565c0', border: '#0d47a1' } },
          Organization:{ color: { background: '#6a1b9a', border: '#4a148c' } },
          Lab:         { color: { background: '#c62828', border: '#b71c1c' } },
          Science:     { color: { background: '#2e7d32', border: '#1b5e20' } },
          Event:       { color: { background: '#ef6c00', border: '#e65100' } },
          Document:    { color: { background: '#00838f', border: '#006064' } },
          Location:    { color: { background: '#5d4037', border: '#3e2723' } },
          Claim:       { color: { background: '#ad1457', border: '#880e4f' } }
        },
        physics: {
          enabled: true,
          barnesHut: {
            gravitationalConstant: -14000,
            centralGravity: 0.2,
            springLength: 150,
            springConstant: 0.04,
            damping: 0.35
          },
          stabilization: { iterations: 150 }
        },
        interaction: {
          hover: true,
          tooltipDelay: 100,
          navigationButtons: true,
          keyboard: true
        }
      };

      if (statusEl) statusEl.remove();
      var network = new vis.Network(container, { nodes: nodes, edges: edges }, options);
      network.once('stabilizationIterationsDone', function () {});

    } catch (err) {
      console.error('Truth Map network error:', err);
      setStatus('Graph failed to initialize. See browser console for details.', true);
    }
  }

  var attempts = 0;
  function tryInit() {
    attempts += 1;
    if (typeof vis !== 'undefined' && vis.Network) {
      buildGraph();
    } else if (attempts < 40) {
      setTimeout(tryInit, 100);
    } else {
      setStatus('Could not load the graph library (CDN). Please refresh or try again later.', true);
    }
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', function () { setTimeout(tryInit, 50); });
  } else {
    setTimeout(tryInit, 50);
  }
})();
</script>
