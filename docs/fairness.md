# Fair Calculator

## How to Use?

Enter the % of each member have in the song (separate by commas, spaces, semicolons, or line breaks):

<form id="std-form" class="std-form">
  <textarea id="vals" rows="6" style="width:100%;" placeholder="e.g., 21.3, 17.9, 14.5, 14.2, 12.7, 11.6, 7.8"></textarea>

  <div style="margin: 0.5rem 0;">
    Decimal places:
    <input type="number" id="decimals" value="2" min="0" max="10" style="width:6rem;">
  </div>

  <button type="button" class="md-button md-button--primary" onclick="calcStd()">Calculate</button>
  <button type="button" class="md-button" onclick="clearStd()">Clear</button>
</form>

**Results:**
<div id="std-result"></div>

<script>
function parseNumbers(input) {
  const tokens = (input || "")
    .replace(/[^\d,.\-+\s;]+/g, " ")
    .split(/[\s,;]+/);

  const nums = [];
  for (const t of tokens) {
    if (!t) continue;
    const normalized = t.replace(",", "."); // support comma decimal
    const x = Number(normalized);
    if (Number.isFinite(x)) nums.push(x);
  }
  return nums;
}

function round(x, d) {
  const f = Math.pow(10, d);
  return Math.round((x + Number.EPSILON) * f) / f;
}

// Classify your fairness metric (you can tweak thresholds/texts)
function classifyDistribution(value) {
  if (value < 10) return "Flawless";
  if (value < 20) return "Perfect";
  if (value < 30) return "Great";
  if (value < 40) return "Good";
  if (value < 50) return "Average";
  if (value < 60) return "Okay";
  if (value < 70) return "Uneven";
  if (value < 80) return "Bad";
  return "Terrible";
}

function calcStd() {
  const raw = document.getElementById('vals').value;
  const dec = Math.min(10, Math.max(0, parseInt(document.getElementById('decimals').value || "2", 10)));

  const data = parseNumbers(raw);
  const n = data.length;
  const out = document.getElementById('std-result');

  if (n === 0) {
    out.innerHTML = '<p style="color:var(--md-accent-fg-color);">You need to enter at least one value.</p>';
    return;
  }

  // mean
  const mean = data.reduce((a,b)=>a+b, 0) / n;

  // sum of squared deviations
  const ss = data.reduce((acc, x) => acc + Math.pow(x - mean, 2), 0);

  // population variance
  const variance = ss / n;

  // population standard deviation
  const std = Math.sqrt(variance);

  // Your metric (kept as std * n) and formatting helpers
  const metric = std * n; // percentage-based metric
  const fmtNum = (x)=> round(x, dec).toFixed(dec);
  const classification = classifyDistribution(metric);

  out.innerHTML = `
    <div class="md-typeset">
      <ul>
        <li>The % percentage difference of this song is <strong>${fmtNum(metric)}%</strong></li>
        <li>This means the distribution is <strong>${classification}</strong>!</li>
      </ul>
    </div>
  `;
}

function clearStd() {
  document.getElementById('vals').value = '';
  document.getElementById('std-result').innerHTML = '';
}
</script>
