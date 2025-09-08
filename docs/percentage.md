# % Calculator

## How to Use?

Enter how many seconds each member sang in the song:

<form id="calc-form">
  <label>First member: <input type="number" id="v1" step="any"></label><br>
  <label>Second member: <input type="number" id="v2" step="any"></label><br>
  <label>Thrid member: <input type="number" id="v3" step="any"></label><br><br>
  <button type="button" onclick="calc()">Calculate</button>
</form>

**Results:**
<div id="result"></div>

<script>
function calc() {
  const v1 = parseFloat(document.getElementById('v1').value) || 0;
  const v2 = parseFloat(document.getElementById('v2').value) || 0;
  const v3 = parseFloat(document.getElementById('v3').value) || 0;
  const total = v1 + v2 + v3;

  if (total === 0) {
    document.getElementById('result').innerText = "You must enter values greater than 0.";
    return;
  }

  const p1 = ((v1/total)*100).toFixed(2);
  const p2 = ((v2/total)*100).toFixed(2);
  const p3 = ((v3/total)*100).toFixed(2);

  document.getElementById('result').innerText =
    `First member: ${p1}% | Second member: ${p2}% | Third member: ${p3}%`;
}
</script>
