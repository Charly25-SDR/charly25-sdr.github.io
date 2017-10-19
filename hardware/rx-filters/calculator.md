---
title: Bandpass Filter Calculator
menu_parent: true
---

# [\<<](.) {{page.title}}

<form action="#">
<h2>Input values</h2>
<formset id="input-fields">
    <label>(Ideal X<sub>L</sub> range to be 50-150 &Omega;)<input type="number" id="input-XL" value="108" step="any" />&Omega;</label>
    <br/>
    <label>Assume unloaded Q, Q<sub>u</sub><input type="number" id="input-Qu" value="200" step="any" /></label>
    <br/>
    <label>Frequency <input type="number" id="input-f" value="24.95" step="any" />MHz</label>
    <br/>
    <label>Bandwidth <input type="number" id="input-BW" value="1" step="any" />MHz</label>
    <br/>
    <label for="input-Z">Z<sub>in</sub>/Z<sub>out</sub> <input type="number" id="input-Z" value="50" step="any" />&Omega;</label>
    <br/>
    <label for="input-AL">Core A<sub>L</sub> <input type="number" id="input-AL" value="4" step="any"/></label>
</formset>
<h2>Output values</h2>
<formset>
    <label>L<sub>1</sub> to L<sub>4</sub><input step="any" id="output-L" readonly="true" />&mu;H</label>
    <br/>
    <label>C<sub>12</sub> <input step="any" id="output-C12" readonly="true" />pF</label>
    <br/>
    <label>C<sub>23</sub> <input step="any" id="output-C23" readonly="true" />pF</label>
    <br/>
    <label>C<sub>34</sub> <input step="any" id="output-C34" readonly="true" />pF</label>
    <br/>
    <label>C<sub>1</sub> <input step="any" id="output-C1" readonly="true" />pF</label>
    <br/>
    <label>C<sub>2</sub> <input step="any" id="output-C2" readonly="true" />pF</label>
    <br/>
    <label>C<sub>3</sub> <input step="any" id="output-C3" readonly="true" />pF</label>
    <br/>
    <label>C<sub>4</sub> <input step="any" id="output-C4" readonly="true" />pF</label>
    <br/>
    <label>Turns (L<sub>1</sub> to L<sub>4</sub>) <input step="any" id="output-turnsL" readonly="true" /> turns</label>
    <br/>
    <label>Input/Output turns (L<sub>k</sub>) <input step="any" id="output-turnsLk" readonly="true" /> turns</label>
</formset>
</form>

<script>
function recalculateFilter() {
    let input = function(name) {
        return document.getElementById("input-" + name).value;
    };
    let XL = input("XL");
    let Qu = input("Qu");
    let f = input("f");
    let BW = input("BW");
    let Z = input("Z");;
    let AL = input("AL");
    let q = 0.7654;
    let k12 = 0.8409;
    let k23  = 0.5412;
    let L = XL / (2 * Math.PI * f);
    let omega = 2 * Math.PI * f * 1e6;
    let C0 = 1e18 / (omega * omega * L);
    let QE = q * f * Qu / (BW * Qu - q * f);
    let Rp = omega * L * QE / 1e6;
    let C12 = C0 * k12 * BW / f;
    let C23 = C0 * k23 * BW / f;
    let C1 = C0 - C12;
    let C2 = C0 - C12 - C23;
    let turnsL = Math.sqrt((L / 1e6) / (AL / 1e9));
    let turnsLk = turnsL / Math.sqrt(Rp /  Z);
    let outputFormat = new Intl.NumberFormat("en-GB", { maximumSignificantDigits: 5 });
    let output = function(name, value) {
        document.getElementById("output-" + name).value = outputFormat.format(value);
    };
    output("L", L);
    output("C12", C12);
    output("C23", C23);
    output("C34", C12);
    output("C1", C1);
    output("C2", C2);
    output("C3", C2);
    output("C4", C1);
    output("turnsL", turnsL);
    output("turnsLk", turnsLk);
};
document.querySelectorAll("#input-fields input").forEach(function(el) {
    el.addEventListener("change", function(_) { recalculateFilter(); });
});
recalculateFilter();
</script>

## Toroid core  data

### Colour code

Suffix | Primary Colour | Secondary Colour
--: | :-: | :-:
-1 | Blue | Clear
-2 | Red | Clear
-3 | Grey | Clear
-6 | Yellow | Clear
-7 | White | Clear
-10 | Black | Clear
-12 | Green | White
-15 | Red | White
-17 | Blue | Yellow
-0 | Tan

### Magnetic dimensions

OD = outer diameter; ID = inner diameter; Ht = height

Core Type | Core A<sub>L</sub> (nH/N²) | OD (mm, *in*) | ID (mm, *in*) | Ht (mm, *in*) | t (cm) | A (cm²) | V (cm³)
:-- | :-: | :-: | :-: | :-: | :-: | :-: | :-:
**T37-1** | 8 | 9.53 *(0.375)* | 5.21 *(0.205)* | 3.25 *(0.128)* | 2.31 | 0.064 | 0.147
**T37-2** | 4
T37-3 | 12 
**T37-6** | 3
**T37-7** | 3.2
**T37-10** | 2.5 
**T37-12** | 1.5 
T37-15 | 9 | 
**T37-17** | 1.5 
**T37-0** | 0.49 
--- | --- | --- | --- | --- | --- | --- | ---
**T44-1** | 10.5 | 11.2 *(0.44)* | 5.82 *(0.229)* | 4.04 *(0.159)* | 2.68 | 0.099 | 0.266
**T44-2** | 5.2
T44-3 | 18
**T44-6** | 4.2
**T44-7** | 4.6
**T44-10** | 3.3
**T44-12** | 1.85
**T44-15** | 16
**T44-17** | 1.85
**T44-0** | 0.65
--- | --- | --- | --- | --- | --- | --- | ---
**T44-2 A** | 3.6 | 11.2 *(0.44)* | 5.82 *(0.229)* | 3.25 *(0.128)* | 2.68 | 0.08 | 0.215
--- | --- | --- | --- | --- | --- | --- | ---
T50-1 | 10 | 12.7 *(0.5)* | 7.7 *(0.303)* | 4.83 *(0.19)* | 3.19 | 0.112 | 0.358
**T50-2** | 4.9
**T50-3** | 17.5
**T50-6** | 4
**T50-7** | 4.3
**T50-10** | 3.1
**T50-12** | 1.8
**T50-15** | 13.5
**T50-17** | 1.8
**T50-0** | 0.64
--- | --- | --- | --- | --- | --- | --- | ---
T51-2 B | 13.8 | 12.7 *(0.5)* | 5.08 *(0.2)* | 7.92 *(0.312)* | 2.79 | 0.282 | 0.786
T51-6 B | 10.2
--- | --- | --- | --- | --- | --- | --- | ---
**T60-2** | 6.5 | 15.2 *(0.6)* | 8.53 *(0.336)* | 5.94 *(0.234)* | 3.74 | 0.187 | 0.699
**T60-6** | 5.5
--- | --- | --- | --- | --- | --- | --- | ---
T68-1 | 11.5 | 17.5 *(0.69)* | 9.4 *(4.83)* | 4.83 *(0.19)* | 4.23 | 0.179 | 0.759
**T68-2** | 5.7
T68-3 | 19.5
**T68-6** | 4.7
