<!-- ===================== -->
<!--  Physics Aviary Tools -->
<!-- ===================== -->

# Physics Aviary Tools

<h2>What this is</h2>

This repository contains **bookmarklets and helper scripts** designed to work with  
Physics Aviary simulations that calculate answers entirely on the client side.

These tools:
- Detect internally computed answers
- Automatically populate input fields
- Apply safe rounding when needed
- Resubmit and redraw the results screen

They are intended for **learning, debugging, and testing**, not for cheating ;).

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
