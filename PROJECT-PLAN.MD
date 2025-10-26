# Style Guide Generator - Project Plan

## Project Overview
Create an AI-powered style guide generator that analyzes input texts/articles and produces a detailed style guide. This guide can be used in future prompts to replicate the writing style with high precision for content generation or translation tasks.

## Tech Stack
- **Backend Framework**: Quarkus (Java)
- **Search/Storage**: Elasticsearch
- **LLM Integration**: LangChain4j
- **Build Tool**: Maven/Gradle

## Architecture Overview

### Components
1. **Text Ingestion Service** - Accept and process input articles/texts
2. **Style Analysis Engine** - Extract style patterns using LLM
3. **Style Guide Generator** - Create structured, reusable style guides
4. **Storage Layer** - Elasticsearch for indexing texts and style guides
5. **API Layer** - RESTful endpoints for CRUD operations

## Core Features

### Phase 1: MVP (Minimum Viable Product)
1. **Text Input API**
   - POST endpoint to accept text samples (articles, documents)
   - Support for multiple text formats (plain text, markdown, JSON)
   - Text preprocessing and cleaning
   - Store texts in Elasticsearch

2. **Style Analysis**
   - Integrate LangChain4j with LLM (OpenAI/Azure/Local models)
   - Analyze multiple dimensions:
     - Tone (formal, casual, professional, conversational)
     - Voice (active/passive, first/second/third person)
     - Vocabulary level and word choice patterns
     - Sentence structure and length patterns
     - Paragraph organization
     - Use of rhetorical devices
     - Technical vs. layman terminology
     - Punctuation patterns
     - Use of idioms, metaphors, analogies

3. **Style Guide Generation**
   - Aggregate analysis from multiple text samples
   - Generate comprehensive style guide with:
     - Overall style summary
     - Specific writing rules and patterns
     - Example phrases and structures
     - Do's and Don'ts
     - Sample transformations
   - Output format: Structured JSON + Markdown prompt template

4. **Storage & Retrieval**
   - Store original texts with metadata in Elasticsearch
   - Index generated style guides
   - Quick retrieval by ID or search

### Phase 2: Enhanced Features
1. **Style Comparison**
   - Compare multiple texts to find common patterns
   - Identify style variations and inconsistencies
   - Statistical analysis of style elements

2. **Style Guide Templates**
   - Pre-built templates for common styles (academic, blog, technical, marketing)
   - Customizable template structure

3. **Validation & Testing**
   - Test generated style guide against sample text
   - Measure style similarity/accuracy
   - Iterative refinement suggestions

4. **Batch Processing**
   - Process multiple documents at once
   - Aggregate analysis across document sets

### Phase 3: Advanced Features
1. **Style Transfer Testing**
   - Generate sample content using the style guide
   - Compare against original to validate accuracy
   - A/B testing for style guide effectiveness

2. **Multi-language Support**
   - Support for different languages
   - Language-specific style patterns

3. **Version Control**
   - Track style guide versions
   - Compare different versions
   - Rollback capabilities

4. **Export Options**
   - Export as prompt templates for different LLMs
   - Format-specific exports (ChatGPT, Claude, etc.)

## Data Model

### Elasticsearch Indices

#### 1. `source_texts` Index
```json
{
  "id": "uuid",
  "title": "string",
  "content": "text",
  "author": "string",
  "source_url": "string",
  "word_count": "integer",
  "created_at": "date",
  "metadata": {
    "language": "string",
    "category": "string",
    "tags": ["array"]
  },
  "style_guide_id": "string"
}
```

#### 2. `style_guides` Index
```json
{
  "id": "uuid",
  "name": "string",
  "description": "text",
  "source_text_ids": ["array"],
  "created_at": "date",
  "updated_at": "date",
  "analysis": {
    "tone": "object",
    "voice": "object",
    "vocabulary": "object",
    "sentence_structure": "object",
    "paragraph_structure": "object"
  },
  "rules": ["array"],
  "examples": ["array"],
  "prompt_template": "text",
  "statistics": "object"
}
```

## API Endpoints

### Text Management
- `POST /api/v1/texts` - Upload text sample
- `GET /api/v1/texts/{id}` - Retrieve text
- `GET /api/v1/texts` - List all texts (paginated)
- `DELETE /api/v1/texts/{id}` - Delete text

### Style Guide Operations
- `POST /api/v1/style-guides/generate` - Generate style guide from text IDs
- `GET /api/v1/style-guides/{id}` - Retrieve style guide
- `GET /api/v1/style-guides` - List all style guides (paginated)
- `PUT /api/v1/style-guides/{id}` - Update style guide
- `DELETE /api/v1/style-guides/{id}` - Delete style guide

### Analysis
- `POST /api/v1/analyze/text` - Quick analysis of single text
- `POST /api/v1/analyze/compare` - Compare multiple texts
- `POST /api/v1/validate` - Validate generated content against style guide

### Export
- `GET /api/v1/style-guides/{id}/export/prompt` - Export as LLM prompt
- `GET /api/v1/style-guides/{id}/export/markdown` - Export as Markdown

## Technical Implementation

### Quarkus Configuration
- Quarkus RESTEasy Reactive for REST endpoints
- Quarkus Elasticsearch client
- SmallRye OpenAPI for API documentation
- Quarkus Scheduler for background jobs
- Quarkus Cache for performance
- Quarkus Validation for input validation

### LangChain4j Integration
- Configure LLM provider (OpenAI, Azure, etc.)
- Create prompt templates for style analysis
- Implement chain-of-thought analysis
- Use structured output parsing
- Implement retry logic and error handling

### Elasticsearch Setup
- Define mappings for text and style guide indices
- Configure analyzers for text processing
- Set up aggregations for statistics
- Implement search with filters

### Prompt Engineering Strategy

#### Style Analysis Prompt Template
```
Analyze the following text and extract detailed style characteristics:

TEXT:
{input_text}

Provide a comprehensive analysis including:
1. Tone and Voice
2. Vocabulary Level and Word Choice
3. Sentence Structure Patterns
4. Paragraph Organization
5. Rhetorical Devices
6. Specific Writing Patterns
7. Grammar and Punctuation Style
8. Cultural or Domain-Specific Elements

Format your response as structured JSON.
```

#### Style Guide Generation Prompt Template
```
Based on the following style analyses from multiple texts, create a comprehensive 
style guide that can be used to replicate this writing style:

ANALYSES:
{aggregated_analyses}

Create a style guide that includes:
1. Overall Style Summary
2. Specific Rules and Guidelines
3. Example Phrases and Structures
4. Do's and Don'ts
5. A reusable prompt template for content generation

The output should be precise enough to replicate the style accurately.
```

## Development Roadmap

### Week 1-2: Project Setup & Infrastructure
- [ ] Initialize Quarkus project with required dependencies
- [ ] Set up Elasticsearch (local/Docker)
- [ ] Configure LangChain4j with LLM provider
- [ ] Create basic project structure
- [ ] Set up testing framework

### Week 3-4: Core Text Management
- [ ] Implement text ingestion API
- [ ] Create Elasticsearch mappings and indices
- [ ] Build text CRUD operations
- [ ] Add input validation and error handling
- [ ] Write unit tests

### Week 5-6: Style Analysis Engine
- [ ] Design and implement LangChain4j integration
- [ ] Create style analysis prompts
- [ ] Build analysis pipeline
- [ ] Implement result parsing and storage
- [ ] Test with various text samples

### Week 7-8: Style Guide Generation
- [ ] Implement aggregation logic for multiple texts
- [ ] Create style guide generation prompts
- [ ] Build style guide data structure
- [ ] Implement CRUD operations for style guides
- [ ] Create export functionality

### Week 9-10: API & Integration
- [ ] Implement all REST endpoints
- [ ] Add OpenAPI documentation
- [ ] Create integration tests
- [ ] Performance optimization
- [ ] Error handling and logging

### Week 11-12: Testing & Refinement
- [ ] End-to-end testing
- [ ] Validate style guide accuracy
- [ ] Performance tuning
- [ ] Documentation
- [ ] Deployment preparation

## Testing Strategy

### Unit Tests
- Service layer tests
- Elasticsearch repository tests
- LangChain4j integration tests
- Utility and helper function tests

### Integration Tests
- API endpoint tests
- End-to-end workflow tests
- Elasticsearch integration tests
- LLM integration tests (with mocking)

### Quality Assurance
- Test with diverse text samples
- Validate style guide effectiveness
- Performance benchmarking
- Security testing

## Configuration Management

### Application Properties
```properties
# Elasticsearch
elasticsearch.hosts=localhost:9200
elasticsearch.username=${ES_USERNAME:elastic}
elasticsearch.password=${ES_PASSWORD:changeme}

# LangChain4j
langchain4j.provider=${LLM_PROVIDER:openai}
langchain4j.api-key=${LLM_API_KEY}
langchain4j.model=${LLM_MODEL:gpt-4}
langchain4j.temperature=0.3
langchain4j.max-tokens=2000

# Application
app.max-text-length=50000
app.min-texts-for-guide=2
app.cache.ttl=3600
```

## Security Considerations
- API key management for LLM providers
- Input validation and sanitization
- Rate limiting for API endpoints
- Elasticsearch authentication
- CORS configuration
- Sensitive data handling

## Monitoring & Logging
- Request/response logging
- LLM API usage tracking
- Performance metrics
- Error tracking and alerting
- Elasticsearch health monitoring

## Future Enhancements
- Web UI for interactive style guide creation
- Real-time style analysis
- Collaborative style guide editing
- Integration with content management systems
- Plugin system for custom analyzers
- Machine learning for style classification
- GraphQL API support

## Success Metrics
- Accuracy of style replication (human evaluation)
- API response time < 5s for analysis
- Support for texts up to 50,000 words
- 95%+ uptime
- Style guide generation time < 30s for 5 documents

## Dependencies
```xml
<!-- Key Maven Dependencies -->
- io.quarkus:quarkus-resteasy-reactive-jackson
- io.quarkus:quarkus-elasticsearch-java-client
- dev.langchain4j:langchain4j-quarkus
- dev.langchain4j:langchain4j-open-ai
- io.quarkus:quarkus-smallrye-openapi
- io.quarkus:quarkus-cache
- io.quarkus:quarkus-scheduler
```

## Getting Started

### Prerequisites
- Java 17+
- Maven 3.8+
- Docker (for Elasticsearch)
- LLM API key (OpenAI/Azure/etc.)

### Initial Setup
```bash
# Clone repository
git clone <repo-url>
cd style-guide-generator

# Start Elasticsearch
docker-compose up -d elasticsearch

# Configure environment
cp .env.example .env
# Edit .env with your LLM API key

# Build and run
./mvnw quarkus:dev
```

## Notes
- This is a living document and will be updated as the project evolves
- Priority is on creating accurate, reusable style guides
- Focus on clean, maintainable code
- Emphasize proper error handling and logging
- Regular testing with real-world text samples is crucial
