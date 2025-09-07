# Graph-based-Recommender-System-with-Duckdb-Analytics-Engine

## Project Overview

This project implements a **Graph-based recommender system** designed to provide personalized product recommendations for e-commerce platforms. The system analyzes user behavior patterns from their purchase and search history to recommend the most relevant products using graph-based algorithms and co-occurrence analysis.

## Project Architecture

<img width="3840" height="3522" alt="ProjectArchitecture (2)" src="https://github.com/user-attachments/assets/d1020e47-0469-4769-91bb-c94ad5be9659" />

## Tech Stack

- **Language**: Python
- **Core Libraries**: DuckDB, pandas, NumPy, NetworkX, Matplotlib
- **Data Storage**: Parquet format for optimized file management
- **Database Operations**: SQL querying with DuckDB
- **Graph Processing**: NetworkX for graph construction and analysis

## Dataset Overview

The dataset contains comprehensive user interaction data from an e-commerce platform with **55+ million records** spanning January 2020, featuring:

- **9 core attributes** per record
- **Event types**: view, cart, purchase
- **User sessions**: 13M+ unique sessions analyzed
- **Products**: 226K+ distinct products
- **Users**: 4.3M+ unique users

### Data Schema

| Field | Type | Description |
|-------|------|-------------|
| event_time | datetime | Timestamp of user action |
| event_type | string | Type of event (view/cart/purchase) |
| product_id | int32 | Unique product identifier |
| category_id | int64 | Product category identifier |
| category_code | string | Human-readable category |
| brand | string | Product brand |
| price | float32 | Product price |
| user_id | int32 | Unique user identifier |
| user_session | string | Session identifier |

## Project Structure & Implementation

### 1. Data Optimization (`1. Data Optimisation.ipynb`)

**Purpose**: Efficiently process and optimize large-scale e-commerce data for graph construction.

**Key Achievements**:
- **Memory Optimization**: Reduced memory usage by ~30% through intelligent dtype conversion
- **Performance Boost**: Converted compressed CSV (2.2GB) to optimized Parquet format (2.0GB)
- **Processing Speed**: Improved data loading from 2+ minutes to 54 seconds
- **Data Integrity**: Maintained full data fidelity while optimizing storage

**Technical Implementation**:
```python
# Memory-optimized data types
dtype_optimization = {
    'product_id': 'int32', 
    'user_id': 'int32', 
    'event_type': 'category', 
    'price': 'float32'
}
```

**Results**:
- Original CSV loading time: 2 minutes 4 seconds
- Optimized Parquet loading time: 54 seconds
- Memory reduction: 3.8GB → 2.8GB

### 2. Data Exploration & Analysis (`2. Data Exploration and Data Analysis.ipynb`)

**Purpose**: Conduct comprehensive exploratory data analysis using high-performance analytics.

**Key Insights Discovered**:

#### User Behavior Patterns
- **Event Distribution**: 
  - Views: 52.49M events (93.8%)
  - Cart additions: 2.64M events (4.7%) 
  - Purchases: 835K events (1.5%)

#### Session Analytics
- **Average Products per Session**: 2.6 distinct product views
- **Active Sessions**: 5.34M sessions with multiple product interactions
- **User Engagement**: High cross-product exploration within sessions

#### Technical Performance
- **Query Speed**: Complex analytics completed in seconds using DuckDB
- **Scalability**: Efficient processing of 55M+ records
- **Data Quality**: Zero null values in critical fields (user_session, user_id, product_id)

### 3. Graph Construction (`3. GraphConstruct.ipynb` & `GraphConstruct.py`)

**Purpose**: Build sophisticated graph structures to capture product relationships based on user co-viewing patterns.

#### Graph Construction Methodology

**Session-based Edge Creation**:
```sql
SELECT product_id,
       LEAD(product_id, 1, -1) OVER (PARTITION BY user_session ORDER BY event_time) as next_viewed_product_id
FROM product_view_tbl
```

#### Three Graph Variants Generated

##### 1. Raw Unweighted Directed Graph
- **Edges**: 25.89M directed edges
- **Structure**: Direct product transition sequences
- **Use Case**: Sequential recommendation patterns

##### 2. Directed Weighted Graph  
- **Edges**: 8.42M unique directed edges
- **Weights**: Co-occurrence frequency counts
- **Structure**: Asymmetric product relationships
- **Use Case**: Directional influence analysis

##### 3. Undirected Weighted Graph
- **Edges**: 6.85M unique undirected edges  
- **Weights**: Combined co-occurrence frequencies
- **Structure**: Symmetric product relationships
- **Use Case**: Mutual similarity recommendations

#### Graph Construction Performance
- **Processing Time**: 5.91 minutes for complete graph generation
- **Data Filtering**: Excluded page refreshes (12.18M self-loops)
- **Session Filtering**: Analyzed only multi-product sessions (5.34M sessions)

## Key Features & Capabilities

### 1. **Multi-Graph Architecture**
- Support for directed and undirected graph representations
- Weighted edges based on co-occurrence frequency
- Flexible graph querying for different recommendation scenarios

### 2. **Scalable Data Processing**
- Optimized for large-scale e-commerce datasets
- Memory-efficient data handling
- High-performance SQL-based graph construction

### 3. **Session-Based Analysis**
- Captures temporal user behavior patterns
- Identifies product transition sequences
- Eliminates noise from page refreshes

### 4. **Comprehensive Analytics**
- Detailed user behavior insights
- Product relationship strength quantification
- Performance metrics and processing statistics

## Results & Performance Metrics

### Data Processing Efficiency
| Operation | Dataset Size | Processing Time | Memory Usage |
|-----------|--------------|-----------------|--------------|
| Data Loading (Optimized) | 55M+ records | 54 seconds | 2.8GB |
| Graph Construction | 43M+ transitions | 5.91 minutes | N/A |
| Analytics Queries | 55M+ records | < 2 seconds | N/A |

### Graph Statistics
| Graph Type | Nodes | Edges | Avg. Weight | Max Weight |
|------------|-------|-------|-------------|------------|
| Directed Weighted | 226K+ | 8.42M | Variable | 478+ |
| Undirected Weighted | 226K+ | 6.85M | Variable | Combined |
| Raw Directed | 226K+ | 25.89M | 1 | 1 |

### User Behavior Insights
- **Cross-category Exploration**: Users frequently explore products across different categories within sessions
- **Brand Switching**: Significant transitions between different brands in the same category
- **Price Sensitivity**: Co-viewing patterns show price-based product comparisons

## Technical Architecture

### Data Flow Pipeline
```
Raw E-commerce Data → Data Optimization → Exploratory Analysis → Graph Construction → Recommendation Engine
```

### Core Components
1. **Data Preprocessor**: Handles memory optimization and format conversion
2. **Analytics Engine**: DuckDB-powered fast analytics and insights generation  
3. **Graph Builder**: Constructs multiple graph representations from user sessions
4. **Recommendation Logic**: Graph-based algorithms for product suggestions

### Performance Optimizations
- **Columnar Storage**: Parquet format for optimal I/O performance
- **In-Memory Analytics**: DuckDB for lightning-fast query processing
- **Efficient Graph Storage**: Optimized edge list representations
- **Memory Management**: Strategic dtype optimization for large datasets

## Implementation Highlights

### Advanced SQL Techniques
- **Window Functions**: LEAD/LAG for sequential analysis
- **Session Aggregation**: Complex multi-level grouping operations
- **Graph Edge Creation**: SQL-based graph construction logic

### Graph Processing Innovations
- **Multi-perspective Graphs**: Different graph types for various recommendation scenarios
- **Weight Normalization**: Co-occurrence frequency as edge weights
- **Noise Reduction**: Intelligent filtering of non-meaningful transitions

### Performance Engineering
- **Lazy Loading**: Efficient data access patterns
- **Batch Processing**: Optimized bulk operations
- **Memory Footprint**: Minimized memory usage through data type optimization

## Potential Applications

### E-commerce Recommendations
- **Product Suggestions**: "Customers who viewed this also viewed..."
- **Cross-selling**: Complementary product recommendations
- **Category Exploration**: Guided product discovery

### Business Intelligence
- **Market Basket Analysis**: Understanding product relationship patterns
- **User Journey Mapping**: Visualizing customer navigation patterns
- **Inventory Optimization**: Data-driven product placement strategies

### Advanced Analytics
- **Similarity Scoring**: Quantifying product relationships
- **Trend Detection**: Identifying emerging product associations
- **Personalization**: User-specific recommendation fine-tuning

## Future Enhancements

### Algorithm Extensions
- **Graph Neural Networks**: Deep learning on graph structures
- **Community Detection**: Product clustering and category optimization
- **Temporal Dynamics**: Time-aware recommendation algorithms

### Scalability Improvements
- **Distributed Processing**: Spark/Dask integration for massive datasets
- **Real-time Updates**: Streaming graph updates from live user data
- **Cloud Deployment**: Scalable cloud-based recommendation infrastructure

### Advanced Features
- **Multi-modal Graphs**: Integration of product content, images, and descriptions
- **Social Graphs**: User-user relationships and collaborative filtering
- **Context-aware Recommendations**: Location, time, and device-based suggestions

---

*This graph-based recommender system demonstrates the power of combining traditional data analytics with modern graph algorithms to create sophisticated, scalable recommendation engines for e-commerce platforms.*
