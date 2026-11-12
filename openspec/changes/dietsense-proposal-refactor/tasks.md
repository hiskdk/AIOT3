# Tasks

## 1. Project Scaffolding and Environment Setup

- [ ] 1.1 Initialize project directory structure (`src/`, `data/`, `tests/`, `specs/`) and verify files exist
- [ ] 1.2 Configure Python dependencies in `pyproject.toml` (LangChain, Pydantic, FastAPI, ChromaDB) and verify environment setup

## 2. Natural Language Intake and Entity Parsing (NER)

- [ ] 2.1 Implement intent classification module supporting `log_food`, `query_nutrition`, `analyze`, and `recommend` intents and verify with unit tests
- [ ] 2.2 Implement Food NER extraction for Taiwanese dish names, portion descriptors, and meal times and verify with test suite
- [ ] 2.3 Implement proactive clarification loop trigger when match confidence < 80% or portion is ambiguous and verify dialogue flow

## 3. Grounded Nutrition Retrieval & Uncertainty Quantification

- [ ] 3.1 Implement USDA FoodData Central REST client and cache layer and verify API response mapping
- [ ] 3.2 Ingest Taiwan FDA Food Composition Database into local structured lookup and verify Taiwanese dish queries
- [ ] 3.3 Build ChromaDB vector index for Taiwanese night-market and street food descriptions and verify semantic similarity search
- [ ] 3.4 Implement uncertainty interval calculation engine ($[E_{\text{low}}, E_{\text{high}}]$) and verify range math against confidence values

## 4. Energy Analytics Engine & Safety Guardrails

- [ ] 4.1 Implement Mifflin-St Jeor BMR calculator and physical activity multiplier TDEE engine and verify with test benchmarks
- [ ] 4.2 Implement daily nutrient aggregator and macronutrient distribution breakdown (carb/protein/fat ratio) and verify calculation outputs
- [ ] 4.3 Implement clinical safety guardrail checking for <1,200 kcal/day thresholds and disordered eating patterns, verifying safety alerts
- [ ] 4.4 Implement graceful degradation fallback for uncataloged homemade recipes and verify proxy estimation logic

## 5. Integration, User Study Benchmark & Handover Documentation

- [ ] 5.1 Create 500-sample annotated Taiwanese dietary evaluation benchmark dataset and verify format compliance
- [ ] 5.2 Implement end-to-end evaluation pipeline measuring calorie error within ±10% and ±20% margins, logging time, and SUS metrics
- [ ] 5.3 Generate root `DESIGN.md` and `README.md` conforming to academic project handover standards and verify completeness

