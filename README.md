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

## Note

Please note that the **Physics Aviary Tools** (bookmarklet) may not work correctly on all simulations. Some simulations may have restrictions or compatibility issues that prevent the tools from functioning as expected.

---
<div class="pa-card">
<h2>Universal bookmarklet</h2>

This bookmarklet automatically rounds values to two decimal places.

<div class="pa-code">

```javascript
javascript:(function(){try{var percentOff=prompt("Enter MAXIMUM percentage error for answers (e.g., 1 for up to 1% off).\n\nNote: Errors above 2% will likely be too far off.\nEach answer will get a random error between 0% and your maximum.","0");if(percentOff===null){return}percentOff=parseFloat(percentOff);if(isNaN(percentOff)){percentOff=0}if(window.$){$("#SpaceForAnswer").show();$("#SystemMessage").show()}if(typeof SubmitForm==="function"){SubmitForm()}function roundTwoDecimals(x){return Math.round(x*100)/100}var answers=[];for(var i=1;i<=10;i++){if(typeof window["Answer"+i]!=="undefined"){answers.push(window["Answer"+i])}}for(var j=0;j<answers.length;j++){var inputId="#A"+(j+1);if(window.$&&$(inputId).length){var randomPercent=Math.random()*percentOff;var multiplier=1-(randomPercent/100);var adjustedAnswer=answers[j]*multiplier;$(inputId).val(roundTwoDecimals(adjustedAnswer))}}if(typeof SubmitForm==="function"){SubmitForm()}}catch(e){alert("Error: "+e.message)}})();
