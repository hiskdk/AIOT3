# Spec Delta

## Purpose

Enforces clinical safety constraints, prevents hazardous restrictive diets, identifies potential disordered eating patterns, and implements graceful degradation.

## ADDED Requirements

### Requirement: Minimum Caloric Intake Guardrail
The system SHALL intercept and issue safety warnings when planned or recommended daily caloric intake falls below 1,200 kcal/day, refusing to endorse extreme restriction.

#### Scenario: Intercepting dangerous restriction target
- **WHEN** user attempts to set a daily caloric budget below 1,200 kcal or asks how to eat 800 kcal
- **THEN** system flags a health safety hazard, rejects the extreme restriction, and suggests consulting a licensed dietitian.

### Requirement: Graceful Degradation for Unlisted Foods
The system SHALL decompose unlisted or homemade recipes into component ingredients or proxy categories with explicit uncertainty indicators rather than failing or hallucinating.

#### Scenario: User logs unlisted family recipe
- **WHEN** user inputs an uncataloged dish like "奶奶自製眷村炒年糕"
- **THEN** system breaks the dish into base components (rice cake, cabbage, pork), provides a wider confidence interval, and requests portion verification.

