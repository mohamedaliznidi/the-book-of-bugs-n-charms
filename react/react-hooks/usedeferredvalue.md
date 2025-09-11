---
description: >-
  Complete grimoire for the useDeferredValue enchantment - master the ancient art of
  deferred state updates in React enchantments, enabling smooth user experiences
  through intelligent value deferral and performance optimization magic.

theme: "magic"
---

# useDeferredValue - The Value Deferral Enchantment

## The Ancient Knowledge

The `useDeferredValue` enchantment is a React hook that allows you to defer updates to a value, enabling React to prioritize more urgent updates first. This mystical spell helps maintain responsive user interfaces by delaying expensive computations while keeping critical interactions immediate, creating smoother user experiences through intelligent value prioritization.

## When to Cast This Spell

1. **Search Input Optimization**: Defer search query processing while keeping input field responsive for immediate typing feedback
2. **Expensive Computations**: Delay heavy calculations that depend on frequently changing values
3. **List Filtering and Sorting**: Defer expensive list operations while maintaining UI responsiveness
4. **Real-time Data Processing**: Handle high-frequency data updates without blocking the main thread
5. **Debouncing Alternative**: Provide React-native debouncing behavior for expensive operations

## Your First Casting

Here's a basic example of using `useDeferredValue` for search optimization:

```jsx
import React, { useState, useDeferredValue, useMemo } from 'react';

function SearchableList({ items }) {
  const [searchTerm, setSearchTerm] = useState('');
  const deferredSearchTerm = useDeferredValue(searchTerm);

  // Expensive filtering operation uses deferred value
  const filteredItems = useMemo(() => {
    if (!deferredSearchTerm) return items;
    
    return items.filter(item =>
      item.name.toLowerCase().includes(deferredSearchTerm.toLowerCase()) ||
      item.description.toLowerCase().includes(deferredSearchTerm.toLowerCase())
    );
  }, [items, deferredSearchTerm]);

  const isStale = searchTerm !== deferredSearchTerm;

  return (
    <div className="searchable-list">
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search mystical items..."
        className="search-input"
      />
      
      <div className={`results-container ${isStale ? 'updating' : ''}`}>
        {isStale && (
          <div className="stale-indicator">
            🔮 Updating search results...
          </div>
        )}
        
        {filteredItems.map(item => (
          <div key={item.id} className="result-item">
            <h3>{item.name}</h3>
            <p>{item.description}</p>
          </div>
        ))}
      </div>
    </div>
  );
}
```

## Advanced Sorcery

### Complex Data Visualization with Deferred Updates

```jsx
import React, { useState, useDeferredValue, useMemo } from 'react';

function DataVisualization({ rawData }) {
  const [filters, setFilters] = useState({
    category: '',
    dateRange: 'all',
    minValue: 0,
    maxValue: 1000
  });
  
  const deferredFilters = useDeferredValue(filters);

  // Expensive data processing with deferred filters
  const processedData = useMemo(() => {
    console.log('Processing data with filters:', deferredFilters);
    
    return rawData
      .filter(item => {
        if (deferredFilters.category && item.category !== deferredFilters.category) {
          return false;
        }
        if (item.value < deferredFilters.minValue || item.value > deferredFilters.maxValue) {
          return false;
        }
        return true;
      })
      .map(item => ({
        ...item,
        processedValue: expensiveCalculation(item.value),
        trend: calculateTrend(item.history)
      }));
  }, [rawData, deferredFilters]);

  const isUpdating = filters !== deferredFilters;

  const updateFilter = (key, value) => {
    setFilters(prev => ({ ...prev, [key]: value }));
  };

  return (
    <div className="data-visualization">
      <div className="filter-panel">
        <select 
          value={filters.category}
          onChange={(e) => updateFilter('category', e.target.value)}
        >
          <option value="">All Categories</option>
          <option value="spells">Spells</option>
          <option value="potions">Potions</option>
          <option value="artifacts">Artifacts</option>
        </select>
        
        <input
          type="range"
          min="0"
          max="1000"
          value={filters.minValue}
          onChange={(e) => updateFilter('minValue', parseInt(e.target.value))}
        />
        
        <input
          type="range"
          min="0"
          max="1000"
          value={filters.maxValue}
          onChange={(e) => updateFilter('maxValue', parseInt(e.target.value))}
        />
      </div>
      
      <div className={`chart-container ${isUpdating ? 'updating' : ''}`}>
        {isUpdating && (
          <div className="update-overlay">
            📊 Recalculating visualization...
          </div>
        )}
        <Chart data={processedData} />
      </div>
    </div>
  );
}

function expensiveCalculation(value) {
  // Simulate expensive computation
  let result = value;
  for (let i = 0; i < 10000; i++) {
    result = Math.sin(result) * Math.cos(result);
  }
  return result;
}
```

### Real-time Data Processing

```jsx
import React, { useState, useDeferredValue, useMemo, useEffect } from 'react';

function RealTimeMonitor() {
  const [liveData, setLiveData] = useState([]);
  const [processingIntensity, setProcessingIntensity] = useState('medium');
  
  const deferredData = useDeferredValue(liveData);
  const deferredIntensity = useDeferredValue(processingIntensity);

  // Simulate real-time data updates
  useEffect(() => {
    const interval = setInterval(() => {
      setLiveData(prev => [
        ...prev.slice(-99), // Keep last 100 items
        {
          id: Date.now(),
          value: Math.random() * 100,
          timestamp: new Date().toISOString()
        }
      ]);
    }, 100); // Update every 100ms

    return () => clearInterval(interval);
  }, []);

  // Expensive processing of deferred data
  const processedMetrics = useMemo(() => {
    if (deferredData.length === 0) return null;

    const intensity = deferredIntensity;
    const iterations = intensity === 'low' ? 100 : intensity === 'medium' ? 1000 : 10000;

    return {
      average: deferredData.reduce((sum, item) => sum + item.value, 0) / deferredData.length,
      trend: calculateComplexTrend(deferredData, iterations),
      volatility: calculateVolatility(deferredData, iterations),
      predictions: generatePredictions(deferredData, iterations)
    };
  }, [deferredData, deferredIntensity]);

  const isProcessing = liveData !== deferredData || processingIntensity !== deferredIntensity;

  return (
    <div className="realtime-monitor">
      <div className="controls">
        <label>
          Processing Intensity:
          <select 
            value={processingIntensity}
            onChange={(e) => setProcessingIntensity(e.target.value)}
          >
            <option value="low">Low</option>
            <option value="medium">Medium</option>
            <option value="high">High</option>
          </select>
        </label>
        
        {isProcessing && (
          <div className="processing-indicator">
            ⚡ Processing real-time data...
          </div>
        )}
      </div>
      
      <div className="metrics-display">
        <div className="live-data-count">
          Live Data Points: {liveData.length}
        </div>
        
        {processedMetrics && (
          <div className={`processed-metrics ${isProcessing ? 'stale' : ''}`}>
            <div>Average: {processedMetrics.average.toFixed(2)}</div>
            <div>Trend: {processedMetrics.trend}</div>
            <div>Volatility: {processedMetrics.volatility.toFixed(2)}</div>
          </div>
        )}
      </div>
      
      <div className="data-visualization">
        <LiveChart data={liveData} />
        {processedMetrics && (
          <ProcessedChart 
            data={processedMetrics} 
            isStale={isProcessing}
          />
        )}
      </div>
    </div>
  );
}
```

### Integration with useTransition

```jsx
import React, { useState, useDeferredValue, useTransition } from 'react';

function CombinedOptimization() {
  const [query, setQuery] = useState('');
  const [sortBy, setSortBy] = useState('relevance');
  const [isPending, startTransition] = useTransition();
  
  // Defer the search query for expensive filtering
  const deferredQuery = useDeferredValue(query);

  const handleQueryChange = (newQuery) => {
    setQuery(newQuery); // Immediate update for input responsiveness
  };

  const handleSortChange = (newSort) => {
    // Use transition for sort changes
    startTransition(() => {
      setSortBy(newSort);
    });
  };

  const searchResults = useMemo(() => {
    return performExpensiveSearch(deferredQuery, sortBy);
  }, [deferredQuery, sortBy]);

  const isQueryStale = query !== deferredQuery;

  return (
    <div className="combined-optimization">
      <div className="search-controls">
        <input
          type="text"
          value={query}
          onChange={(e) => handleQueryChange(e.target.value)}
          placeholder="Search..."
        />
        
        <select 
          value={sortBy}
          onChange={(e) => handleSortChange(e.target.value)}
          disabled={isPending}
        >
          <option value="relevance">Relevance</option>
          <option value="date">Date</option>
          <option value="popularity">Popularity</option>
        </select>
      </div>
      
      <div className="status-indicators">
        {isQueryStale && (
          <div className="query-indicator">🔍 Updating search...</div>
        )}
        {isPending && (
          <div className="sort-indicator">📊 Resorting results...</div>
        )}
      </div>
      
      <div className="results">
        {searchResults.map(result => (
          <SearchResult key={result.id} result={result} />
        ))}
      </div>
    </div>
  );
}
```

## Performance Optimization Patterns

### Memoization with Deferred Values

```jsx
import React, { useState, useDeferredValue, useMemo, useCallback } from 'react';

function OptimizedDataProcessor({ data }) {
  const [filters, setFilters] = useState({});
  const [transformations, setTransformations] = useState([]);

  const deferredFilters = useDeferredValue(filters);
  const deferredTransformations = useDeferredValue(transformations);

  // Memoize expensive operations with deferred values
  const filteredData = useMemo(() => {
    console.log('Filtering data...');
    return data.filter(item => {
      return Object.entries(deferredFilters).every(([key, value]) => {
        if (!value) return true;
        return item[key] === value;
      });
    });
  }, [data, deferredFilters]);

  const transformedData = useMemo(() => {
    console.log('Transforming data...');
    return deferredTransformations.reduce((acc, transformation) => {
      return applyTransformation(acc, transformation);
    }, filteredData);
  }, [filteredData, deferredTransformations]);

  // Memoize callbacks to prevent unnecessary re-renders
  const updateFilter = useCallback((key, value) => {
    setFilters(prev => ({ ...prev, [key]: value }));
  }, []);

  const addTransformation = useCallback((transformation) => {
    setTransformations(prev => [...prev, transformation]);
  }, []);

  const isStale = filters !== deferredFilters ||
                  transformations !== deferredTransformations;

  return (
    <div className="optimized-processor">
      <FilterPanel onFilterChange={updateFilter} />
      <TransformationPanel onAddTransformation={addTransformation} />

      <div className={`results ${isStale ? 'updating' : ''}`}>
        {isStale && <div className="stale-indicator">🔄 Processing...</div>}
        <DataDisplay data={transformedData} />
      </div>
    </div>
  );
}
```

### Conditional Deferral Based on Performance

```jsx
import React, { useState, useDeferredValue, useMemo } from 'react';

function AdaptivePerformanceComponent({ data, performanceMode = 'auto' }) {
  const [searchTerm, setSearchTerm] = useState('');

  // Conditionally defer based on data size or performance mode
  const shouldDefer = performanceMode === 'auto'
    ? data.length > 1000
    : performanceMode === 'performance';

  const deferredSearchTerm = useDeferredValue(searchTerm);
  const effectiveSearchTerm = shouldDefer ? deferredSearchTerm : searchTerm;

  const filteredData = useMemo(() => {
    if (!effectiveSearchTerm) return data;

    const startTime = performance.now();
    const result = data.filter(item =>
      item.name.toLowerCase().includes(effectiveSearchTerm.toLowerCase())
    );
    const endTime = performance.now();

    console.log(`Filtering took ${endTime - startTime} milliseconds`);
    return result;
  }, [data, effectiveSearchTerm]);

  const isDeferred = shouldDefer && searchTerm !== deferredSearchTerm;

  return (
    <div className="adaptive-performance">
      <div className="performance-info">
        <div>Data size: {data.length} items</div>
        <div>Performance mode: {performanceMode}</div>
        <div>Deferral active: {shouldDefer ? 'Yes' : 'No'}</div>
      </div>

      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />

      {isDeferred && (
        <div className="deferred-indicator">
          ⏳ Deferring expensive search operation...
        </div>
      )}

      <div className="results">
        {filteredData.map(item => (
          <div key={item.id}>{item.name}</div>
        ))}
      </div>
    </div>
  );
}
```

## Integration with Suspense

```jsx
import React, { useState, useDeferredValue, Suspense } from 'react';

function SuspenseIntegration() {
  const [resourceId, setResourceId] = useState(1);
  const deferredResourceId = useDeferredValue(resourceId);

  return (
    <div className="suspense-integration">
      <div className="resource-selector">
        {[1, 2, 3, 4, 5].map(id => (
          <button
            key={id}
            onClick={() => setResourceId(id)}
            className={resourceId === id ? 'active' : ''}
          >
            Resource {id}
          </button>
        ))}
      </div>

      {resourceId !== deferredResourceId && (
        <div className="transition-indicator">
          🔄 Loading new resource...
        </div>
      )}

      <Suspense fallback={<div>Loading resource...</div>}>
        <ExpensiveResource resourceId={deferredResourceId} />
      </Suspense>
    </div>
  );
}

function ExpensiveResource({ resourceId }) {
  // This component might suspend while loading
  const resource = useResource(resourceId);

  return (
    <div className="resource-content">
      <h2>Resource {resourceId}</h2>
      <pre>{JSON.stringify(resource, null, 2)}</pre>
    </div>
  );
}
```

## Wisdom of the Deferral Ancients

- **Use for Expensive Computations**: Defer values that trigger expensive calculations or rendering
- **Combine with useMemo**: Always pair with useMemo to prevent unnecessary recalculations
- **Monitor Staleness**: Check if current value differs from deferred value to show loading states
- **Consider Data Size**: More beneficial with larger datasets or complex computations
- **Test Performance Impact**: Measure actual performance improvements on target devices
- **Avoid Over-Deferring**: Not every value needs deferral - use judiciously for expensive operations

## Common Curses & Their Remedies

### Curse 1: Forgetting to Use useMemo with Deferred Values
Without memoization, expensive computations still run on every render.

**Counter-Spell:**
```jsx
// ❌ Deferred value without memoization
function BadExample({ data }) {
  const [search, setSearch] = useState('');
  const deferredSearch = useDeferredValue(search);

  // This still runs on every render!
  const filtered = data.filter(item =>
    item.name.includes(deferredSearch)
  );

  return <div>{filtered.length} results</div>;
}

// ✅ Proper memoization with deferred value
function GoodExample({ data }) {
  const [search, setSearch] = useState('');
  const deferredSearch = useDeferredValue(search);

  const filtered = useMemo(() =>
    data.filter(item => item.name.includes(deferredSearch)),
    [data, deferredSearch]
  );

  return <div>{filtered.length} results</div>;
}
```

### Curse 2: Not Providing Visual Feedback for Stale Values
Users should know when displayed data is outdated.

**Banishment Ritual:**
```jsx
// ✅ Always indicate when values are stale
function SearchComponent() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  const isStale = query !== deferredQuery;

  return (
    <div>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      {isStale && <div className="stale-indicator">Updating...</div>}
      <Results query={deferredQuery} />
    </div>
  );
}
```

### Curse 3: Deferring Values That Should Be Immediate
Some updates need immediate visual feedback and shouldn't be deferred.

**Protective Ward:**
```jsx
// ❌ Don't defer form input values
const [inputValue, setInputValue] = useState('');
const deferredInput = useDeferredValue(inputValue); // Bad!

// ✅ Defer expensive computations, not input values
const [inputValue, setInputValue] = useState('');
const deferredInput = useDeferredValue(inputValue);

return (
  <div>
    <input
      value={inputValue} // Immediate feedback
      onChange={e => setInputValue(e.target.value)}
    />
    <ExpensiveComponent searchTerm={deferredInput} />
  </div>
);
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useDeferredValue" %}

{% embed url="https://react.dev/learn/keeping-components-pure" %}

{% embed url="https://github.com/reactwg/react-18/discussions" %}
```
