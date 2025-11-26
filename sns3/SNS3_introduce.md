# Refrence
>https://www.sns3.org/

## SNS3 Introduce

- Satellite Network Simulator 3 (SNS3) is a satellite network extension to Network Simulator 3 (ns-3) platform.
- SNS3 models a full interactive multi-spot beam satellite network with a geostationary satellite (GEO).
- transparent star (bent-pipe) payload.

- reference satellite system have 72 spot-beams with an  European coverage.

- 5 gateways (GW) and Ka-band frequencies as feeder/user link.

- implements the DVB-RCS2 (RTN) - DVB-S2 (FW) standards.

## Frame configuration (時間與頻寬怎麼切格子)
### Return Link (DVB-RCS2)
- Use **Multi-Frequency Time Division Multiple Access (MF-TDMA)**.
- Composed of:
  1. Superframe sequences
  2. Superframes
  3. Frames
  4. Time slots
  5. Bandwidth Time Units (BTUs)

