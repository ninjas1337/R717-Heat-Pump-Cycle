# R717 Heat Pump Cycle

An interactive, single-file visualisation of a single-stage vapour-compression heat pump running on ammonia (R717). Set the heating supply temperature and the heat source temperature, and the page recalculates all four stages of the cycle, draws them on a log p–h chart and places each state on ammonia's p–T phase diagram.

**Live demo:** [ninjas1337.github.io/R717-Heat-Pump-Cycle](https://ninjas1337.github.io/R717-Heat-Pump-Cycle/)

Everything runs in the browser from one file, [`index.html`](index.html). It has no build step and no dependencies. The ammonia property model is built in, so the calculations also work offline. Only the web fonts are loaded from Google Fonts.

## Features

- **Inputs:** heating supply temperature (presets for floor heating, radiators, district heating and process heat), heat source temperature, heat exchanger approach ΔT, suction superheat, liquid subcooling, compressor isentropic efficiency and heating capacity.
- **Key figures:** heating COP, Carnot COP, second-law efficiency, pressure ratio, discharge temperature and compressor power.
- **Circuit schematic:** an animated diagram of the evaporator, compressor, condenser and expansion valve, with state labels. The condenser shows its desuperheating, condensing and subcooling zones in proportion.
- **Stage explanations:** click a component or tab to get the physics of compression, condensation, expansion and evaporation, filled in with the current numbers.
- **log p–h diagram:** saturation dome, isotherms, isentropes and quality lines, with the cycle drawn on top and a hover readout of p, h and phase.
- **p–T phase diagram:** sublimation, vaporisation and (schematic) melting curves, plus the triple point, the critical point and the cycle states.
- **State table:** T, p, h, s, ρ and phase for every state point, including the ideal isentropic discharge 2s.
- **Light and dark themes** that follow the system setting. Your inputs are remembered in the browser between visits.

## Usage

Use the [live demo](https://ninjas1337.github.io/R717-Heat-Pump-Cycle/), or open `index.html` in any modern browser. You can also serve the folder with any static web server.

## Example: default settings

Supply 55 °C, source 8 °C, ΔT = 5 K, superheat 5 K, subcooling 3 K, η<sub>is</sub> = 0.75, heating capacity 1 MW:

| Quantity | Value |
|---|---|
| Evaporating / condensing | 3 °C at 4.80 bar / 60 °C at 26.15 bar |
| Pressure ratio | 5.45 |
| Discharge temperature | 172 °C |
| Vapour quality after valve, x₄ | 0.210 |
| COP, heating | 3.86 |
| Carnot COP | 5.85 |
| Second-law efficiency | 66 % |
| Compressor power | 259 kW |

## Thermodynamic model

The property data comes from the NIST Chemistry WebBook, which evaluates the Gao et al. (2023) reference equation of state for ammonia [1, 2]. Three compact models are fitted to more than 300 NIST points (0.06 to 105 bar, −78 to 450 °C):

1. **Vapour pressure:** a Wagner-type equation [3] with T<sub>c</sub> = 405.56 K and p<sub>c</sub> = 113.634 bar. Largest deviation from NIST: 0.05 %.
2. **Saturated liquid and vapour h, s and ρ:** ancillary power series in (1 − T/T<sub>c</sub>). Largest deviation: 0.8 kJ/kg in h, 0.2 % in liquid density.
3. **Superheated vapour:** an ideal-gas part with Planck–Einstein terms [4] plus a residual Gibbs-energy series in p/p<sub>c</sub> and T<sub>c</sub>/T, fitted to NIST h, s and ρ up to 80 bar. RMS deviation 0.15 kJ/kg, largest 1.6 kJ/kg. With one whole isobar left out of the fit (10, 20, 40 or 60 bar), the largest error on that isobar stays below 1.3 kJ/kg.

Enthalpy and entropy use the IIR reference state: h = 200 kJ/kg and s = 1.000 kJ/kg·K for saturated liquid at 0 °C.

### Cycle assumptions

The model follows the standard vapour-compression analysis [5, 6]:

- no pressure drop in pipes or heat exchangers
- condensing temperature = supply + ΔT, evaporating temperature = source − ΔT
- adiabatic compressor with isentropic efficiency η<sub>is</sub> = (h<sub>2s</sub> − h<sub>1</sub>) / (h<sub>2</sub> − h<sub>1</sub>)
- isenthalpic throttling, h<sub>4</sub> = h<sub>3</sub>
- subcooled liquid taken as h ≈ h<sub>f</sub>(T)

Performance figures:

- COP<sub>H</sub> = (h<sub>2</sub> − h<sub>3</sub>) / (h<sub>2</sub> − h<sub>1</sub>)
- COP<sub>Carnot</sub> = T<sub>cond</sub> / (T<sub>cond</sub> − T<sub>evap</sub>), using the condensing and evaporating temperatures
- energy balance q<sub>H</sub> = q<sub>L</sub> + w

### Phase diagram

The vaporisation curve is the Wagner equation. The sublimation curve is the Clausius–Clapeyron equation through the triple point (195.49 K, 0.0605 bar), using Δh<sub>sub</sub> = 31.2 kJ/mol as measured by Overstreet and Giauque [7]. The melting curve is drawn schematically: it rises very steeply from the triple point, and no fitted melting equation is used.

### Limitations

- The superheated vapour model is fitted up to 80 bar and 450 °C. If the discharge temperature goes above 450 °C, the page shows a warning and the results are only indicative. A real plant would split the lift over two compression stages with intercooling [6].
- The model covers a single-stage cycle with no pressure drop or heat loss. It is not a design tool for a real plant.

## References

1. Gao, K., Wu, J., Bell, I. H., Harvey, A. H., Lemmon, E. W. (2023). A reference equation of state with an associating term for the thermodynamic properties of ammonia. *Journal of Physical and Chemical Reference Data*, 52(1). [doi:10.1063/5.0128269](https://doi.org/10.1063/5.0128269)
2. Lemmon, E. W., Bell, I. H., Huber, M. L., McLinden, M. O. Thermophysical Properties of Fluid Systems. In: Linstrom, P. J., Mallard, W. G. (eds.), *NIST Chemistry WebBook, NIST Standard Reference Database 69*. National Institute of Standards and Technology. Data retrieved September 2026. [webbook.nist.gov/chemistry/fluid](https://webbook.nist.gov/chemistry/fluid/)
3. Wagner, W. (1973). New vapour pressure measurements for argon and nitrogen and a new method for establishing rational vapour pressure equations. *Cryogenics*, 13(8), 470–482. [doi:10.1016/0011-2275(73)90003-9](https://doi.org/10.1016/0011-2275(73)90003-9)
4. Span, R. (2000). *Multiparameter Equations of State: An Accurate Source of Thermodynamic Property Data*. Springer, Berlin. [doi:10.1007/978-3-662-04092-8](https://doi.org/10.1007/978-3-662-04092-8)
5. Çengel, Y. A., Boles, M. A., Kanoğlu, M. (2019). *Thermodynamics: An Engineering Approach*, 9th ed. McGraw-Hill, ch. 11 (refrigeration cycles and heat pumps).
6. Stoecker, W. F. (1998). *Industrial Refrigeration Handbook*. McGraw-Hill, New York.
7. Overstreet, R., Giauque, W. F. (1937). Ammonia. The heat capacity and vapor pressure of solid and liquid. Heat of vaporization. The entropy values from thermal and spectroscopic data. *Journal of the American Chemical Society*, 59(2), 254–259. [doi:10.1021/ja01281a008](https://doi.org/10.1021/ja01281a008)

## Author

Sanjin Redzic. ORCID: [0009-0006-7034-6869](https://orcid.org/0009-0006-7034-6869)

## License

Released under the [MIT License](LICENSE).
