# Spec Delta

## Purpose

Enables conversational meal intake recording through natural language understanding, entity extraction, and proactive clarification loops for ambiguous dietary descriptions.

## ADDED Requirements

### Requirement: Natural Language Meal Intake Parsing
The system SHALL accept unstructured Traditional Chinese meal descriptions and classify user intent into meal logging, nutritional querying, daily intake analysis, or diet recommendation.

#### Scenario: User logs composite Taiwanese dish
- **WHEN** user inputs "我剛剛吃了一碗加蛋的台式牛肉麵，湯喝了一半"
- **THEN** system classifies intent as `log_food` and extracts entities for beef noodles, egg, and partial soup intake.

### Requirement: Food Named Entity Recognition (NER)
The system SHALL extract food entity names, portion descriptions, meal types, and consumption timestamps from user input text.

#### Scenario: Extracting multiple food items in one prompt
- **WHEN** user inputs "早餐吃了一顆茶葉蛋和一杯中溫無糖豆漿"
- **THEN** system extracts two distinct items: "茶葉蛋" (1 piece) and "無糖豆漿" (1 medium cup, warm).

### Requirement: Proactive Ambiguity Clarification
The system SHALL trigger a clarification loop to prompt the user when food identity or portion details are ambiguous or confidence score is below 0.80.

#### Scenario: Vague portion trigger
- **WHEN** user inputs "午餐吃了一碗泡麵" without specifying brand or broth consumption
- **THEN** system prompts user with targeted clarification questions regarding brand, size, and whether the broth was finished.

