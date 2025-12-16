<!-- ===================== -->
<!--  Physics Aviary Tools -->
<!-- ===================== -->

<style>
  .pa-container {
    max-width: 900px;
    margin: auto;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    line-height: 1.6;
  }

  .pa-title {
    text-align: center;
    font-size: 2.6em;
    font-weight: 700;
    color: #1f2937;
    margin-bottom: 0.2em;
  }

  .pa-subtitle {
    text-align: center;
    color: #6b7280;
    margin-bottom: 2em;
  }

  .pa-card {
    background: #f9fafb;
    border: 1px solid #e5e7eb;
    border-radius: 12px;
    padding: 1.5em;
    margin-bottom: 1.5em;
  }

  .pa-card h2 {
    margin-top: 0;
    color: #111827;
  }

  .pa-code {
    background: #0f172a;
    color: #e5e7eb;
    padding: 1em;
    border-radius: 8px;
    overflow-x: auto;
    font-size: 0.9em;
  }

  .pa-warning {
    background: #fff7ed;
    border: 1px solid #fed7aa;
    border-radius: 10px;
    padding: 1em;
    color: #9a3412;
  }

  .pa-footer {
    text-align: center;
    font-size: 0.9em;
    color: #6b7280;
    margin-top: 3em;
  }
</style>

<div class="pa-container">

<div class="pa-title">Physics Aviary Tools</div>
<div class="pa-subtitle">
Client side bookmarklets and debugging helpers for Physics Aviary simulations
</div>

---

<div class="pa-card">
<h2>What this is</h2>

This repository contains **bookmarklets and helper scripts** designed to work with  
Physics Aviary simulations that calculate answers entirely on the client side.

These tools:
- Detect internally computed answers
- Automatically populate input fields
- Apply safe rounding when needed
- Resubmit and redraw the results screen

They are intended for **learning, debugging, and testing**, not for server bypassing.

</div>

---

<div class="pa-card">
<h2>How it works</h2>

Physics Aviary simulations:
- Compute correct answers in JavaScript
- Store them in variables such as `Answer1`, `Answer2`, etc
- Compare user input using percent error
- Render results on a canvas

These tools run **in the same execution context** as the simulation and simply reuse
the values already calculated by the page.

No network requests are intercepted.  
No servers are modified.

</div>

---

<div class="pa-card">
<h2>Universal bookmarklet</h2>

This bookmarklet works for simulations with **1 to many answers** and automatically
rounds values to one decimal place.

<div class="pa-code">

```javascript
javascript:(function(){
	try{
		if (window.$){
			$("#SpaceForAnswer").show();
			$("#SystemMessage").show();
		}

		if (typeof SubmitForm === "function"){
			SubmitForm();
		}

		function roundOneDecimal(x){
			return Math.round(x * 10) / 10;
		}

		var answers = [];
		for (var i = 1; i <= 10; i++){
			if (typeof window["Answer" + i] !== "undefined"){
				answers.push(window["Answer" + i]);
			}
		}

		for (var j = 0; j < answers.length; j++){
			var inputId = "#A" + (j + 1);
			if (window.$ && $(inputId).length){
				$(inputId).val(roundOneDecimal(answers[j]));
			}
		}

		if (typeof SubmitForm === "function"){
			SubmitForm();
		}
	}catch(e){
		alert("Bookmarklet error: " + e.message);
	}
})();
