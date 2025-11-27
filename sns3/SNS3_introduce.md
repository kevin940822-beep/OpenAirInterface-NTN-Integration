# Refrence
> https://www.sns3.org/
> 
> https://github.com/sns3/sns3-satellite/blob/master/doc/satellite-design.rst

## SNS3 Introduce

- Satellite Network Simulator 3 (SNS3) is a satellite network extension to Network Simulator 3 (ns-3) platform.
- SNS3 models a full interactive multi-spot beam satellite network with a geostationary satellite (GEO).
- transparent star (bent-pipe) payload.

- reference satellite system have 72 spot-beams with an  European coverage.

- serverd by 5 gateways (GWs) and using Ka-band frequencies on the feeder/user link.

- implements the DVB-RCS2/S2 standards.
  - DVB-RCS2 (return link) : Digital Video Broadcast - Return Channel via Satellite - 2nd generation
  - DVB-S2 (forword link) : Digital Video Broadcasting - Satellite - 2nd generation

## Frame configuration (時間與頻寬怎麼切格子)
### Return Link (DVB-RCS2)
- Use **Multi-Frequency Time Division Multiple Access (MF-TDMA)**.
- Composed of:
  1. Superframe sequences
  2. Superframes
  3. Frames
  4. Time slots
  5. Bandwidth Time Units (BTUs)

