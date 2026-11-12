# Spec Delta

## Purpose

Retrieves nutritional data across multiple authoritative databases and computes bounded caloric and macronutrient uncertainty intervals to eliminate LLM hallucination.

## ADDED Requirements

### Requirement: Multi-Source Nutritional Grounding
The system SHALL query USDA FoodData Central, Taiwan FDA Food Composition Database, and a local Taiwanese cuisine vector database to retrieve nutritional candidate profiles.

#### Scenario: Grounding Taiwanese local cuisine
- **WHEN** user logs "蚵仔煎" or "珍珠奶茶"
- **THEN** system queries the Taiwan FDA database and local vector store, returning calibrated nutritional density values per 100 grams.

### Requirement: Uncertainty Quantification and Range Estimation
The system SHALL output lower, point, and upper estimated caloric boundaries based on entity match confidence and portion description ambiguity rather than a false-precision point estimate.

#### Scenario: Calorie range generation for estimated meal
- **WHEN** user logs an estimated meal portion with 85% match confidence
- **THEN** system presents a bounded caloric interval such as "320–410 kcal" alongside the primary estimate and explicit data sources.

