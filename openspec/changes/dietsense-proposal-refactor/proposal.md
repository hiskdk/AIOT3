# Proposal: DietSense Domain AI Agent Refactor

## Why

Obesity and overweight affect over 2.6 billion adults globally, with 47.9% of adults in Taiwan classified as overweight or obese. Although dietary tracking doubles the success rate of weight management, traditional calorie logging apps (e.g., MyFitnessPal) suffer from an 85% 30-day drop-off rate due to high cognitive burden: manual database search, ambiguous portions, and manual decomposition of composite dishes. Furthermore, general-purpose LLMs hallucinate nutritional numbers with mean absolute percentage errors exceeding 35%. 

DietSense resolves these bottlenecks by transitioning from passive logging to a proactive, context-aware, and data-grounded Domain AI Agent. By combining natural language understanding with deterministic domain calculation and local Taiwanese food databases, DietSense grounds meal tracking in verifiable facts while protecting user health through automated safety guardrails.

## What Changes

- **Natural Language Meal Logging**: Support unstructured conversational Chinese input ("一碗加蛋台式牛肉麵，湯喝了一半") with Food NER and proactive clarification loops.
- **Multi-Source Grounded Nutrition Retrieval**: Parallel query USDA FoodData Central, the Taiwan FDA Food Composition Database, and a specialized vector database of Taiwanese cuisine.
- **Uncertainty Quantification**: Replace false-precision point estimates with bounded calorie and macronutrient intervals derived from confidence scoring.
- **Energy Analytics Engine**: Compute BMR using the Mifflin-St Jeor equation, estimate personalized TDEE based on physical activity factors, and compare daily intake progress.
- **Clinical & Behavioral Safety Guardrails**: Block extreme calorie deficits (<1,200 kcal/day), detect binge-restrict behavioral patterns, and gracefully handle unknown food entries.

## Capabilities

### New Capabilities

- `diet-agent/natural-language-intake`: Natural language intent classification, food entity extraction (NER), and proactive ambiguity clarification.
- `diet-agent/nutrition-grounding-engine`: Multi-source data retrieval (USDA, Taiwan FDA, local vector DB) with confidence ranking and uncertainty range calculation.
- `diet-agent/energy-analytics-tdee`: Mifflin-St Jeor BMR/TDEE energy balance modeling and macronutrient distribution tracking.
- `diet-agent/safety-guardrails`: Clinical calorie floor enforcement (<1,200 kcal/day), eating disorder warning triggers, and out-of-database graceful degradation.

### Modified Capabilities

*(None - this is the initial project specification architecture)*

## Impact

- **Core System**: Introduces the 8-module DietSense orchestration pipeline (User Scenario → Input Processing → Planner Agent → Data Retrieval → Analytics Engine → Domain Knowledge Integration → Decision Engine → Output/Interaction).
- **APIs & Data Sources**: Integrates USDA FoodData Central REST API, Taiwan FDA Food Database (Open Data), and Chroma/FAISS local vector database.
- **Dependencies**: Python 3.11+, LangChain / LlamaIndex agentic framework, Whisper API (optional speech-to-text), and Pydantic schema validation.

