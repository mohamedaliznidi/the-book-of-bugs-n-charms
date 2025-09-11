---
description: >-
  Complete grimoire for the useTransition enchantment - master the ancient art of
  concurrent rendering in React enchantments, enabling smooth user experiences
  through non-blocking state updates and performance optimization magic.

theme: "magic"
---

# useTransition - The Concurrent Rendering Enchantment

## The Ancient Knowledge

The `useTransition` enchantment is a powerful React hook that enables concurrent rendering features, allowing you to mark state updates as non-urgent transitions. This mystical spell helps maintain responsive user interfaces by preventing blocking updates from freezing the UI, creating smoother user experiences through intelligent priority management of state changes.

## When to Cast This Spell

1. **Non-Urgent State Updates**: Mark expensive state updates as transitions to prevent UI blocking during heavy computations
2. **Search and Filtering Operations**: Defer search results updates while keeping input responsive for immediate user feedback
3. **Tab Switching Interfaces**: Smooth transitions between different views without blocking user interactions
4. **Data Visualization Updates**: Update charts and graphs without freezing the interface during complex rendering
5. **List Rendering Optimization**: Handle large list updates while maintaining scroll and interaction responsiveness

## Your First Casting

Here's a basic example of using `useTransition` for a search interface:

```jsx
import React, { useState, useTransition } from 'react';

function SearchInterface() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  const handleSearch = (searchTerm) => {
    // Immediate update - keeps input responsive
    setQuery(searchTerm);
    
    // Transition update - non-blocking search results
    startTransition(() => {
      const filteredResults = performExpensiveSearch(searchTerm);
      setResults(filteredResults);
    });
  };

  return (
    <div className="search-container">
      <input
        type="text"
        value={query}
        onChange={(e) => handleSearch(e.target.value)}
        placeholder="Search mystical spells..."
        className="search-input"
      />
      
      {isPending && (
        <div className="loading-indicator">
          🔮 Searching through ancient grimoires...
        </div>
      )}
      
      <div className="results-container">
        {results.map(result => (
          <div key={result.id} className="result-item">
            {result.title}
          </div>
        ))}
      </div>
    </div>
  );
}

function performExpensiveSearch(term) {
  // Simulate expensive search operation
  const allSpells = generateLargeDataset();
  return allSpells.filter(spell => 
    spell.title.toLowerCase().includes(term.toLowerCase())
  );
}
```

## Advanced Sorcery

### Tab Switching with Smooth Transitions

```jsx
import React, { useState, useTransition } from 'react';

function TabInterface() {
  const [activeTab, setActiveTab] = useState('spells');
  const [isPending, startTransition] = useTransition();

  const tabs = [
    { id: 'spells', label: 'Mystical Spells', component: SpellsTab },
    { id: 'potions', label: 'Magical Potions', component: PotionsTab },
    { id: 'artifacts', label: 'Ancient Artifacts', component: ArtifactsTab }
  ];

  const handleTabChange = (tabId) => {
    startTransition(() => {
      setActiveTab(tabId);
    });
  };

  const ActiveComponent = tabs.find(tab => tab.id === activeTab)?.component;

  return (
    <div className="tab-interface">
      <div className="tab-buttons">
        {tabs.map(tab => (
          <button
            key={tab.id}
            onClick={() => handleTabChange(tab.id)}
            className={`tab-button ${activeTab === tab.id ? 'active' : ''}`}
            disabled={isPending}
          >
            {tab.label}
            {isPending && activeTab === tab.id && ' 🔄'}
          </button>
        ))}
      </div>
      
      <div className={`tab-content ${isPending ? 'transitioning' : ''}`}>
        {ActiveComponent && <ActiveComponent />}
      </div>
    </div>
  );
}
```

### Data Visualization with Smooth Updates

```jsx
import React, { useState, useTransition, useMemo } from 'react';

function DataVisualization() {
  const [dataset, setDataset] = useState('sales');
  const [filters, setFilters] = useState({});
  const [isPending, startTransition] = useTransition();

  const processedData = useMemo(() => {
    return processLargeDataset(dataset, filters);
  }, [dataset, filters]);

  const updateFilters = (newFilters) => {
    startTransition(() => {
      setFilters(prev => ({ ...prev, ...newFilters }));
    });
  };

  const switchDataset = (newDataset) => {
    startTransition(() => {
      setDataset(newDataset);
      setFilters({}); // Reset filters for new dataset
    });
  };

  return (
    <div className="data-visualization">
      <div className="controls">
        <select 
          onChange={(e) => switchDataset(e.target.value)}
          disabled={isPending}
        >
          <option value="sales">Sales Data</option>
          <option value="users">User Analytics</option>
          <option value="performance">Performance Metrics</option>
        </select>
        
        <FilterControls 
          onFiltersChange={updateFilters}
          disabled={isPending}
        />
      </div>
      
      <div className={`chart-container ${isPending ? 'updating' : ''}`}>
        {isPending && (
          <div className="chart-overlay">
            <div className="loading-spinner">📊 Updating visualization...</div>
          </div>
        )}
        <Chart data={processedData} />
      </div>
    </div>
  );
}
```

### List Rendering with Concurrent Updates

```jsx
import React, { useState, useTransition, useDeferredValue } from 'react';

function LargeList() {
  const [searchTerm, setSearchTerm] = useState('');
  const [sortBy, setSortBy] = useState('name');
  const [isPending, startTransition] = useTransition();
  
  // Defer the search term to prevent blocking
  const deferredSearchTerm = useDeferredValue(searchTerm);

  const handleSearch = (term) => {
    setSearchTerm(term); // Immediate update for input
  };

  const handleSort = (sortOption) => {
    startTransition(() => {
      setSortBy(sortOption);
    });
  };

  const filteredAndSortedItems = useMemo(() => {
    let items = largeItemsList.filter(item =>
      item.name.toLowerCase().includes(deferredSearchTerm.toLowerCase())
    );
    
    return items.sort((a, b) => {
      switch (sortBy) {
        case 'name':
          return a.name.localeCompare(b.name);
        case 'date':
          return new Date(b.date) - new Date(a.date);
        case 'popularity':
          return b.popularity - a.popularity;
        default:
          return 0;
      }
    });
  }, [deferredSearchTerm, sortBy]);

  return (
    <div className="large-list">
      <div className="list-controls">
        <input
          type="text"
          value={searchTerm}
          onChange={(e) => handleSearch(e.target.value)}
          placeholder="Search items..."
          className="search-input"
        />
        
        <select 
          value={sortBy}
          onChange={(e) => handleSort(e.target.value)}
          disabled={isPending}
        >
          <option value="name">Sort by Name</option>
          <option value="date">Sort by Date</option>
          <option value="popularity">Sort by Popularity</option>
        </select>
        
        {isPending && <span className="sorting-indicator">⚡ Sorting...</span>}
      </div>
      
      <div className="items-container">
        {filteredAndSortedItems.map(item => (
          <ListItem key={item.id} item={item} />
        ))}
      </div>
    </div>
  );
}
```

## Integration with Suspense

```jsx
import React, { useState, useTransition, Suspense } from 'react';

function SuspenseIntegration() {
  const [resourceId, setResourceId] = useState(1);
  const [isPending, startTransition] = useTransition();

  const loadResource = (id) => {
    startTransition(() => {
      setResourceId(id);
    });
  };

  return (
    <div className="suspense-integration">
      <div className="resource-buttons">
        {[1, 2, 3, 4, 5].map(id => (
          <button
            key={id}
            onClick={() => loadResource(id)}
            disabled={isPending}
            className={resourceId === id ? 'active' : ''}
          >
            Resource {id}
            {isPending && resourceId === id && ' 🔄'}
          </button>
        ))}
      </div>
      
      <Suspense fallback={<div>Loading resource...</div>}>
        <ResourceComponent resourceId={resourceId} />
      </Suspense>
    </div>
  );
}
```

## Performance Optimization Techniques

### Combining with useMemo and useCallback

```jsx
import React, { useState, useTransition, useMemo, useCallback } from 'react';

function OptimizedComponent() {
  const [data, setData] = useState([]);
  const [filters, setFilters] = useState({});
  const [isPending, startTransition] = useTransition();

  // Memoize expensive computations
  const processedData = useMemo(() => {
    return data.filter(item => {
      return Object.entries(filters).every(([key, value]) => {
        if (!value) return true;
        return item[key]?.toString().toLowerCase().includes(value.toLowerCase());
      });
    });
  }, [data, filters]);

  // Memoize transition callbacks
  const updateFilter = useCallback((key, value) => {
    startTransition(() => {
      setFilters(prev => ({ ...prev, [key]: value }));
    });
  }, [startTransition]);

  const resetFilters = useCallback(() => {
    startTransition(() => {
      setFilters({});
    });
  }, [startTransition]);

  return (
    <div className="optimized-component">
      <FilterPanel
        onFilterChange={updateFilter}
        onReset={resetFilters}
        disabled={isPending}
      />

      <div className={`results ${isPending ? 'updating' : ''}`}>
        {processedData.map(item => (
          <ResultItem key={item.id} item={item} />
        ))}
      </div>
    </div>
  );
}
```

### Batch Multiple Transitions

```jsx
function BatchedTransitions() {
  const [state1, setState1] = useState('');
  const [state2, setState2] = useState('');
  const [state3, setState3] = useState('');
  const [isPending, startTransition] = useTransition();

  const updateAllStates = (newValues) => {
    startTransition(() => {
      // Batch multiple state updates in a single transition
      setState1(newValues.value1);
      setState2(newValues.value2);
      setState3(newValues.value3);
    });
  };

  return (
    <div className="batched-transitions">
      <button
        onClick={() => updateAllStates({
          value1: 'New Value 1',
          value2: 'New Value 2',
          value3: 'New Value 3'
        })}
        disabled={isPending}
      >
        Update All States {isPending && '⏳'}
      </button>

      <div className="state-display">
        <div>State 1: {state1}</div>
        <div>State 2: {state2}</div>
        <div>State 3: {state3}</div>
      </div>
    </div>
  );
}
```

## Wisdom of the Transition Ancients

- **Use for Non-Urgent Updates**: Only wrap state updates that don't need immediate visual feedback
- **Combine with useDeferredValue**: Use together for optimal performance in search and filtering scenarios
- **Monitor isPending State**: Provide visual feedback during transitions to improve user experience
- **Avoid Overuse**: Not every state update needs to be a transition - use judiciously for expensive operations
- **Test on Slower Devices**: Transitions are most beneficial on devices with limited processing power
- **Consider User Expectations**: Some updates should be immediate (form inputs) while others can be deferred (search results)

## Common Curses & Their Remedies

### Curse 1: Overusing Transitions for Immediate Updates
Wrapping every state update in a transition can make the UI feel sluggish.

**Counter-Spell:**
```jsx
// ❌ Don't wrap immediate UI updates
const handleInputChange = (value) => {
  startTransition(() => {
    setInputValue(value); // This should be immediate
  });
};

// ✅ Keep immediate updates outside transitions
const handleInputChange = (value) => {
  setInputValue(value); // Immediate
  startTransition(() => {
    setSearchResults(performSearch(value)); // Deferred
  });
};
```

### Curse 2: Not Providing Visual Feedback During Transitions
Users need to know when expensive operations are happening.

**Banishment Ritual:**
```jsx
// ✅ Always show loading state during transitions
function SearchComponent() {
  const [isPending, startTransition] = useTransition();

  return (
    <div>
      <input onChange={handleSearch} />
      {isPending && <LoadingSpinner />}
      <Results />
    </div>
  );
}
```

### Curse 3: Mixing Urgent and Non-Urgent Updates in Same Transition
This can cause unexpected behavior and poor user experience.

**Protective Ward:**
```jsx
// ❌ Don't mix urgent and non-urgent updates
const handleAction = () => {
  startTransition(() => {
    setButtonState('clicked'); // Should be immediate
    setExpensiveData(computeData()); // Can be deferred
  });
};

// ✅ Separate immediate and deferred updates
const handleAction = () => {
  setButtonState('clicked'); // Immediate feedback
  startTransition(() => {
    setExpensiveData(computeData()); // Deferred computation
  });
};
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useTransition" %}

{% embed url="https://react.dev/learn/keeping-components-pure" %}

{% embed url="https://github.com/reactwg/react-18/discussions" %}
```
