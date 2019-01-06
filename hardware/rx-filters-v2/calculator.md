---
title: Rx Filter V2 Calculator
menu_parent: true
---

# [\<<](.) {{page.title}}

## **PROTOTYPE, ONLY FOR TESTING PURPOSES!**

## Inputs

<form action="#">
<h3>Frequency (Low + High or Center + Fractional BW)</h3>
<fieldset id="frequency-selection">
    <fieldset id="frequency-selection-low-high">
        <label><input type="radio" name="frequency-selection" id="frequency-selection-low-high-radio" checked>
        Low <input type="number" id="frequency-selection-low" min="0" step="any" value="7"> MHz;
        High <input type="number" id="frequency-selection-high" min="0" step="any" value="7.2"> MHz</label><br>
    </fieldset>
    <fieldset id="frequency-selection-center-fbw">
        <label><input type="radio" name="frequency-selection" id="frequency-selection-center-fbw-radio">
        Center <input type="number" id="frequency-selection-center" min="0" step="any" readonly> MHz;
        FBW <input type="number" id="frequency-selection-fbw" min="0" max="1" step="any" readonly> (0&ndash;1)</label>
    </fieldset>
</fieldset>
<h3>Ripple, Return Loss or VSWR</h3>
<fieldset id="ripple-return-vswr">
    <label><input type="radio" name="ripple-return-vswr" id="ripple-radio">
    Ripple <input type="number" id="ripple" min="0" step="any" readonly> dB</label><br>
    <label><input type="radio" name="ripple-return-vswr" id="return-radio" checked>
    Return Loss <input type="number" id="return" max="0" step="any" value="-55"> dB (&lt; 0)</label><br>
    <label><input type="radio" name="ripple-return-vswr" id="vswr-radio">
    VSWR <input type="number" id="vswr" min="1" step="any" readonly></label>
</fieldset>
<h3>Coils</h3>
<fieldset id="coils">
    <label><input type="radio" name="coils" id="coils-impedance-radio" checked>
    Impedance <input type="number" id="coils-impedance" min="0" step="any" value="108"> &#x2126;</label><br>
    <label><input type="radio" name="coils" id="coils-inductivity-radio">
    Inductivity <input type="number" id="coils-inductivity" min="0" step="any" readonly> &micro;H</label>
</fieldset>
<h3>Misc Parameters</h3>
<fieldset id="misc-params">
    <label>Order <input type="number" id="order" min="2" step="1" value="4"> (&ge; 2)</label><br>
    <label>Input Impedance <input type="number" id="input-impedance" min="0" step="any" value="50"> &#x2126;</label><br>
    <label>Output Impedance <input type="number" id="output-impedance" min="0" step="any" value="50"> &#x2126;</label>
</fieldset>
<h3>Q Factors (only for filter curve plot)</h3>
<fieldset id="q-factors">
    <label>Capacitors <input type="number" id="capacitor-q-factor" min="0" step="any" value="1000"></label><br>
    <label>Coils <input type="number" id="coils-q-factor" min="0" step="any" value="100"></label>
</fieldset>
</form>

## Result

### Schematic

<div id="calculation-error">Given parameters out of design range</div>

<svg id="schematic-output" width="100%" height="0em"></svg>

### Filter Curve

<div id="curveplot"></div>

<script src="https://unpkg.com/d3@3/d3.min.js"></script>
<script src="https://unpkg.com/function-plot@1/dist/function-plot.js"></script>

<script>
(function(){
const low_high_radio = document.getElementById('frequency-selection-low-high-radio')
const center_fbw_radio = document.getElementById('frequency-selection-center-fbw-radio')

const low_field = document.getElementById('frequency-selection-low')
const high_field = document.getElementById('frequency-selection-high')
const center_field = document.getElementById('frequency-selection-center')
const fbw_field = document.getElementById('frequency-selection-fbw')

const ripple_radio = document.getElementById('ripple-radio')
const return_radio = document.getElementById('return-radio')
const vswr_radio = document.getElementById('vswr-radio')

const ripple_field = document.getElementById('ripple')
const return_field = document.getElementById('return')
const vswr_field = document.getElementById('vswr')

const impedance_radio = document.getElementById('coils-impedance-radio')
const inductivity_radio = document.getElementById('coils-inductivity-radio')

const impedance_field = document.getElementById('coils-impedance')
const inductivity_field = document.getElementById('coils-inductivity')

const order_field = document.getElementById('order')
const input_impedance_field = document.getElementById('input-impedance')
const output_impedance_field = document.getElementById('output-impedance')

const calculation_error = document.getElementById('calculation-error')
const schematic_field = document.getElementById('schematic-output')

const capacity_q_field = document.getElementById('capacitor-q-factor')
const inductivity_q_field = document.getElementById('coils-q-factor')

;[low_high_radio, center_fbw_radio].forEach(function(el) {
    el.addEventListener('input', function(_) {
        if (low_high_radio.checked) {
            low_field.readOnly = false
            high_field.readOnly = false
            center_field.readOnly = true
            fbw_field.readOnly = true
        } else {
            low_field.readOnly = true
            high_field.readOnly = true
            center_field.readOnly = false
            fbw_field.readOnly = false
        }
    })
})

function recalculate_center_fbw() {
    const low = Number(low_field.value)
    const high = Number(high_field.value)
    const center = Math.sqrt(low * high)
    center_field.value = center
    fbw_field.value = (high - low) / center
}
;[low_field, high_field].forEach(function(el) {
    el.addEventListener('input', function(_) {
        recalculate_center_fbw()
        recalculate_coils()
        recalculate_schematic()
    });
})
recalculate_center_fbw()
;[center_field, fbw_field].forEach(function(el) {
    el.addEventListener('input', function(_) {
        // WolframAlpha query used: solve f=(h-k)/x, x=sqrt(k*h) for k
        const center = Number(center_field.value)
        const fbw = Number(fbw_field.value)
        const center_ = Math.sqrt((Math.pow(fbw, 2) + 4) * Math.pow(center, 2))
        low_field.value = 0.5 * (center_ - fbw * center)
        high_field.value = 0.5 * (center_ + fbw * center)
        recalculate_schematic()
    })
})

;[ripple_radio, return_radio, vswr_radio].forEach(function(el) {
    el.addEventListener('input', function(_) {
        if (ripple_radio.checked) {
            ripple_field.readOnly = false
            return_field.readOnly = true
            vswr_field.readOnly = true
        } else if (return_radio.checked) {
            ripple_field.readOnly = true
            return_field.readOnly = false
            vswr_field.readOnly = true
        } else {
            ripple_field.readOnly = true
            return_field.readOnly = true
            vswr_field.readOnly = false
        }
    })
})

function ripple_from_vswr(vswr) {
    return -10 * Math.log10(1 - Math.pow((vswr - 1) / (vswr + 1), 2))
}
function ripple_from_return_loss(return_loss) {
    return -10 * Math.log10(1 - Math.pow(10, 0.1 * return_loss))
}
function return_loss_from_ripple(ripple) {
    return 10 * Math.log10(1 - Math.pow(10, -0.1 * ripple))
}
function vswr_from_ripple(ripple) {
    // WolframAlpha: solve x=-10 * log10(1 - ((y-1)/(y+1))^2) for y > 1
    return (Math.pow(10, 0.1 * ripple)
        * (2 * Math.sqrt(1 - Math.pow(10, -0.1 * ripple))
            - Math.pow(10, -0.1 * ripple)
            + 2))
}

ripple_field.addEventListener('input', function(_) {
    const ripple = Number(ripple_field.value)
    return_field.value = return_loss_from_ripple(ripple)
    vswr_field.value = vswr_from_ripple(ripple)
    recalculate_schematic()
})
function recalculate_from_return_loss() {
    const return_loss = Number(return_field.value)
    const ripple = ripple_from_return_loss(return_loss)
    ripple_field.value = ripple
    vswr_field.value = vswr_from_ripple(ripple)
}
recalculate_from_return_loss()
return_field.addEventListener('input', function(_) {
    recalculate_from_return_loss()
    recalculate_schematic()
})
vswr_field.addEventListener('input', function(_) {
    const vswr = Number(vswr_field.value)
    const ripple = ripple_from_vswr(vswr)
    ripple_field.value = ripple
    return_field.value = return_loss_from_ripple(ripple)
    recalculate_schematic()
})

;[impedance_radio, inductivity_radio].forEach(function(el) {
    el.addEventListener('input', function(_) {
        if (impedance_radio.checked) {
            impedance_field.readOnly = false
            inductivity_field.readOnly = true
        } else {
            impedance_field.readOnly = true
            inductivity_field.readOnly = false
        }
    })
})

function recalculate_inductivity() {
    const impedance = Number(impedance_field.value)
    const center = Number(center_field.value)
    inductivity_field.value = impedance / (2 * Math.PI * center)
}
recalculate_inductivity()
function recalculate_impedance() {
    const inductivity = Number(inductivity_field.value)
    const center = Number(center_field.value)
    impedance_field.value = inductivity * 2 * Math.PI * center
}
impedance_field.addEventListener('input', function(_) {
    recalculate_inductivity()
    recalculate_schematic()
})
inductivity_field.addEventListener('input', function(_) {
    recalculate_impedance()
    recalculate_schematic()
})
function recalculate_coils() {
    if (impedance_radio.checked) {
        recalculate_inductivity()
    } else {
        recalculate_impedance()
    }
}
center_field.addEventListener('input', function(_) {
    recalculate_coils()
})
;[order_field, input_impedance_field, output_impedance_field, capacity_q_field, inductivity_q_field].forEach(function(el) {
    el.addEventListener('input', function(_) {
        recalculate_schematic()
    })
})

function coth(x) {
    return Math.cosh(x) / Math.sinh(x)
}

function make_shape(name, attrs, inner) {
    const shape = document.createElementNS('http://www.w3.org/2000/svg', name)
    for (const attr in attrs) {
        shape.setAttributeNS(null, attr, attrs[attr])
    }
    if (inner !== undefined) {
        shape.innerHTML = inner
    }
    return shape
}
function make_yem(start_y) {
    return function(y) { return start_y + y + 'em' }
}
function si_prefix(value) {
    if (value < 1e-18) {
        return 0
    }
    const exponent = Math.floor(Math.log10(value)/3)
    value *= Math.pow(10, -3 * exponent)
    value = value.toFixed(3 - Math.floor(Math.log10(value)))
    if (exponent < 0) {
        return value + ['', 'm', '&micro;', 'n', 'p', 'f', 'a'][-exponent]
    } else {
        return value + ['', 'k', 'M', 'G', 'T', 'P', 'E'][exponent]
    }
}
function draw_horizontal_capacitor(capacity, start_y) {
    const yem = make_yem(start_y)
    schematic_field.appendChild(make_shape('line', {x1: '1em', y1: yem(0), x2: '1em', y2: yem(4), stroke: 'black'}))
    schematic_field.appendChild(make_shape('circle', {cx: '1em', cy: yem(2), r: '0.25em', fill: 'black'}))
    schematic_field.appendChild(make_shape('line', {x1: '1em', y1: yem(2), x2: '4em', y2: yem(2), stroke: 'black'}))
    schematic_field.appendChild(make_shape('line', {x1: '4em', y1: yem(1), x2: '4em', y2: yem(3), stroke: 'black'}))
    schematic_field.appendChild(make_shape('line', {x1: '4.5em', y1: yem(1), x2: '4.5em', y2: yem(3), stroke: 'black'}))
    schematic_field.appendChild(make_shape('text', {x: '5em', y: yem(1.5)}, si_prefix(capacity) + 'F'))
    schematic_field.appendChild(make_shape('line', {x1: '4.5em', y1: yem(2), x2: '7.5em', y2: yem(2), stroke: 'black'}))
    schematic_field.appendChild(make_shape('line', {x1: '7.5em', y1: yem(2), x2: '7.5em', y2: yem(4), stroke: 'black'}))
    schematic_field.appendChild(make_shape('line', {x1: '6.5em', y1: yem(4), x2: '8.5em', y2: yem(4), stroke: 'black', 'stroke-width': '0.25em'}))
    return 4
}
function draw_inductor(inductivity, start_y) {
    const emsize = window.getComputedStyle(schematic_field).getPropertyValue('font-size').replace('px', '')
    const em = function(value) { return value * emsize }
    const yem_ = function(y) { return em(start_y + y) }
    const yem = make_yem(start_y)
    const windingpath = Array.apply(null, Array(4)).map(function(_) { return ['a', em(0.5), em(0.5), '0 0 1 0', em(1)].join(" ") }).join(' ')
    schematic_field.appendChild(make_shape('path', {d: ['M', em(1), yem_(0), 'v', em(1), windingpath, 'v', em(1)].join(' '), stroke: 'black', fill: 'none'}))
    schematic_field.appendChild(make_shape('text', {x: '2em', y: yem(3.5)}, si_prefix(inductivity) + 'H'))
    return 6
}
function draw_vertical_capacitor(capacity, start_y) {
    const yem = make_yem(start_y)
    schematic_field.appendChild(make_shape('line', {x1: '1em', y1: yem(0), x2: '1em', y2: yem(1), stroke: 'black'}))
    schematic_field.appendChild(make_shape('line', {x1: '0em', y1: yem(1), x2: '2em', y2: yem(1), stroke: 'black'}))
    schematic_field.appendChild(make_shape('line', {x1: '0em', y1: yem(1.5), x2: '2em', y2: yem(1.5), stroke: 'black'}))
    schematic_field.appendChild(make_shape('line', {x1: '1em', y1: yem(1.5), x2: '1em', y2: yem(2.5), stroke: 'black'}))
    schematic_field.appendChild(make_shape('text', {x: '2.5em', y: yem(1.5)}, si_prefix(capacity) + 'F'))
    return 2.5
}

// Basic complex arithmetic
function com(re, im) {
    return {re: re, im: im}
}
function cadd(x1, x2) {
    return com(x1.re + x2.re, x1.im + x2.im)
}
function cneg(x) {
    return com(-x.re, -x.im)
}
function csub(x1, x2) {
    return cadd(x1, cneg(x2))
}
function cmul(x1, x2) {
    return com(x1.re * x2.re - x1.im * x2.im, x1.im * x2.re + x1.re * x2.im)
}
function cinv(x) {
    const denom = x.re * x.re + x.im * x.im
    return com(x.re / denom, -x.im / denom)
}
function cdiv(x1, x2) {
    return cmul(x1, cinv(x2))
}
function cabs(x) {
    return Math.sqrt(x.re * x.re + x.im * x.im)
}
function abcdmult(x1, x2) {
    const mam = function(y1, y2, y3, y4) {
        return cadd(cmul(y1, y2), cmul(y3, y4))
    }
    return {A: mam(x1.A, x2.A, x1.B, x2.C),
        B: mam(x1.A, x2.B, x1.B, x2.D),
        C: mam(x1.C, x2.A, x1.D, x2.C),
        D: mam(x1.C, x2.B, x1.D, x2.D)}
}

function recalculate_schematic() {
    const center = Number(center_field.value)
    const fbw = Number(fbw_field.value)
    const w0 = 2 * Math.PI * center * 1e6

    const ripple = Number(ripple_field.value)

    const inductivity = Number(inductivity_field.value)
    const Ls = inductivity * 1e-6

    const order = Number(order_field.value)
    const Z0 = Number(input_impedance_field.value)
    const Znp1 = Number(output_impedance_field.value)

    const QC = Number(capacity_q_field.value)
    const QL = Number(inductivity_q_field.value)

    // Chebyshev prototype LPF
    const c17_37 = 40 / Math.log(10) // roughly 17.37
    const beta = Math.log(coth(ripple / c17_37))
    const gamma = Math.sinh(beta / (2*order))
    const g = [1]
    g[1] = 2 / gamma * Math.sin(Math.PI / (2*order))
    for (let i = 2; i <= order; i++) {
        g[i] = 1 / g[i-1] * 4 * Math.sin((2*i - 1) * Math.PI / (2*order))
            * Math.sin((2*i - 3) * Math.PI / (2*order))
            / (Math.pow(gamma, 2) + Math.pow(Math.sin((i-1) * Math.PI / order), 2))
    }
    g[order + 1] = (order % 2 !== 0) ? 1 : Math.pow(coth(beta/4), 2)
    for (let g_ of g) {
        if (isNaN(g_)) {
            schematic_field.innerHTML = ''
            calculation_error.style.display = 'block'
            return
        }
    }
    
    // First round of constants
    Omc = 1 // Prototype filter center frequency
    const Cs = 1 / (Math.pow(w0, 2) * Ls)
    const K = [NaN] // K[0] is never used
    K[1] = Math.sqrt(Z0 * fbw * w0 * Ls / (Omc * g[0] * g[1]))
    for (let i = 1; i < order; i++) {
        K[i+1] = fbw * w0 * Ls / Omc * Math.sqrt(1 / (g[i] * g[i+1]))
    }
    K[order+1] = Math.sqrt(Znp1 * fbw * w0 * Ls / (Omc * g[order] * g[order+1]))
    const G0_ = Z0 * Math.pow(2 * w0 * Cs * K[1], 2)
        / (Math.pow(Z0, 2) + Math.pow(2 * w0 * Cs * Math.pow(K[1], 2), 2))
    const B0_ = Math.pow(Znp1, 2) * 2 * w0 * Cs
        / (Math.pow(Z0, 2) + Math.pow(2 * w0 * Cs * Math.pow(K[1], 2), 2))
    
    // First capacitors
    const C = [NaN] // C[0] is never used
    const Cx0 = Math.sqrt(-G0_ / (Z0 * Math.pow(w0, 2) * (Z0 * G0_ - 1)))
    const Cxnp1 = Cx0 // First and last capacitors get eliminated (0 capacitance) by clever math
    C[1] = Math.sqrt(G0_ * (1 + Math.pow(Z0 * w0 * Cx0, 2)) / (Z0 * Math.pow(w0, 2)))
    const Cp1 = 1/w0 * (B0_
        - w0 * C[1] /* eliminated (Cp0==0) */
        / (1 + Math.pow(Z0 * w0 * Cx0, 2)))
    
    // Second round of constants
    const D = [NaN]
    const N = [NaN]
    const Np = [NaN]
    for (let i = 1; i <= order + 1; i++) {
        const k = K[i]
        D[i] = 4*Cs / (1 - 2*Cs * w0 * k) + 1 / (w0 * k)
        N[i] = Math.pow(2*Cs / (1 - 2*Cs * w0 * k), 2)
        Np[i] = 1 / (w0 * k) * 2*Cs / (1 - 2*Cs * w0 * k)
    }
    
    // Intermediate capacitors
    const Cp = [NaN]
    for (let i = 2; i <= order; i++) {
        C[i] = N[i] / D[i]
        Cp[i-1] = Np[i] / D[i]
    }

    // Last round of constants
    Gnp1_ = Znp1 * Math.pow(2 * w0 * Cs * K[order+1], 2)
        / (Math.pow(Znp1, 2) + Math.pow(2 * w0 * Cs * Math.pow(K[order+1], 2), 2))
    Bnp1_ = Math.pow(Znp1, 2) * 2 * w0 * Cs
        / (Math.pow(Znp1, 2) + Math.pow(2 * w0 * Cs * Math.pow(K[order+1], 2), 2))

    // Final capacitors
    C[order+1] = Math.sqrt(Gnp1_ * (1 + Math.pow(Znp1 * w0 * Cxnp1, 2)) / (Znp1 * Math.pow(w0, 2)))
    Cpn = 1 / w0 * (Bnp1_
        - w0 * C[order+1] /* eliminated (Cpnp1==0) */
        / (1 + Math.pow(Znp1 * w0 * Cxnp1, 2)))
    
    // Drawing stuff
    schematic_field.innerHTML = ''
    for (let c of [Cp1, Cpn].concat(C.slice(1), Cp.slice(1))) {
        if (c < 0 || isNaN(c)) {
            calculation_error.style.display = 'block'
            return
        }
    }
    calculation_error.style.display = 'none'
    schematic_field.appendChild(make_shape('circle', {cx: '1em', cy: '1em', r: '0.25em', stroke: 'black', fill: 'none'}))
    schematic_field.appendChild(make_shape('text', {x: '1.75em', y: '1.375em'}, si_prefix(Z0) + '&#x2126;'))
    schematic_field.appendChild(make_shape('line', {x1: '1em', y1: '1.25em', x2: '1em', y2: '2.25em', stroke: 'black'}))
    let start_y = 2.25
    start_y += draw_vertical_capacitor(C[1], start_y)
    start_y += draw_horizontal_capacitor(Cp1, start_y)
    start_y += draw_inductor(Ls, start_y)
    for (let i = 1; i < order; i++) {
        start_y += draw_horizontal_capacitor(Cp[i], start_y)
        start_y += draw_vertical_capacitor(C[i+1], start_y)
        start_y += draw_horizontal_capacitor(Cp[i], start_y)
        start_y += draw_inductor(Ls, start_y)
    }
    start_y += draw_horizontal_capacitor(Cpn, start_y)
    start_y += draw_vertical_capacitor(C[order+1], start_y)
    const yem = make_yem(start_y)
    schematic_field.appendChild(make_shape('line', {x1: '1em', y1: yem(0), x2: '1em', y2: yem(1), stroke: 'black'}))
    schematic_field.appendChild(make_shape('text', {x: '1.75em', y: yem(1.375)}, si_prefix(Znp1) + '&#x2126;'))
    schematic_field.appendChild(make_shape('circle', {cx: '1em', cy: yem(1.25), r: '0.25em', stroke: 'black', fill: 'none'}))
    start_y += 2.25
    schematic_field.style.height = start_y + 'em'

    const Zc = function(c, f) {
        const X = 1 / (2 * Math.PI * f * c)
        return com(X / QC, -X)
    }
    const Yc = function(c, f) {
        return cinv(Zc(c, f))
    }
    const Yl = function(l, f) {
        const X = 2 * Math.PI * f * l
        return cinv(com(X / QL, X))
    }
    const abcdser = function(c) {
        return function(f) {
            return {A: com(1, 0), B: Zc(c, f), C: com(0, 0), D: com(1, 0)}
        }
    }
    const abcdpi = function(c1, l, c2) {
        return function(f) {
            const Y1 = Yc(c1, f)
            const Y3 = Yl(l, f)
            const Y2 = Yc(c2, f)
            const A = cadd(com(1, 0), cdiv(Y2, Y3))
            const B = cinv(Y3)
            const C = cadd(cadd(Y1, Y2), cdiv(cmul(Y1, Y2), Y3))
            const D = cadd(com(1, 0), cdiv(Y1, Y3))
            return {A: A, B: B, C: C, D: D}
        }
    }

    let ABCDs = []
    ABCDs.push(abcdser(C[1]))
    ABCDs.push(abcdpi(Cp1, Ls, Cp[1]))
    for (let i = 1; i <= order - 2; i++) {
        ABCDs.push(abcdser(C[i+1]))
        ABCDs.push(abcdpi(Cp[i], Ls, Cp[i+1]))
    }
    ABCDs.push(abcdser(C[order]))
    ABCDs.push(abcdpi(Cp[order-1], Ls, Cpn))
    ABCDs.push(abcdser(C[order+1]))
    const ABCD = function(f) {
        const mapped = ABCDs.map(function(abcd) { return abcd(f) })
        return ABCDs.map(function(abcd) { return abcd(f) }).reduce(abcdmult)
    }
    const S21 = function(f) {
        const v = ABCD(f)
        return cdiv(com(2, 0), cadd(cadd(cadd(v.A, cdiv(v.B, com(Znp1, 0))), cmul(v.C, com(Z0, 0))), v.D))
    }
    const dB = function(x) {
        return 10 * Math.log10(x)
    }
    const transfer = function(f) {
        const a = cabs(S21(f))
        return a * a
    }
    functionPlot({
        target: '#curveplot',
        xAxis: {
            domain: [1, center * 3e6]
        },
        yAxis: {
            domain: [-100, 0]
        },
        data: [{
            graphType: 'polyline',
            fn: function(scope) { return dB(transfer(scope.x)) }
        }]
    })
}
recalculate_schematic()
})()
</script>