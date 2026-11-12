# Spec Delta

## Purpose

Calculates personal basal metabolic rate, total daily energy expenditure, and real-time caloric balance with macronutrient distribution analysis.

## ADDED Requirements

### Requirement: Deterministic Energy Balance Calculation
The system SHALL compute Basal Metabolic Rate using the Mifflin-St Jeor formula and determine Total Daily Energy Expenditure using validated physical activity multipliers.

#### Scenario: Male adult BMR and TDEE evaluation
- **WHEN** user profile specifies male, 70 kg, 175 cm, age 25, sedentary activity
- **THEN** system computes BMR as 1,673.75 kcal and baseline TDEE as 2,008.5 kcal.

### Requirement: Daily Energy and Macronutrient Tracking
The system SHALL aggregate logged food items to evaluate remaining caloric budget against personal TDEE targets and calculate carbohydrate, protein, and fat percentage distributions.

#### Scenario: Real-time caloric deficit reporting
- **WHEN** user requests daily intake status after logging 1,450 kcal against a 2,000 kcal target
- **THEN** system outputs a 550 kcal remaining deficit and displays a macronutrient distribution breakdown.

