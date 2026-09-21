# Solar-Powered Coastal Utility Hub berbasis Kopdes Merah Putih

## Executive Summary

Laporan ini menyajikan desain awal dan model techno-economic untuk **Solar-Powered Coastal Utility Hub** yang dioperasikan oleh Kopdes Merah Putih. Hub menyediakan dua layanan: air bersih untuk rumah tangga dan food-grade ice slurry untuk nelayan.

Model ini adalah **pre-feasibility**, bukan hasil validasi lokasi. Belum ada data desa, profil beban, kualitas air baku, harga ikan, jumlah nelayan, quotation vendor, atau data iradiasi lokasi spesifik. Oleh karena itu, seluruh angka yang tidak memiliki sumber lapangan diberi label **ENGINEERING ASSUMPTION**, **MARKET ASSUMPTION**, atau **SIMULATION**.

### Hasil base case

| Metric | Nilai | Status |
|---|---:|---|
| PV capacity | 50 kWp | Engineering assumption |
| PV production | 175 kWh/day; 63,875 kWh/year | Simulation |
| Clean-water nameplate | 20 m³/day | Engineering assumption |
| Ice-slurry nameplate | 100 kg/day | Engineering assumption |
| Base utilisation | 70% | Simulation assumption |
| Water sold | 14 m³/day | Simulation |
| Ice slurry sold | 70 kg/day | Simulation |
| Households served | sekitar 140 rumah tangga | Derived; 100 L/household/day |
| Fishing trips served | 3.5 trips/day; 105 trips/month | Derived; 20 kg/trip |
| Annual revenue | Rp191.6 juta | Market assumption + simulation |
| Annual OPEX | Rp95.0 juta | Engineering/market assumption |
| Operating profit before tax | Rp96.6 juta/tahun | Derived |
| Initial CAPEX | Rp1.25 miliar | Market estimate assumption |
| Simple payback | sekitar 12.9 tahun | Derived |
| Simple ROI | sekitar 7.7%/tahun | Derived |

**Kesimpulan awal:** sistem teknis dapat dibuat konsisten dengan PV 50 kWp, tetapi model komersial base case masih memiliki payback panjang. Kelayakan menjadi lebih kuat jika hub memperoleh CAPEX grant/blended finance, meningkatkan utilisasi, menaikkan nilai layanan cold-chain, atau dibangun pada skala yang lebih besar. Angka 30 kg/day dari konsep SEAGARA lebih tepat untuk demonstrator/pilot, bukan kapasitas bisnis hub.

---

## 1. Problem & Opportunity

Masyarakat pesisir dapat menghadapi keterbatasan air tawar, biaya energi dan transportasi tinggi, serta kehilangan mutu ikan akibat pendinginan yang terlambat atau tidak merata. Masalah tersebut saling terkait: air bersih dan cold-chain sama-sama membutuhkan energi, pompa, storage, operasi rutin, dan tata kelola lokal.

Kopdes dapat menjadi operator karena memiliki kedekatan dengan pengguna, mekanisme penagihan lokal, dan peluang menggabungkan layanan sosial dengan unit usaha produktif. Nilai bisnis tidak hanya berasal dari penjualan komoditas, tetapi juga dari pengurangan kehilangan mutu ikan dan meningkatnya keandalan layanan.

---

## 2. Proposed Solution

Arsitektur yang diusulkan:

```text
Solar PV -> Energy Management System ->
  Water treatment/desalination -> clean-water storage -> households
  Ice-slurry generator -> insulated storage -> fish boxes -> fishermen

Sensors -> data platform -> forecast -> optimisation -> schedule -> operator recommendation
```

AI diposisikan sebagai **decision-support system**. Pada tahap awal, model dapat menggunakan data simulasi dan historical synthetic data. Sistem tidak boleh diklaim telah memiliki AI terlatih sebelum data sensor dan hasil validasi tersedia.

---

## 3. Technical Design

### 3.1 PV system

| Parameter | Nilai awal | Label |
|---|---:|---|
| Roof area assumed | 500 m² | Engineering assumption |
| Effective PV area | 350 m² | Engineering assumption |
| Module size | 550 Wp | Market/engineering assumption |
| Module count | 91 modules | Derived, 50.05 kWp DC |
| Installed rating | 50 kWp DC | Design target |
| Peak sun hours | 4.5 h/day | Location validation required |
| Performance ratio | 0.78 | Engineering assumption |
| Inverter efficiency | 0.97 | Engineering assumption |
| MPPT/combiner allowance | included in PR | Engineering assumption |
| Temperature/shading/wiring losses | included in PR | Engineering assumption |

Energi PV harian:

`50 kWp x 4.5 h/day x 0.78 = 175.5 kWh/day`

Untuk model digunakan **175 kWh/day**, **5,250 kWh/month** dengan 30 hari, dan **63,875 kWh/year** dengan 365 hari. Produksi aktual perlu dihitung ulang menggunakan data iradiasi bulanan di lokasi. Sumber data yang perlu dipakai pada tahap survey: BMKG, Global Solar Atlas/World Bank, atau pengukuran pyranometer lokal.

Atap 500 m² dan utilisasi efektif 350 m² harus diverifikasi secara struktural, termasuk orientasi, kemiringan, bayangan, akses maintenance, dan ruang fire safety.

### 3.2 Battery and EMS

Desain hub menggunakan battery sebagai buffer dan sumber beban kritis, bukan sebagai sumber energi utama.

| Parameter | Nilai awal |
|---|---:|
| Battery bank | 48 V, 200 Ah LiFePO4 |
| Nominal energy | 9.6 kWh |
| DoD | 80% |
| Round-trip/usable efficiency assumption | 95% |
| Usable energy | `9.6 x 0.80 x 0.95 = 7.3 kWh` |
| Primary use | night critical loads, controls, refrigeration stability |
| Autonomy | approximately 4-7 hours for critical loads only |

Battery 24 V, 100 Ah dari konsep SEAGARA hanya memiliki energi nominal 2.4 kWh. Dengan DoD 80% dan efisiensi 95%, energi usable sekitar 1.8 kWh. Kapasitas tersebut tidak cukup untuk mengoperasikan hub 50 kWp sepanjang malam; cocok sebagai demonstrator kecil atau buffer, bukan desain komersial hub.

### 3.3 Water treatment

Konfigurasi awal yang disarankan adalah pretreatment, cartridge filter, RO air payau/laut sesuai hasil uji, remineralisation/post-treatment, disinfection, dan storage.

| Parameter | Nilai |
|---|---:|
| Product-water nameplate | 20 m³/day |
| Specific energy assumption | 4.5 kWh/m³ |
| Daily water-treatment energy | 90 kWh/day |
| Recovery ratio | 40% for seawater-oriented design; validate locally |
| Feedwater for 20 m³/day product | approximately 50 m³/day at 40% recovery |
| Storage target | 1 day of product water = 20 m³ |
| Household planning demand | 100 L/household/day |
| Full-capacity household equivalent | 200 households |
| Base-case households served | 140 households |

Pada 20 m³/day, output tahunan nameplate adalah 7,300 m³. Pada utilisasi 70%, air terjual adalah 5,110 m³/tahun. Air baku, brine discharge, salinity, chemical cleaning, membrane life, dan izin lingkungan wajib divalidasi melalui survey dan pilot.

### 3.4 Ice-slurry system

| Parameter | Nilai awal | Catatan |
|---|---:|---|
| Ice-slurry nameplate | 100 kg/day | Hub design assumption |
| Base output/sales | 70 kg/day | 70% utilisation |
| Target slurry temperature | near 0°C | Product and safety validation required |
| Fish load planning | 50 kg/trip | Market assumption |
| Ice slurry per trip | 20 kg/trip | Thermal/operational assumption |
| Trips served | 3.5/day at base | Derived |
| Energy intensity | 0.12 kWh/kg slurry | Engineering assumption; vendor test required |
| Refrigeration and auxiliaries | approximately 12 kWh/day at 100 kg/day | Derived/assumption |

Angka compressor sekitar 0.78 kW dan COP 2.5 dari konsep SEAGARA tidak otomatis menghasilkan 100 kg/day. Untuk evaluasi kasar, beban refrigeration ideal dapat diperkirakan dari enthalpy removal, sedangkan konsumsi listrik aktual harus memasukkan compressor cycling, condenser, heat exchanger, agitator, pump, ambient 32°C, seawater 28°C, insulation, dan losses. Karena itu, **0.12 kWh/kg adalah asumsi desain awal yang harus diuji pada prototype**.

Kapasitas SEAGARA 30 kg/day hanya melayani `30/20 = 1.5 trip/day`, atau sekitar 45 trip/month, sebelum allowance untuk peak demand dan storage. Pilihan desain:

| Opsi | Kapasitas | Kegunaan | Trade-off |
|---|---:|---|---|
| Pilot | 30 kg/day | 1-2 trip/day | CAPEX rendah, demand coverage terbatas |
| Hub awal | 100 kg/day | sekitar 3-5 trip/day | lebih relevan untuk unit bisnis, CAPEX lebih tinggi |
| Scalable | 2 x 100 kg/day modules | peak season dan redundancy | investasi modular dan kontrol lebih kompleks |

---

## 4. AI & Resource Management

### Data input

- Energi: irradiance, weather forecast, PV voltage/current/power, battery SOC, historical generation, load.
- Air: demand harian, rumah tangga aktif, tank level, RO production, feedwater quality, filter pressure.
- Nelayan: departure schedule, kapal aktif, trip, catch estimate, ice requirement, storage booking.
- Lingkungan: ambient temperature, seawater temperature, rainfall, weather warning.

### Pipeline

1. **Forecast:** PV generation, water demand, ice demand, departure schedule.
2. **Optimisation:** match forecast generation, storage, and priority loads.
3. **Scheduling:** choose treatment and refrigeration windows.
4. **Resource allocation:** allocate energy to critical water, cold-chain, battery, and non-critical loads.
5. **Anomaly detection:** flag abnormal compressor power, pump pressure, membrane differential pressure, and battery behaviour.
6. **Human approval:** operator confirms recommendations and can override them.

Contoh rekomendasi simulasi: *"Forecast solar generation tomorrow is high; run water treatment and increase ice production between 10:00-14:00, then charge the battery after minimum cold-chain demand is satisfied."* Ini adalah **SIMULATION**, bukan output model terlatih.

Model minimum viable AI: baseline moving average/gradient boosting untuk forecast, rule-based EMS untuk prioritas, dan anomaly threshold sebelum machine learning yang lebih kompleks. Evaluasi dengan MAE/MAPE, forecast bias, service-level, kWh curtailed, dan shortage events.

---

## 5. Capacity and Energy Balance

### 5.1 Daily energy balance at base design

| Load | Calculation | kWh/day |
|---|---|---:|
| Water treatment | 20 m³ x 4.5 kWh/m³ | 90.0 |
| Ice slurry | 100 kg x 0.12 kWh/kg | 12.0 |
| Raw/product-water pumps | assumption | 8.0 |
| Agitator, heat rejection, refrigeration auxiliaries | included allowance | 4.0 |
| Control, IoT, lighting, monitoring | assumption | 3.0 |
| Maintenance/standby allowance | assumption | 3.0 |
| **Operating load** |  | **120.0** |
| PV production | 50 x 4.5 x 0.78 | **175.0** |
| Available surplus before battery/curtailment | 175 - 120 | **55.0** |

The 55 kWh/day surplus is not guaranteed every day. It is an average-design-day surplus and must be stress-tested using monthly irradiation, rainy days, demand peaks, and degraded equipment. Production scheduling should use the surplus for extra water/ice, battery charging, or controlled curtailment. In a low-solar day, non-critical production is deferred.

### 5.2 Annual allocation at nameplate-equivalent operation

| Category | kWh/year | Share of 63,875 kWh PV |
|---|---:|---:|
| Water treatment | 32,850 | 51.4% |
| Ice slurry | 4,380 | 6.9% |
| Pumps | 2,920 | 4.6% |
| Auxiliary/agitator/refrigeration | 1,460 | 2.3% |
| Control/monitoring | 1,095 | 1.7% |
| Standby allowance | 1,095 | 1.7% |
| Battery charging, conversion loss, curtailment reserve | 20,075 | 31.4% |

The last row is a balancing/reserve category, not productive consumption. It must be replaced by measured battery throughput and actual curtailment after commissioning. The arithmetic should be re-baselined once actual operating hours are known; the productive load is 43,800 kWh/year and the remainder is available reserve/curtailment.

**Consistency check:** average PV production (175 kWh/day) is above the modeled average operating load (120 kWh/day), but this does not prove 24/7 reliability. A monthly energy model and rainy-season test are required.

---

## 6. Ice vs Ice Slurry

| Parameter | Conventional crushed ice | Ice slurry |
|---|---|---|
| Contact area | point/limited contact | high contact with fish surface |
| Heat transfer | slower and less uniform | faster and more uniform |
| Temperature control | often variable | controllable near target temperature |
| Handling | bags/blocks, manual distribution | pumpable or pourable, requires equipment |
| Water/fish interface | may create meltwater pockets | more uniform cooling, food-grade design required |
| Labour | breaking, moving, layering | generator, storage, dispensing |
| Fish quality risk | higher if cooling is delayed | potentially lower, must validate in field |
| Price basis | commonly lower per kg | may be higher per kg |
| Economic value | low upfront cost | value from reduced quality loss and better consistency |

Ice slurry should not be sold on an unsupported claim that it increases fish price. The defensible claim is that it **may reduce quality loss and improve temperature control**, subject to a pilot measuring core temperature, histamine/safety indicators where relevant, rejection rate, and realised selling price.

---

## 7. Fisherman Economic Impact

### Scenario assumptions

- 50 kg fish/trip.
- Fish value scenario: Rp22,000/kg.
- Without cold-chain quality loss: 12%.
- With ice slurry quality loss: 5%.
- Ice slurry need: 20 kg/trip.
- Ice slurry price: Rp4,500/kg.
- Conventional ice reference: 25 kg/trip x Rp2,500/kg.
- Additional handling/operational cost with slurry: Rp10,000/trip.
- These are **MARKET ASSUMPTIONS**, not local facts.

| Item | Without cold chain | With ice slurry |
|---|---:|---:|
| Potential fish value | Rp1,100,000 | Rp1,100,000 |
| Quality loss | Rp132,000 | Rp55,000 |
| Cooling cost | Rp62,500 conventional ice | Rp90,000 slurry |
| Additional operating cost | Rp0 | Rp10,000 |
| Net income/trip | **Rp905,500** | **Rp945,000** |
| Improvement |  | **Rp39,500/trip** |

At 12 trips/month, the illustrative improvement is **Rp474,000/fisher/month**. This is a scenario, not a promise. If quality loss reduction is only 3 percentage points, or if fish value is lower, the benefit can disappear. The pilot must measure actual price, reject rate, and costs.

---

## 8. CAPEX Estimate

Indicative 2026 planning estimate, to be replaced by quotations. Values below are **MARKET ESTIMATES / ASSUMPTIONS**, not vendor quotations.

| Package | Estimated cost |
|---|---:|
| 50 kWp PV modules and mounting | Rp350,000,000 |
| Inverter, MPPT, DC/AC protection, wiring, monitoring | Rp150,000,000 |
| 48 V 200 Ah LiFePO4, BMS, enclosure, protection | Rp90,000,000 |
| Pretreatment, RO/desalination, pumps, membrane | Rp220,000,000 |
| Water tanks, post-treatment, piping | Rp90,000,000 |
| 100 kg/day ice-slurry generator and refrigeration package | Rp180,000,000 |
| Ice storage, insulation, dispenser, fish boxes | Rp55,000,000 |
| Sensors, EMS, IoT, dashboard, controls | Rp45,000,000 |
| Civil work, installation, commissioning, training | Rp70,000,000 |
| **Estimated initial CAPEX** | **Rp1,250,000,000** |

Recommended procurement: obtain at least three quotations, require energy and production acceptance tests, and separate pilot CAPEX from commercial expansion CAPEX. A 15% contingency would increase the total to approximately Rp1.44 billion; the financial model below uses Rp1.25 billion before contingency to keep the base calculation transparent.

---

## 9. OPEX and Unit Economics

### Annual OPEX assumption

| Cost item | Rp/year |
|---|---:|
| Operator allocation and administration | 30,000,000 |
| Routine maintenance PV, RO, pumps, refrigeration | 18,000,000 |
| Filters, membranes, treatment consumables | 15,000,000 |
| Software, connectivity, monitoring | 6,000,000 |
| Cleaning, water quality testing, sanitation | 8,000,000 |
| Battery/refrigeration replacement reserve | 12,000,000 |
| Insurance and miscellaneous | 6,000,000 |
| **Total OPEX** | **95,000,000** |

Operator cost is modeled as part-time/shared Kopdes staff. If a full-time technician is required, OPEX must be increased.

### HPP water

Using annual nameplate production of 7,300 m³:

- Cash OPEX allocation to water at 70% of OPEX: Rp66.5 juta/year.
- Annualised CAPEX allocation at 10-year straight-line life: Rp125 juta/year.
- Illustrative HPP water: `(66.5 + 125) juta / 7,300 m³ = approximately Rp26,200/m³`.
- At base sales volume 5,110 m³/year, fully allocated HPP becomes approximately Rp37,500/m³.

This is a critical finding: a social selling price of Rp15,000/m³ does not recover fully allocated CAPEX. The water tariff may need subsidy, grant-funded CAPEX, a higher-value delivered-water service, or cross-subsidy from ice service.

### HPP ice slurry

At 36,500 kg/year nameplate:

- Allocated cash operating cost to ice at 30% OPEX: Rp28.5 juta/year.
- Allocated CAPEX over 10 years to ice package and shared controls: approximately Rp27 juta/year.
- Indicative fully allocated HPP: `(28.5 + 27) juta / 36,500 kg = approximately Rp1,520/kg` before distribution and peak-demand inefficiency.

A selling price of Rp4,500/kg leaves contribution for shared overhead and service quality, but the actual HPP must be recalculated after measuring electricity and maintenance per kg.

---

## 10. Revenue Model

Base selling-price assumptions:

- Water: Rp15,000/m³, equivalent to Rp15/L only if sold as a delivered/treated service; packaging and delivery are excluded.
- Ice slurry: Rp4,500/kg.
- Subscription: not included in base revenue to avoid double counting; can be added after customer validation.

At 70% utilisation:

- Water revenue: `14 m³/day x Rp15,000 x 365 = Rp76.65 juta/year`.
- Ice revenue: `70 kg/day x Rp4,500 x 365 = Rp114.98 juta/year`.
- **Total annual revenue = Rp191.63 juta.**
- **Operating profit before tax = Rp191.63 - Rp95.00 = Rp96.63 juta/year.**

Revenue assumes all modeled output is sold. A real model should separately track production, saleable output, booked demand, unpaid loss, and unsold inventory.

---

## 11. Scenario Analysis

| Metric | Conservative | Base | Optimistic |
|---|---:|---:|---:|
| Utilisation | 45% | 70% | 90% |
| Water sold/day | 9 m³ | 14 m³ | 18 m³ |
| Ice sold/day | 45 kg | 70 kg | 90 kg |
| Households equivalent | 90 | 140 | 180 |
| Trips served/day | 2.25 | 3.5 | 4.5 |
| Annual revenue | Rp123.2m | Rp191.6m | Rp246.4m |
| Annual OPEX | Rp95.0m | Rp95.0m | Rp101.0m |
| Operating profit | Rp28.2m | Rp96.6m | Rp145.4m |
| Simple payback on Rp1.25bn | 44.3 years | 12.9 years | 8.6 years |
| Simple ROI | 2.3% | 7.7% | 11.6% |

The optimistic case is not automatically the expected case. It requires confirmed demand, repeat purchase, sufficient storage, reliable production, and collection discipline. The conservative case shows that low utilisation makes the project financially weak even though the service may remain socially valuable.

---

## 12. Sensitivity Analysis

Illustrative base operating profit is Rp96.6 juta/year.

| Change from base | Approximate effect | Interpretation |
|---|---:|---|
| Utilisation -20% relative | profit falls to approximately Rp58m/year | demand is the strongest early risk |
| Utilisation +20% relative | profit rises to approximately Rp135m/year | payback improves to about 9.3 years |
| Selling prices -20% | profit falls to approximately Rp58m/year | water tariff and ice price matter materially |
| Selling prices +20% | profit rises to approximately Rp135m/year | must be validated against willingness to pay |
| CAPEX +20% | payback becomes approximately 15.5 years | quotation and civil-work risk |
| OPEX +30% | profit falls to approximately Rp68m/year | maintenance and staffing risk |
| PV generation -20% | production scheduling tightens; revenue impact depends on storage and demand | rainy season and shading risk |
| Ice COP/energy intensity worsens 30% | direct cost increases modestly in this model, but capacity/peak operation suffers | validate with test data |

Sensitivity should be replaced with a monthly cash-flow model using actual quotations and local demand. NPV and IRR are not reported as decision-grade figures yet because discount rate, tax, grants, replacement timing, residual value, and financing terms are not provided.

---

## 13. Financial Feasibility

### Simple 10-year cash-flow illustration

| Year | Net cash flow before financing |
|---:|---:|
| 0 | -Rp1,250m |
| 1-10 | +Rp96.6m/year in base case |
| 10-year cumulative before residual value | -Rp284m |

With constant base-case profit, the project does **not** recover the initial CAPEX within 10 years. A 10-year replacement/maintenance event would worsen this result. The project can be defensible as a community utility and climate-resilience project, but the business case needs one or more of the following:

1. grant or concessional financing for PV, RO, and initial infrastructure;
2. larger demand cluster and higher utilisation;
3. paid cold-chain service based on value created, not commodity ice price alone;
4. delivery/subscription revenue with validated willingness to pay;
5. modular deployment that defers CAPEX until demand is proven;
6. equipment procurement and service contract that reduce downtime and replacement shocks.

A grant covering 40% of CAPEX reduces equity investment to Rp750m; base-case simple payback on remaining investment becomes about 7.8 years. This is still not a guarantee and must be modeled with the actual grant conditions.

---

## 14. Operational Scheduling

The following is a starting schedule, subject to EMS optimisation:

| Time | Priority activity |
|---|---|
| 05:00-08:00 | minimum water treatment, tank check, battery-supported controls |
| 08:00-12:00 | high-solar water production and ice production |
| 12:00-15:00 | peak production, storage replenishment, battery charging |
| 15:00-18:00 | production based on booking and next departure schedule |
| 18:00-06:00 | storage-based service and critical-load operation only |

Water quality and cold-chain safety override revenue optimisation. The operator must have manual fallback procedures for sensor failure, low SOC, poor water quality, compressor trip, and extreme weather.

---

## 15. Risk Analysis

| Risk | Consequence | Mitigation |
|---|---|---|
| Actual solar resource lower than assumed | production deficit | monthly resource model, oversize PV, load shifting |
| Feedwater quality varies | membrane fouling and unsafe product water | lab test, pretreatment, scheduled cleaning, quality monitoring |
| Demand below 70% | weak revenue and long payback | pre-sales, booking, pilot, modular capacity |
| Compressor or pump downtime | cold-chain service interruption | spare parts, service contract, redundancy |
| Battery degradation | reduced night reliability | conservative DoD, SOC monitoring, replacement reserve |
| Brine disposal impact | environmental and permit risk | site-specific discharge study and permit |
| Unverified fish-price uplift | overclaimed impact | measure quality loss and realised price during pilot |
| AI recommendation failure | bad scheduling | human approval, rule-based fallback, audit logs |
| Tariff affordability | low household adoption | tiered/social tariff, subsidy, cross-subsidy |
| Collection/payment leakage | lower cash flow | prepaid credits, transparent meter and daily reconciliation |

---

## 16. Implementation Roadmap

### Phase 1 - Survey, 0-3 months

Confirm roof structure, solar resource, water source and quality, brine pathway, household demand, fishing schedule, fish price, willingness to pay, and local permits.

### Phase 2 - Pilot, 4-8 months

Deploy water-treatment pilot and approximately 30 kg/day ice-slurry demonstrator. Collect energy, temperature, water quality, maintenance, demand, and fish-quality data.

### Phase 3 - Commissioning, 9-12 months

Install the selected PV, EMS, storage, commercial water package, and modular ice system. Run acceptance tests for kWh/m³, kWh/kg, production capacity, temperature, water quality, and uptime.

### Phase 4 - Commercial operation, year 2

Operate with prepaid water/ice credits, monthly KPI review, service-level monitoring, and human-approved scheduling recommendations.

### Phase 5 - Scaling, year 3 onward

Add ice and water modules only after demand exceeds 70% of installed capacity for at least three consecutive months and cash collection is proven.

---

## 17. Data Quality and Required Validation

| Data | Current status | Required validation |
|---|---|---|
| PV irradiation | Engineering assumption | location-specific monthly dataset |
| Roof area | Engineering assumption | structural and shade survey |
| RO energy and recovery | Engineering assumption | vendor datasheet and pilot test |
| Ice energy intensity | Engineering assumption | acceptance test at local ambient/seawater condition |
| Water price | Market assumption | household survey and competitor check |
| Ice price | Market assumption | fisherman interviews and booking trial |
| Fish quality-loss reduction | Simulation assumption | controlled before/after pilot |
| CAPEX | Market estimate | minimum three quotations |
| OPEX | Engineering/market assumption | operator and service quotations |
| AI performance | Simulation only | sensor dataset and forecast validation |

No external web research or verified local quotation was available in the workspace when this report was prepared. URLs and named sources should be added after the location is selected. Priority source list: BMKG or Global Solar Atlas/World Bank for solar resource; BPS, KKP, and local fisheries offices for demographics/fishing data; laboratory analysis for feedwater; manufacturer datasheets and vendor quotations for equipment; and PLN/ESDM factors for any avoided-emission calculation.

---

## 18. Final Recommendation

Proceed with a **data-first pilot**, not an immediate full-scale commercial claim. The technical concept is plausible, and the energy balance is internally consistent at average design conditions. The financial model is credible precisely because it does not hide the weak points: base-case payback is about 12.9 years before financing, while low utilisation produces an unattractive return.

The strongest competition narrative is therefore:

- Kopdes owns and operates a modular productive utility;
- PV reduces exposure to diesel/grid energy;
- water service addresses a daily community need;
- ice slurry addresses a measurable fish-quality problem;
- AI starts as transparent forecasting and scheduling support;
- pilot data determines expansion and prevents overclaiming.

---

## 19. Ten Pitch-Deck Numbers

| Metric | Value | Unit | Basis |
|---|---:|---|---|
| PV capacity | 50 | kWp | Engineering assumption |
| Energy production | 175 | kWh/day | 50 x 4.5 PSH x 0.78 PR |
| Clean-water production | 20 | m³/day | Engineering assumption |
| Ice-slurry production | 100 | kg/day | Hub design assumption |
| Households served | 140 | households | Base 70%, 100 L/household/day |
| Fishermen/trips served | 3.5 | trips/day | 70 kg/day / 20 kg/trip |
| Water selling price | 15,000 | Rp/m³ | Market assumption |
| Ice-slurry price | 4,500 | Rp/kg | Market assumption |
| Kopdes operating profit | 8.1 | Rp million/month | Base case before tax/financing |
| Payback period | 12.9 | years | Rp1.25bn CAPEX, base cash flow |

Additional defensible pitch numbers, subject to pilot validation: illustrative fisherman benefit **Rp39,500/trip** and annual base-case revenue **Rp191.6 juta**. Do not present them as verified local outcomes until field data is collected.
