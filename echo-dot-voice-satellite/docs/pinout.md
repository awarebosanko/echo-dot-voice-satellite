# FPC Signal Analysis (In Progress)

Goal: identify which lines on the FPC ribbon between Board 1 (processor)
and Board 2 (mic/LED/button) carry:
- Power / ground
- I2S clock, word-select, data (from the TLV320ADC3101 ADCs)
- Any control/enable lines for the mics or LED driver

## Method

- Continuity-trace each FPC pin back to its component on Board 2
- Confirm digital audio lines with a logic analyzer while the unit was
  still stock-powered (before final desoldering), or by referencing the
  TLV320ADC3101 datasheet pinout against traced connections
- Cross-check against the TLV320ADC3101 and MT8516 datasheets for
  expected I2S/TDM signal names

## Findings

_TBD — populate once signal tracing is done._
