## Disclaimer

Please note that the code provided in this repository is not the actual production version due to strict confidentiality and data privacy protocols. The files included are dummy versions designed to protect proprietary information while maintaining the exact system structure and logic flow described in the project documentation. For further technical details regarding the architecture, data engineering, and performance results, please consult the full report available as a PDF in the docs folder.

# Real Estate Assistant Chatbot - Splendom Suites Group

This repository contains the engineering documentation and architecture for the Splendom Suites intelligent virtual assistant as shown in the image below. The project focuses on automating B2B queries for corporate housing and executive stays in major European cities like Vienna, Madrid, and Barcelona.

![Splendom Chatbot User Interface](docs/splendom-chatbot.png)

## Project Overview

The primary challenge was to unify fragmented data (from over 30 Word documents and multiple Excel spreadsheets) into a single, automated interface. The system has evolved from a monolithic LLM-based prototype to a high-performance modular NLU/NER pipeline optimized for resource-constrained environments.

## Technical Evolution

### Phase 1: Monolithic Approach (V1)
The initial prototype relied on a Llama3-70B model acting as a general reasoning engine.
* Infrastructure: Non-private cloud-based LLM.
* Performance Issues: High latency, frequent hallucinations (inventing amenities), and unscalable regex-based data parsing.
* Accuracy: Global success rate of 73.8%.

### Phase 2: Modular Engineering (V2) - Current
The system was re-engineered into a modular pipeline to ensure data privacy and efficiency on a standard 8GB RAM VPS.
* NLU Router: A fine-tuned RoBERTa-base model for intent classification (Discovery, Details, Other).
* NER Extractor: A specialized spaCy/RoBERTa component for identifying addresses, bedroom types, and over 50 specific amenity classes.
* Logic Layer: Transitioned from probabilistic generation to deterministic filtering using a Python backend and a consolidated JSON inventory.
* Inference: Runs in milliseconds on a CPU.

## Performance Metrics

Validation tests on the modular architecture show significant improvements in reliability and speed:

* NLU Intent Classification: 0.9776 F1-Score.
* NER Entity Extraction: ~0.94 Global F1-Score.
* Data Accuracy: 100% (elimination of hallucinations via deterministic retrieval).
* Latency: Sub-second response times on local hardware.

### Key Entity Performance (Sample)
| Entity Type | Category | Precision | Recall | F1-Score |
| :--- | :--- | :--- | :--- | :--- |
| ADDRESS | Location | 0.9907 | 1.0000 | 0.9953 |
| APARTMENT NAME | Identifier | 1.0000 | 1.0000 | 1.0000 |
| TWO BEDROOM | Unit Type | 0.9672 | 0.9833 | 0.9752 |
| INTERNET | Amenity | 0.9518 | 0.9875 | 0.9693 |

## System Architecture

1. Data Ingestion: Automated pipeline with data augmentation (expanding 400 real descriptions to 10,000 synthetic variations for robust training).
2. Intent Routing: Input is classified to determine if it is a search query or a specific property question.
3. Entity Recognition: Extraction of structured data from natural language.
4. Deterministic Query: Python-based filtering against the master JSON inventory.
5. Secure Response: Information delivery without external API dependencies.

## Roadmap (Phase 3)

* Local NLG: Integration of quantized SLMs (Small Language Models) like Phi-3 or Llama-3-8B (4-bit) for human-like response generation.
* Multi-Label Classification: Handling complex queries with overlapping intents.
* Contextual Query Rewriting: Maintaining conversation history for multi-turn interactions.
* Local RAG: Implementing vector-based retrieval for internal policy manuals.

## Technologies Used

* Models: RoBERTa-base, Llama3-70B (for data synthesis).
* Frameworks: spaCy, Transformers (Hugging Face), Flask.
* Data: Pandas, NumPy, Regex.
* Deployment: Optimized for 8GB RAM VPS (CPU-only inference).

## Author

* Gonzalo Torras Serrano - AI Engineering and Systems Development for Splendom Suites Group.
