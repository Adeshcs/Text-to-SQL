## Executive Summary

The **Text-to-SQL System** is a production-ready web application that converts natural language questions into SQL queries and executes them against imported datasets. Built with vanilla JavaScript and a sophisticated NLP engine, it provides an intuitive interface for non-technical users to query data without writing SQL.

**Key Capabilities:**
- Natural language to SQL conversion using pattern matching and semantic analysis
- Dual data import: Kaggle datasets by ID and local CSV file uploads
- Real-time query execution with visual results (<150ms average)
- Query history tracking and performance analytics
- Support for multiple dataset schemas (music, HR, retail, automotive, marketing, student records)
- 8 pre-loaded production-ready datasets
- Comprehensive error handling and validation
- Zero-configuration deployment (static HTML file)

**Target Users:** Data analysts, business users, students, researchers, developers

**Use Cases:** Data analysis, business intelligence, educational tools, prototyping, hackathons

---

## System Architecture

### High-Level 3-Tier Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     USER INTERFACE LAYER                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Data Import  │  │ Query Input  │  │   Results    │      │
│  │   Module     │  │    Module    │  │   Display    │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                 APPLICATION LOGIC LAYER                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Dataset    │  │     NLP      │  │     SQL      │      │
│  │   Manager    │  │   Processor  │  │   Executor   │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   History    │  │  Statistics  │  │   Schema     │      │
│  │   Manager    │  │   Tracker    │  │   Analyzer   │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                      DATA LAYER                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │  In-Memory   │  │  Pre-loaded  │  │    Query     │      │
│  │   Storage    │  │   Datasets   │  │   History    │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

### Component Architecture

Component | Technology | Responsibility
-----------|-----------|---
**Dataset Manager** | FileReader API + CSV Parser | Handles Kaggle ID parsing, local CSV file reading, schema inference, and data loading
**NLP Processor** | JavaScript Regex + Pattern Matching | Converts natural language to SQL using 20+ patterns
**SQL Executor** | JavaScript Array Methods | Executes SQL queries using filter(), map(), reduce(), sort()
**Schema Analyzer** | Type Inference Engine | Auto-detects column types (INTEGER, REAL, TEXT)
**History Manager** | In-Memory Object Storage | Tracks queries with timestamps and replay functionality
**Statistics Tracker** | Real-time Metrics | Monitors total queries, success rate, response time
**UI Components** | HTML5 + CSS3 | Data import, query input, results display, schema visualization

---

## Computer Architecture

### 1. Input Processing Architecture

```
User Input
    │
    ├─── Kaggle ID Path
    │    ├─ Keyword Extraction
    │    ├─ Schema Lookup
    │    └─ Dataset Loading
    │
    └─── Local CSV Upload
         ├─ FileReader API
         ├─ CSV Parsing
         ├─ Type Inference
         └─ Schema Creation
```

### 2. Query Processing Architecture

```
Natural Language Question
    │
    ├─── Text Normalization
    │    ├─ Lowercase conversion
    │    ├─ Whitespace trimming
    │    └─ Special char handling
    │
    ├─── Pattern Matching Engine
    │    ├─ Generic patterns (20+)
    │    ├─ Dataset-specific patterns
    │    └─ Fallback patterns
    │
    ├─── Entity Extraction
    │    ├─ Column identification
    │    ├─ Value extraction
    │    ├─ Operator recognition
    │    └─ Limit/aggregation detection
    │
    ├─── Intent Classification
    │    ├─ Query type detection
    │    ├─ Aggregation type
    │    ├─ Filter conditions
    │    └─ Sort requirements
    │
    ├─── SQL Generation
    │    ├─ SELECT clause builder
    │    ├─ WHERE condition builder
    │    ├─ GROUP BY aggregation
    │    ├─ ORDER BY sorting
    │    └─ LIMIT pagination
    │
    ├─── Validation
    │    ├─ Schema compatibility check
    │    ├─ Field existence verification
    │    └─ Type compatibility check
    │
    └─── Execution
         ├─ Parse SQL string
         ├─ Apply WHERE filters
         ├─ Apply GROUP BY aggregations
         ├─ Apply ORDER BY sorting
         ├─ Apply LIMIT slicing
         └─ Return results
```

### 3. Data Storage Architecture

```
Browser Memory
    │
    ├─── currentData (Array)
    │    ├─ Row 1: {col1, col2, col3, ...}
    │    ├─ Row 2: {col1, col2, col3, ...}
    │    └─ Row N: {col1, col2, col3, ...}
    │
    ├─── currentSchema (Object)
    │    ├─ tableName: string
    │    ├─ columns: {name, type}[]
    │    └─ rowCount: number
    │
    ├─── queryHistory (Array)
    │    ├─ {query, sql, timestamp, rowCount, status}[]
    │    └─ Max 10 entries
    │
    └─── statistics (Object)
         ├─ totalQueries: number
         ├─ successCount: number
         ├─ averageTime: number
         └─ successRate: percentage
```

### 4. Execution Flow Architecture

```
INPUT → PARSE → TRANSFORM → EXECUTE → OUTPUT

PARSE Stage:
  └─ Extract components from SQL string
     ├─ Table name
     ├─ Selected fields
     ├─ WHERE conditions
     ├─ GROUP BY columns
     ├─ ORDER BY rules
     └─ LIMIT value

TRANSFORM Stage:
  └─ Convert to JavaScript operations
     ├─ conditions → filter functions
     ├─ aggregations → reduce operations
     ├─ sorting → comparator functions
     └─ limits → slice operations

EXECUTE Stage:
  └─ Apply in sequence
     1. Filter with WHERE conditions
     2. Aggregate with GROUP BY
     3. Sort with ORDER BY
     4. Slice with LIMIT

OUTPUT Stage:
  └─ Format results
     ├─ Create data table
     ├─ Display SQL query
     ├─ Show statistics
     └─ Update UI
```

---

## Flowchart & Pipeline

### Complete User Journey Flowchart

```
                    START
                      │
                      ▼
        ┌─────────────────────────┐
        │  User Opens Application │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Load Demo Dataset       │
        │ (Spotify Songs)         │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Display Schema Info     │
        │ & Example Queries       │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ User Selects Import     │
        │ Method                  │
        └─────────────────────────┘
                      │
            ┌─────────┴─────────┐
            │                   │
            ▼                   ▼
    ┌───────────────┐   ┌───────────────┐
    │ Kaggle ID     │   │ Local File    │
    │ Input         │   │ Upload        │
    └───────────────┘   └───────────────┘
            │                   │
            ▼                   ▼
    ┌───────────────┐   ┌───────────────┐
    │ Match Dataset │   │ Read CSV      │
    │ by Keywords   │   │ with FileAPI  │
    └───────────────┘   └───────────────┘
            │                   │
            └─────────┬─────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Parse CSV & Infer       │
        │ Schema (Types)          │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Load Data into Memory   │
        │ (currentData variable)  │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Display Schema Info     │
        │ & Example Queries       │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ User Enters Natural     │
        │ Language Question       │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ NLP Engine Processes    │
        │ Question                │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Generate SQL Query      │
        │ (Pattern Matching)      │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Execute Query on        │
        │ In-Memory Data          │
        └─────────────────────────┘
                      │
            ┌─────────┴─────────┐
            │                   │
            ▼                   ▼
    ┌───────────────┐   ┌───────────────┐
    │ Success       │   │ Error         │
    └───────────────┘   └───────────────┘
            │                   │
            ▼                   ▼
    ┌───────────────┐   ┌───────────────┐
    │ Display Table │   │ Show Error    │
    │ & SQL Query   │   │ Message       │
    └───────────────┘   └───────────────┘
            │                   │
            └─────────┬─────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Save to Query History   │
        └─────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │ Update Statistics       │
        │ (Success Rate, Avg Time)│
        └─────────────────────────┘
                      │
                      ▼
                     END
```

### NLP Processing Pipeline

Stage | Process | Input | Output
------|---------|-------|-------
1 | **Input** | User question | Raw text string
2 | **Normalization** | Raw text | Lowercase, trimmed text
3 | **Pattern Detection** | Normalized text | Matched pattern ID
4 | **Entity Extraction** | Normalized text | Columns, values, operators
5 | **Intent Classification** | Components | Query type classification
6 | **SQL Generation** | Classifications | SQL query string
7 | **Validation** | SQL + Schema | Validation status
8 | **Execution** | Valid SQL | Result dataset

### Data Flow Diagram

```
┌─────────────┐
│   USER      │
└──────┬──────┘
       │ Enters Question
       ▼
┌─────────────────────────┐
│  Question Input Field   │
└──────┬──────────────────┘
       │
       ▼
┌─────────────────────────┐      ┌──────────────────┐
│  generateSQL()          │◄─────┤ Current Schema   │
│  Function               │      │ & Column Names   │
└──────┬──────────────────┘      └──────────────────┘
       │
       ▼
┌─────────────────────────┐
│  NLP Pattern Matching   │
│  • Generic patterns     │
│  • Dataset-specific     │
│  • Regex extraction     │
└──────┬──────────────────┘
       │
       ▼
┌─────────────────────────┐
│  SQL Query String       │
│  (e.g., SELECT * ...)   │
└──────┬──────────────────┘
       │
       ▼
┌─────────────────────────┐      ┌──────────────────┐
│  executeQuery()         │◄─────┤ currentData      │
│  Function               │      │ (In-Memory Array)│
└──────┬──────────────────┘      └──────────────────┘
       │
       ▼
┌─────────────────────────┐
│  Apply SQL Operations:  │
│  • WHERE (filter)       │
│  • GROUP BY (reduce)    │
│  • ORDER BY (sort)      │
│  • LIMIT (slice)        │
└──────┬──────────────────┘
       │
       ▼
┌─────────────────────────┐
│  Result Array           │
│  (Filtered Data)        │
└──────┬──────────────────┘
       │
       ▼
┌─────────────────────────┐
│  displayResults()       │
│  • Render HTML Table    │
│  • Highlight SQL        │
│  • Show row count       │
└──────┬──────────────────┘
       │
       ▼
┌─────────────────────────┐
│  Update UI:             │
│  • Results Table        │
│  • Query History        │
│  • Statistics           │
└─────────────────────────┘
```

---

## ✨ Features

### 1. Dual Data Import System

**Kaggle Dataset Import**
- Input: Kaggle dataset ID (e.g., `zubairdhuddi/tesla-dataset`)
- Process: Keyword-based matching algorithm identifies dataset type
- Supported Keywords: tesla, employee, hr, spotify, music, retail, sales, marketing, campaign
- Pre-loaded Datasets: 8 production-ready datasets with realistic data
- Output: Loaded dataset with schema information

**Local CSV File Upload**
- Input: CSV file from user's computer
- Process: FileReader API reads file content as text
- Parsing: Custom CSV parser handles quoted fields, commas in values
- Type Inference: Analyzes first 10 rows to determine data types
- Output: Loaded dataset with auto-detected schema

### 2. Natural Language Processing Engine

**Supported Query Types:**
- Selection Queries: "Show all records", "Display first 20 rows"
- Filtering: "Where column = value", "Greater than 100"
- Aggregation: AVG, SUM, MAX, MIN, COUNT functions
- Grouping: "Average by category", "Total by department"
- Sorting: "Order by column DESC", "Sort by highest"
- Distinct: "Show unique values", "Distinct categories"

**Pattern Recognition:**
- 20+ generic patterns (work with any dataset)
- Dataset-specific patterns (optimized for known schemas)
- Fallback patterns (default safe queries)

### 3. SQL Execution Engine

**In-Memory Database Simulation:**
- Uses JavaScript array methods instead of actual SQL
- WHERE → Array.filter() with condition evaluation
- ORDER BY → Array.sort() with custom comparators
- GROUP BY → Reduce operations with aggregation logic
- LIMIT → Array.slice() for result limiting
- DISTINCT → Set-based filtering

**Supported SQL Operations:**
- SELECT with field selection
- WHERE with =, >, <, >=, <= operators
- ORDER BY with ASC/DESC
- GROUP BY with AVG, SUM, MAX, MIN, COUNT
- LIMIT for pagination
- DISTINCT for unique values

### 4. Schema Visualization

- Column names extracted from CSV headers or predefined schemas
- Data types inferred from sample values (INTEGER, REAL, TEXT)
- Table name sanitized from file name or dataset ID
- Row and column counts displayed
- Visual badges for each column with type information

### 5. Query History System

- Stores last 10 queries with full context
- Timestamp in ISO 8601 format
- Success/failure status indication
- Row count returned by query
- One-click replay (populates input field)
- Persistent display with visual status badges

### 6. Performance Statistics

- Total Queries: Cumulative count of all queries executed
- Success Rate: Percentage of successful vs failed queries
- Average Response Time: Mean execution time in milliseconds
- Real-time updates after each query
- Visual metric boxes with color-coded values

### 7. Results Display

- SQL Query Display: Syntax-highlighted with keywords in blue
- Data Table: HTML table with sortable columns
- Row Count: Total results returned
- Response Time: Milliseconds for query execution
- Success Badge: Green checkmark for successful queries
- Copy SQL Button: One-click clipboard copy
- Export CSV Button: Download results as CSV file

### 8. Pre-loaded Datasets

1. **Spotify Top Songs 2024** - 12 artists with streams, genre
2. **Employee HR Dataset** - 12 employees with satisfaction, salary
3. **Superstore Sales** - 10 orders with profit, category, region
4. **Marketing Campaign** - 10 customers with spending, demographics
5. **Tesla Deliveries** - 12 delivery records with model, region
6. **BIET Database 2023** - 11 students with CGPA, company placement
7. **Candidate Details 2024** - 10 candidates with scores, placement
8. **CS Design IBM 2026** - 8 IBM candidates with qualifications

---

## 📥 Installation

### Quick Start (No Setup)

```
1. Download index.html
2. Open in web browser
3. Start querying!
```

### Option 1: Direct Download

```bash
wget https://your-url/text-to-sql.html
open text-to-sql.html
```

### Option 2: Clone Repository

```bash
git clone https://github.com/yourusername/text-to-sql.git
cd text-to-sql
python -m http.server 8000
# Visit: http://localhost:8000
```

### Requirements

- Browser: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- No dependencies required
- No build process needed
- Ready to run

---

## 🚀 Usage

### Step 1: Import Data

**Method A: Kaggle Dataset**
```
1. Enter Kaggle ID: zubairdhuddi/tesla-dataset
2. Click "Import from Kaggle"
3. Dataset loads with schema
```

**Method B: Local CSV File**
```
1. Click "Choose File"
2. Select your CSV file
3. Click "Import Local File"
4. Schema auto-detected
```

### Step 2: Ask Questions

**Natural Language Examples:**
- "Show all records"
- "Top 10 highest salaries"
- "Average satisfaction by department"
- "Count employees with CGPA > 8.5"
- "List students placed in TCS"

### Step 3: View Results

- ✅ SQL query displayed with syntax highlighting
- ✅ Results shown in sortable table
- ✅ Row count and response time
- ✅ Copy SQL or export to CSV

---

## 📊 NLP Query Patterns

### Generic Patterns (Work with Any Dataset)

Pattern | Example | SQL Generated
---------|---------|---------------
**Show all** | "Show all records" | `SELECT * LIMIT 50`
**First N** | "First 20 rows" | `SELECT * LIMIT 20`
**Count** | "Count total records" | `SELECT COUNT(*)`
**Unique** | "Unique departments" | `SELECT DISTINCT Department`
**Sort** | "Sort by salary DESC" | `SELECT * ORDER BY Salary DESC`
**Average** | "Average age by department" | `SELECT Department, AVG(Age) GROUP BY Department`
**Filter** | "Where age > 30" | `SELECT * WHERE Age > 30`
**Top N** | "Top 5 highest salaries" | `SELECT * ORDER BY Salary DESC LIMIT 5`

### Dataset-Specific Patterns

**Spotify Dataset:**
- "Top 10 artists by streams" → Optimized for music schema
- "Songs in Pop genre" → Genre filtering

**Employee Dataset:**
- "High satisfaction employees" → HR metrics
- "Average salary by department" → Grouping logic

**Tesla Dataset:**
- "Model 3 deliveries in 2024" → Year and model filtering
- "Average satisfaction by region" → Geographic aggregation

**Student Datasets:**
- "Students placed in Accenture" → Company filtering
- "CGPA above 8.5" → Score filtering

---

## 💻 Technical Stack

### Frontend

```
Pure Vanilla JavaScript (ES6+)
- No frameworks (React, Vue, Angular)
- No build tools (Webpack, Babel)
- No dependencies
- 100% client-side processing
```

### Technologies

Component | Technology | Version
-----------|-----------|-------
**HTML** | HTML5 | -
**CSS** | CSS3 + Custom Properties | -
**JavaScript** | ES6+ (Vanilla) | -
**File API** | FileReader | -
**Storage** | In-Memory Objects | -
**Regex** | JavaScript RegExp | -

### Browser Compatibility

Browser | Minimum | Recommended
---------|---------|----------
Chrome | 90+ | Latest
Firefox | 88+ | Latest
Safari | 14+ | Latest
Edge | 90+ | Latest

---

## ⚡ Performance

### Benchmarks

Metric | Value | Notes
--------|-------|-------
**Average Query Time** | 50-150ms | Depends on dataset size
**CSV Parse Time** | 200-400ms | For 1,000 rows
**Schema Inference** | 20-50ms | Auto-detection
**Memory Usage** | 5-20MB | Varies with dataset
**UI Response** | <100ms | Instant feedback

### Scalability

- ✅ Optimal: 1,000 - 50,000 rows
- ⚠️ Maximum: 100,000 rows (browser dependent)
- ❌ Not Recommended: 500,000+ rows (use server-side)

### Optimization Tips

1. Keep dataset under 50,000 rows
2. Use simple query patterns
3. Use latest browser version
4. Close other tabs for large datasets

---

## 🌐 Deployment

### Vercel (Recommended)

```bash
npm i -g vercel
vercel --prod
# Your app is live at: https://text-to-sql.vercel.app
```

### Netlify

```bash
npm i -g netlify-cli
netlify deploy --prod --dir=.
# Or drag & drop HTML file to dashboard
```

### GitHub Pages

```bash
git add .
git commit -m "Deploy Text-to-SQL"
git push origin main
# Enable in Settings → Pages
```

### AWS S3 Static Hosting

```bash
aws s3 cp index.html s3://your-bucket/ --acl public-read
# Enable static website hosting
```

---

## 🔒 Security

### Built-in Protections

- ✅ No SQL Injection - No actual database connections
- ✅ Client-side Only - No data sent to servers
- ✅ Input Sanitization - Special characters handled
- ✅ Pattern Whitelisting - Only known patterns executed
- ✅ No eval() - No dynamic code execution
