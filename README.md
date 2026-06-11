# CHZHVAN CG01 Gas Meter Teardown

> please enjoy this teardown of this interesting CG01 gas meter. I was impressed at the full color screen and wanted to do a quick teardown of it in order to evaluate what possibilities it might have for becoming a wireless home sensor. it appears that there is a 4 pin, potentially UART connection to be made here, but I haven't yet done any firmware dump. I've used codex and gemini to do a bit more analysis and organization of the teardown and photos. there isn't much room in the case for an additional wifi component and I'm a bit concerned with the fact that  mains voltage is wired directly on the same board as the low voltage components. further testing/development will likely include a separate 5 volt power source and skip the wall plug altogether. also, not pictured is the speaker which i removed directly after opening. this device is probably not reliable enough to act as a primary carbon monoxide sensor anyways

This is a quick teardown of the CHZHVAN CG01, sold as a low-cost combined combustible gas and carbon monoxide detector. The interesting parts are the surprisingly nice full-color display, a compact single-board design, and a likely microcontroller debug/test header that may make it possible to repurpose the unit as a wireless home sensor after more probing.

The important caveat: this teardown is not a validation of the device's gas or CO accuracy. Do not treat this as a primary life-safety carbon monoxide alarm. Use certified CO alarms for that job.

![CG01 opened on the bench with display, PCB, screws, and the round plug-in case separated](assets/photos/01-exploded-layout.jpg)

## Device Identity

The outside label identifies the unit as a CHZHVAN CG01 "Combined Gas & Carbon Monoxide Alarm." The casing and PCB also carry HRD-GC02 markings, which matches a related Chinese appearance-design publication for "Methane and Carbon Monoxide Alarm (HRD-GC02)."

![External label showing CHZHVAN CG01 and AC input rating](assets/photos/02-label-model-input.jpg)

![External label showing 5 V / 1 A max wording and Shenzhen BAOWODA manufacturer line](assets/photos/03-label-power-manufacturer.jpg)

Observed labels and manual details:

| Field | Value |
| --- | --- |
| Brand on unit | CHZHVAN |
| Product model | CG01 |
| Product description | Combined Gas & Carbon Monoxide Alarm / Combustible Gas & CO Detector |
| Input rating on label | AC 100-240 V, 50-60 Hz |
| Working voltage/current line | 5 V, 1 A max |
| Manufacturer on label/manual | Shenzhen BAOWODA Technology Co., Ltd. |
| Manual product name | Combustible Gas & CO Detector |
| Manual model | CG01 |
| Internal molded marking | HRD-GC02-2 |
| PCB marking | HRD-GC02 Ver1.4 |

Related appearance-design publication:

| Field | Value |
| --- | --- |
| Title | Methane and Carbon Monoxide Alarm (HRD-GC02) |
| Publication number | CN309611093S |
| Application number | 202530119920.4 |
| Application date | 2025-03-13 |
| Publication date | 2025-11-18 |
| Applicant / patentee | Shenzhen Huerdun Technology Co., Ltd. |
| Designer | Guo Zhihua |
| Main classification | 10-05 |
| Agency | Guangdong Shenke Law Firm |
| Agent | Wu Jun |
| Address | Room 305, Building G, No. 62, Puxia Road, Liuyue North Community, Henggang Street, Longgang District, Shenzhen, Guangdong Province, 518000, China |
| Key design feature | Shape |
| Best representative view | Perspective view 1 |

I would treat the BAOWODA/CHZHVAN/HRD-GC02 naming as a likely shared product, OEM, or design-family relationship rather than proof that all records refer to the exact same retail SKU.

## Manuals And Product Links

Manuals.plus PDFs:

- [Manuals.plus PDF 1](https://manuals.plus/m/8d3ae95211ce29aa4cd40857e15c04374e0e1ad29cb9e1e3573d071be3636ebb.pdf)
- [Manuals.plus PDF 2](https://manuals.plus/m/a9fce58f3bb8f634b40b0ca3a2581a542f50115a1a8de5016d965f432112a3ad.pdf)
- [Manuals.plus PDF 3](https://manuals.plus/m/899777f4e84a9016b76af7b2c6e4fcfb5102312d0254f2d464ec3424154c0093.pdf)

Retail listing:

- [AliExpress listing](https://www.aliexpress.us/item/3256809830015016.html?gatewayAdapt=glo2usa4itemAdapt)

Manual cover and declaration excerpt used while reviewing this device:

![Manual cover for CHZHVAN Combustible Gas & CO Detector model CG01](assets/manual/manual-cover.png)

![Manual declaration page identifying Shenzhen BAOWODA and CG01](assets/manual/manual-page-02.png)

## Manual-Derived Specs

The English section of the CG01 manual gives these working specs:

| Spec | Manual value |
| --- | --- |
| Housing size | 65 mm diameter, 23 mm thick |
| Product power | Less than 3 W |
| Sensor life | 5 years |
| Preheat time | About 180 seconds before normal detection |
| LPG / combustible gas range | 0-8000 ppm |
| CO range | 0-999 ppm |
| Temperature display range | 0-100 C / 32-212 F |
| Relative humidity range | 0-99% RH |
| Use environment | -10 C to 55 C, less than 95% RH, no condensation |
| Alarm method | Sound and light alarm with LCD display |
| Alarm volume | Greater than 85 dB |

The manual's combustible gas bar thresholds are:

| Display color | Gas concentration |
| --- | --- |
| Green | Less than 1000 ppm, below 2% LEL |
| Yellow | 1000-2000 ppm, 2-4% LEL |
| Orange | 2000-3000 ppm, 4-6% LEL |
| Red | More than 3000 ppm, above 6% LEL; manual says alarm within 30 seconds |

The manual's CO response table is:

| CO level | Response time |
| --- | --- |
| 27 +/- 3 ppm | More than 120 min |
| 55 +/- 5 ppm | 60-90 min |
| 110 +/- 10 ppm | 10-40 min |
| 330 +/- 30 ppm | Less than 3 min |

The manual also says the unit displays real-time values during warm-up but has no detection function during the preheating period. It recommends monthly testing and says temperature/humidity readings can be biased by heat from the AC-powered unit itself.

## External And Mechanical Design

The product is a very compact round plug-in alarm. The side label shows universal mains input, while the manual declaration lists low-voltage, EMC, RoHS, and gas/CO detector standards. The front display shown in the manual is much more visually polished than expected for a roughly $20-30 detector module.

The opened case is tight. There is very little unused volume for an ESP8266/ESP32-class add-on board unless the speaker/buzzer volume, plug cavity, or another area is sacrificed. Even then, keeping a wireless module inside the mains-powered enclosure is a bad default from a safety and RF-debugging standpoint.

## Display

The display is a small color TFT with a flat-flex cable. This is one of the more interesting parts of the device because the rest of the alarm electronics are very cost-optimized.

![Removed color TFT display module with flat flex cable](assets/photos/04-display-module.jpg)

The display is not just a segmented LCD. The manual artwork and the physical module both indicate a full-color screen used for CO ppm, gas level bars, temperature, humidity, brightness status, alarm status, and icons.

## PCB Power And Sensor Side

![PCB side showing gas sensor, flyback transformer, plug-side power supply area, and button contacts](assets/photos/05-power-sensor-side.jpg)

The sensor side of the board includes:

- A metal-can gas sensor marked by the PCB as `S-GAS`.
- A compact offline power-supply section with transformer, capacitor, and mains-adjacent circuitry.
- A connector/footprint area marked near `BUZ`, likely for the speaker/buzzer that was removed before these photos.
- Spring/button contacts around the perimeter for the side buttons.
- Broad copper areas and exposed plug-related structures that make the board mechanically simple but electrically unpleasant to handle while powered.

The visible gas sensor looks like a low-cost semiconductor style sensor package. Since there is only one obvious metal-can sensor in these photos, selectivity between combustible gas and CO should be treated with skepticism unless independent sensor part identification and calibration testing prove otherwise.

## PCB Logic Side

![Logic side of the PCB showing MCU, display connector, three tactile switches, and unpopulated 4-pad header](assets/photos/06-logic-side-mcu-header.jpg)

The logic side includes:

- Main MCU at `U2`.
- Display FPC connector.
- Three tactile buttons on the edge, including labels for `POWER/TEST` and `DISP/F`.
- A 4-pad unpopulated through-hole header near the MCU.
- A `7533-1` low-dropout regulator at `U1`, likely generating the 3.3 V logic rail.
- Pads for an external crystal footprint at `Y1`.
- A small 8-pin IC at `U3` that was not identified during this teardown.

The single-board construction means mains-derived power and low-voltage logic are physically close. For any firmware or debug work, assume the board is unsafe to connect directly to a laptop, USB UART, SWD probe, logic analyzer, oscilloscope, or grounded bench gear while plugged into the wall.

## MCU And Debug Header

![Close-up of the MCU, 4-pad header, and 7533-1 regulator](assets/photos/07-mcu-header-regulator-closeup.jpg)

Gemini-assisted visual identification suggested the main MCU is probably an Artery Technology AT32 ARM microcontroller. The package marking is partially obscured, but visible `RY`, `ARM`, and AT32-like markings fit that family. Possible candidates from the visual analysis included AT32F403/AT32F415/AT32F425-class parts, with exact suffix unresolved.

The `7533-1` marking at `U1` is consistent with a Holtek HT7533-1 style 3.3 V LDO. That supports the assumption that the MCU/debug header is probably 3.3 V logic, but this still needs meter confirmation.

The 4-pad header is the most interesting future interface. It could be UART, SWD, factory programming, fixture test, or some combination of power/ground/data pins. Nothing in this teardown confirms the pinout. The safe next step is electrical mapping, not blind connection.

Suggested debug workflow:

1. Keep the device disconnected from mains.
2. Identify board ground using continuity to the LDO ground and large low-voltage ground pours.
3. Identify the 3.3 V rail at the LDO output.
4. Map the 4-pad header to ground, 3.3 V, MCU pins, reset, or test points.
5. Power the low-voltage side from a current-limited isolated bench supply only after confirming the expected injection point.
6. Watch the candidate data pins with a 3.3 V logic analyzer during boot for UART output at common baud rates.
7. If the pinout looks like SWD, try a CMSIS-DAP/J-Link/ST-Link style probe with OpenOCD only after confirming voltage and ground.
8. Attempt a firmware read only after verifying the debug interface and accepting that readout protection may be enabled.

## Wireless Sensor Potential

The CG01 is tempting as a Home Assistant or MQTT sensor because it already has:

- A color screen.
- A sensor front-end.
- Local alarm behavior.
- A microcontroller that may have accessible debug/test pads.
- A mains-powered enclosure.

The same points also make it risky. The board is small, heat from the built-in supply already affects temperature/humidity readings according to the manual, and there is not much safe space for an additional Wi-Fi module. A separate 5 V supply and bypassing the wall-plug mains path is the cleaner development path.

Practical future paths:

- Use the CG01 as a donor enclosure/display/sensor board, powered from an isolated 5 V source.
- Sniff UART or SWD if the 4-pad header exposes it.
- Leave the original alarm behavior intact and only mirror readings if a serial protocol is found.
- Mount a wireless bridge externally instead of packing it into the round case.
- Treat the gas/CO readings as hobby telemetry unless calibrated against known reference conditions.

## Safety Notes

- Do not probe this board while it is plugged into mains.
- Do not connect grounded test gear to the board unless it is powered from a safe isolated source.
- Do not assume the low-voltage side is safe just because the MCU uses 3.3 V logic.
- Do not rely on this teardown or this device as a primary CO safety system.
- If used for experiments, keep certified CO and gas alarms installed separately.

## Open Questions

- Exact MCU part number.
- Exact gas sensor part number and sensing chemistry.
- Whether the 4-pad header is UART, SWD, or factory-only test.
- Whether firmware readout protection is enabled.
- Whether the display protocol is MCU-integrated parallel/SPI or handled by another controller path.
- Whether the sensor readings can be extracted digitally without disturbing the alarm's original behavior.
- Whether the device has any meaningful calibration data stored in flash.

## Repository Contents

- `assets/photos/` contains the teardown photos converted from the uploaded HEIC originals to JPEG.
- `assets/manual/` contains small rendered manual excerpts used for identification/context.
- The original chip-identification Markdown note is not included as a separate file; its conclusions were rewritten and integrated into this README.
