# Validation & Evidence Status

This document separates **verified system behavior** from **future scientific validation work** so that SIH judges can see exactly what the prototype currently proves.

## Software validation

The project includes backend tests under `backend/tests/` covering important platform behavior such as authentication/RBAC, health reporting, AIS state handling, investigation-case behavior, correlation workflow, evidence timelines, and jurisdiction services.

Before a final SIH submission, run the complete suite against the exact deployment being demonstrated and retain **one authoritative final report**. Do not publish conflicting summaries from different runs.

Example local command:

```bash
cd backend
pytest -q
```

## SAR detector status

The current image-analysis component is an **experimental SAR dark-spot candidate detector**. It is intended to identify areas that deserve investigation; it is not presented as a validated oil-spill classifier.

SAR dark areas can also be produced by low-wind zones, biogenic films, wakes, upwelling and other oceanographic effects. Therefore detector output is combined with provenance, human review and vessel-correlation evidence rather than treated as proof by itself.

## Vessel-correlation validation

Candidate ranking is designed to be explainable. The investigation layer considers factors such as spatial proximity, temporal proximity, track continuity, heading compatibility, environmental drift compatibility and AIS reliability. The system can reduce/cap confidence when the evidence is weak or ambiguous.

A ranked vessel is therefore an **investigation candidate**, not an automatic attribution of legal responsibility.

## Next scientific validation steps

A stronger post-hackathon validation programme should use a labelled benchmark of confirmed spill and non-spill SAR scenes and report appropriate metrics such as precision, recall, F1 and segmentation overlap/IoU where applicable. Vessel-attribution performance should be evaluated separately on incidents with independently verified vessel histories.

## Reproducibility principle

For judge demonstrations, prefer a stored reference case with source/provenance information over scripted numbers. Live-feed status and stored historical evidence should remain visibly distinct in the UI.
