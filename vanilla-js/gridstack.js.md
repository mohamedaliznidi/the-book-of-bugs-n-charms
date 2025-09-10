---
description: >-
  GridStack.js is a modern Typescript library for building interactive dashboards
  with drag-and-drop, resize, and responsive grid layouts.

theme: "magic"
---

# The GridStack.js Interactive Dashboard Sorcery Grimoire

## The Ancient Knowledge

GridStack.js is a modern TypeScript library for building interactive dashboards through grid manipulation enchantments. It provides drag-and-drop functionality, resizable widgets, and responsive grid layouts through spatial organization magic. Perfect for creating admin dashboards, portfolio layouts, and any application requiring dynamic grid-based interfaces with mystical layout powers.

## When to Cast These Spells

1. **Admin Dashboard Rituals**: Create customizable dashboards where users can rearrange widgets through interface manipulation magic
2. **Portfolio Layout Enchantments**: Build responsive portfolio grids with drag-and-drop functionality using presentation sorcery
3. **Content Management Sorcery**: Allow users to organize content blocks dynamically through content arrangement spells
4. **Analytics Dashboard Mysticism**: Create interactive data visualization layouts with dynamic grid enchantments
5. **Widget-based Application Magic**: Build applications with moveable and resizable components through flexible interface spells

## Summoning the Grid Spirits

### CDN Invocation

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gridstack@9.2.0/dist/gridstack.min.css" />
<script src="https://cdn.jsdelivr.net/npm/gridstack@9.2.0/dist/gridstack-all.js"></script>
```

### NPM Ritual

```bash
npm install gridstack
```

### Yarn Enchantment

```bash
yarn add gridstack
```

## Basic Grid Setup Rituals

### HTML Sacred Structure

```html
<!DOCTYPE html>
<html>
<head>
    <title>GridStack Example</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gridstack@9.2.0/dist/gridstack.min.css" />
</head>
<body>
    <div class="grid-stack">
        <div class="grid-stack-item" gs-w="4" gs-h="2">
            <div class="grid-stack-item-content">Widget 1</div>
        </div>
        <div class="grid-stack-item" gs-w="4" gs-h="4">
            <div class="grid-stack-item-content">Widget 2</div>
        </div>
        <div class="grid-stack-item" gs-w="4" gs-h="2">
            <div class="grid-stack-item-content">Widget 3</div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/gridstack@9.2.0/dist/gridstack-all.js"></script>
    <script src="script.js"></script>
</body>
</html>
```

### Basic JavaScript Initialization Spells

```javascript
// Initialize GridStack
const grid = GridStack.init({
    cellHeight: 80,
    verticalMargin: 10,
    horizontalMargin: 10,
    minRow: 1,
    maxRow: 12,
    animate: true,
    float: false,
    removable: '.trash',
    acceptWidgets: true
});

// Add event listeners
grid.on('change', function(event, items) {
    console.log('Grid changed:', items);
});

grid.on('added', function(event, items) {
    console.log('Items added:', items);
});

grid.on('removed', function(event, items) {
    console.log('Items removed:', items);
});
```

## Configuration Option Enchantments

### Grid Option Mysticism

```javascript
const gridOptions = {
    // Grid dimensions
    cellHeight: 80,           // Height of each cell in pixels
    verticalMargin: 10,       // Vertical margin between items
    horizontalMargin: 10,     // Horizontal margin between items
    minRow: 1,               // Minimum number of rows
    maxRow: 0,               // Maximum rows (0 = unlimited)
    column: 12,              // Number of columns

    // Behavior
    animate: true,           // Animate item movements
    float: false,            // Allow items to float up
    removable: '.trash',     // CSS selector for remove zone
    acceptWidgets: true,     // Accept widgets from other grids
    alwaysShowResizeHandle: false, // Always show resize handles

    // Responsive
    disableOneColumnMode: false, // Disable one-column mode on mobile
    oneColumnModeDomSort: false, // Sort DOM in one-column mode

    // Drag & Drop
    dragIn: '.newWidget',    // CSS selector for draggable items
    dragInOptions: {
        revert: 'invalid',
        scroll: false,
        appendTo: 'body',
        helper: 'clone'
    },

    // Resize
    resizable: {
        handles: 'e, se, s, sw, w'
    },

    // Static grid (no interactions)
    staticGrid: false
};

const grid = GridStack.init(gridOptions);
```

## Dynamic Widget Management Sorcery

### Adding Widget Programmatic Conjurations

```javascript
class DashboardManager {
    constructor(containerId) {
        this.grid = GridStack.init({
            cellHeight: 80,
            verticalMargin: 10,
            animate: true
        }, containerId);

        this.widgetCounter = 0;
        this.setupEventListeners();
    }

    addWidget(content, options = {}) {
        const defaultOptions = {
            w: 4,
            h: 2,
            autoPosition: true,
            id: `widget-${++this.widgetCounter}`
        };

        const widgetOptions = { ...defaultOptions, ...options };

        const widgetElement = this.grid.addWidget(`
            <div class="grid-stack-item-content">
                <div class="widget-header">
                    <span class="widget-title">${content.title || 'Widget'}</span>
                    <button class="widget-close" onclick="dashboard.removeWidget('${widgetOptions.id}')">×</button>
                </div>
                <div class="widget-body">
                    ${content.body || 'Widget content'}
                </div>
            </div>
        `, widgetOptions);

        return widgetElement;
    }

    removeWidget(widgetId) {
        const widget = document.getElementById(widgetId);
        if (widget) {
            this.grid.removeWidget(widget);
        }
    }

    updateWidget(widgetId, newContent) {
        const widget = document.getElementById(widgetId);
        if (widget) {
            const contentDiv = widget.querySelector('.widget-body');
            if (contentDiv) {
                contentDiv.innerHTML = newContent;
            }
        }
    }

    setupEventListeners() {
        this.grid.on('change', (event, items) => {
            this.saveLayout();
        });

        this.grid.on('resizestop', (event, element) => {
            // Handle resize completion
            this.onWidgetResize(element);
        });
    }

    saveLayout() {
        const layout = this.grid.save();
        localStorage.setItem('dashboard-layout', JSON.stringify(layout));
    }

    loadLayout() {
        const savedLayout = localStorage.getItem('dashboard-layout');
        if (savedLayout) {
            this.grid.load(JSON.parse(savedLayout));
        }
    }

    onWidgetResize(element) {
        // Trigger any necessary updates when widget is resized
        const event = new CustomEvent('widgetResized', {
            detail: { element, grid: this.grid }
        });
        element.dispatchEvent(event);
    }
}

// Usage
const dashboard = new DashboardManager('.grid-stack');

// Add different types of widgets
dashboard.addWidget({
    title: 'Sales Chart',
    body: '<canvas id="sales-chart"></canvas>'
}, { w: 6, h: 4 });

dashboard.addWidget({
    title: 'User Stats',
    body: '<div class="stats">1,234 active users</div>'
}, { w: 3, h: 2 });
```

### Widget Template Manifestations

```javascript
class WidgetTemplates {
    static chart(title, chartId) {
        return {
            title: title,
            body: `
                <div class="chart-container">
                    <canvas id="${chartId}"></canvas>
                </div>
            `,
            className: 'chart-widget'
        };
    }

    static metric(title, value, unit = '') {
        return {
            title: title,
            body: `
                <div class="metric-widget">
                    <div class="metric-value">${value}</div>
                    <div class="metric-unit">${unit}</div>
                </div>
            `,
            className: 'metric-widget'
        };
    }

    static list(title, items) {
        const listItems = items.map(item => `<li>${item}</li>`).join('');
        return {
            title: title,
            body: `
                <div class="list-widget">
                    <ul>${listItems}</ul>
                </div>
            `,
            className: 'list-widget'
        };
    }

    static iframe(title, src) {
        return {
            title: title,
            body: `
                <div class="iframe-widget">
                    <iframe src="${src}" frameborder="0"></iframe>
                </div>
            `,
            className: 'iframe-widget'
        };
    }

    static custom(title, htmlContent) {
        return {
            title: title,
            body: htmlContent,
            className: 'custom-widget'
        };
    }
}

// Usage
dashboard.addWidget(WidgetTemplates.chart('Revenue Chart', 'revenue-chart'), { w: 8, h: 6 });
dashboard.addWidget(WidgetTemplates.metric('Total Sales', '$125,430'), { w: 4, h: 3 });
dashboard.addWidget(WidgetTemplates.list('Recent Orders', ['Order #1234', 'Order #1235', 'Order #1236']), { w: 4, h: 4 });
```

## Responsive Design Adaptation Spells

### Mobile-First Responsive Grid Enchantments

```javascript
const responsiveGrid = GridStack.init({
    cellHeight: 80,
    verticalMargin: 10,
    // Responsive breakpoints
    column: 12,
    minRow: 1,
    maxRow: 0,

    // Mobile settings
    disableOneColumnMode: false,
    oneColumnModeDomSort: true,

    // Responsive column settings
    columnOpts: {
        breakpoints: [
            { w: 768, c: 1 },   // Mobile: 1 column
            { w: 992, c: 6 },   // Tablet: 6 columns
            { w: 1200, c: 12 }  // Desktop: 12 columns
        ]
    }
});

// Handle responsive changes
window.addEventListener('resize', debounce(() => {
    responsiveGrid.column(getColumnCount());
}, 250));

function getColumnCount() {
    const width = window.innerWidth;
    if (width < 768) return 1;
    if (width < 992) return 6;
    return 12;
}

function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}
```

### Responsive Widget Sizing Alchemy

```javascript
class ResponsiveWidgetManager {
    constructor(containerId) {
        this.grid = GridStack.init({
            cellHeight: 80,
            animate: true
        }, containerId);

        this.setupResponsiveHandling();
    }

    setupResponsiveHandling() {
        const mediaQueries = [
            { query: '(max-width: 768px)', columns: 1 },
            { query: '(min-width: 769px) and (max-width: 1024px)', columns: 6 },
            { query: '(min-width: 1025px)', columns: 12 }
        ];

        mediaQueries.forEach(({ query, columns }) => {
            const mq = window.matchMedia(query);
            mq.addListener(() => {
                if (mq.matches) {
                    this.grid.column(columns);
                    this.adjustWidgetSizes(columns);
                }
            });

            // Check initial state
            if (mq.matches) {
                this.grid.column(columns);
                this.adjustWidgetSizes(columns);
            }
        });
    }

    adjustWidgetSizes(columns) {
        const widgets = this.grid.getGridItems();

        widgets.forEach(widget => {
            const currentW = parseInt(widget.getAttribute('gs-w'));

            if (columns === 1) {
                // Mobile: full width
                this.grid.update(widget, { w: 1 });
            } else if (columns === 6) {
                // Tablet: adjust proportionally
                const newW = Math.min(currentW, 6);
                this.grid.update(widget, { w: newW });
            } else {
                // Desktop: restore original size or use current
                const originalW = widget.dataset.originalW || currentW;
                this.grid.update(widget, { w: parseInt(originalW) });
            }
        });
    }

    addResponsiveWidget(content, sizes = {}) {
        const defaultSizes = {
            mobile: { w: 1, h: 2 },
            tablet: { w: 3, h: 2 },
            desktop: { w: 4, h: 2 }
        };

        const widgetSizes = { ...defaultSizes, ...sizes };
        const currentColumns = this.grid.getColumn();

        let initialSize;
        if (currentColumns === 1) {
            initialSize = widgetSizes.mobile;
        } else if (currentColumns === 6) {
            initialSize = widgetSizes.tablet;
        } else {
            initialSize = widgetSizes.desktop;
        }

        const widget = this.grid.addWidget(content, {
            ...initialSize,
            autoPosition: true
        });

        // Store responsive sizes as data attributes
        widget.dataset.mobileSizes = JSON.stringify(widgetSizes.mobile);
        widget.dataset.tabletSizes = JSON.stringify(widgetSizes.tablet);
        widget.dataset.desktopSizes = JSON.stringify(widgetSizes.desktop);

        return widget;
    }
}

## Drag and Drop from External Source Mysticism

### External Widget Palette Conjurations

```html
<div class="widget-palette">
    <div class="palette-item" data-widget-type="chart">
        <i class="icon-chart"></i>
        Chart Widget
    </div>
    <div class="palette-item" data-widget-type="metric">
        <i class="icon-metric"></i>
        Metric Widget
    </div>
    <div class="palette-item" data-widget-type="table">
        <i class="icon-table"></i>
        Table Widget
    </div>
</div>

<div class="grid-stack"></div>
```

```javascript
class DragDropManager {
    constructor(gridSelector, paletteSelector) {
        this.grid = GridStack.init({
            cellHeight: 80,
            verticalMargin: 10,
            acceptWidgets: true,
            dragIn: '.palette-item',
            dragInOptions: {
                revert: 'invalid',
                scroll: false,
                appendTo: 'body',
                helper: 'clone',
                zIndex: 1000
            }
        }, gridSelector);

        this.setupPalette(paletteSelector);
        this.setupGridEvents();
    }

    setupPalette(paletteSelector) {
        const paletteItems = document.querySelectorAll(`${paletteSelector} .palette-item`);

        paletteItems.forEach(item => {
            item.draggable = true;
            item.addEventListener('dragstart', (e) => {
                const widgetType = item.dataset.widgetType;
                e.dataTransfer.setData('text/plain', widgetType);
                e.dataTransfer.effectAllowed = 'copy';
            });
        });
    }

    setupGridEvents() {
        this.grid.on('dropped', (event, previousWidget, newWidget) => {
            const widgetType = previousWidget.dataset.widgetType;
            this.createWidgetFromType(newWidget, widgetType);
        });

        this.grid.on('dragstart', (event, element) => {
            element.classList.add('dragging');
        });

        this.grid.on('dragstop', (event, element) => {
            element.classList.remove('dragging');
        });
    }

    createWidgetFromType(element, type) {
        let content = '';

        switch (type) {
            case 'chart':
                content = this.createChartWidget();
                break;
            case 'metric':
                content = this.createMetricWidget();
                break;
            case 'table':
                content = this.createTableWidget();
                break;
            default:
                content = '<div>Unknown widget type</div>';
        }

        element.querySelector('.grid-stack-item-content').innerHTML = content;
        element.dataset.widgetType = type;
    }

    createChartWidget() {
        return `
            <div class="widget-header">
                <span class="widget-title">Chart Widget</span>
                <div class="widget-controls">
                    <button class="widget-config">⚙️</button>
                    <button class="widget-close">×</button>
                </div>
            </div>
            <div class="widget-body">
                <canvas class="chart-canvas"></canvas>
            </div>
        `;
    }

    createMetricWidget() {
        return `
            <div class="widget-header">
                <span class="widget-title">Metric Widget</span>
                <div class="widget-controls">
                    <button class="widget-config">⚙️</button>
                    <button class="widget-close">×</button>
                </div>
            </div>
            <div class="widget-body metric-display">
                <div class="metric-value">0</div>
                <div class="metric-label">Metric</div>
            </div>
        `;
    }

    createTableWidget() {
        return `
            <div class="widget-header">
                <span class="widget-title">Table Widget</span>
                <div class="widget-controls">
                    <button class="widget-config">⚙️</button>
                    <button class="widget-close">×</button>
                </div>
            </div>
            <div class="widget-body">
                <table class="data-table">
                    <thead>
                        <tr><th>Column 1</th><th>Column 2</th></tr>
                    </thead>
                    <tbody>
                        <tr><td>Data 1</td><td>Data 2</td></tr>
                    </tbody>
                </table>
            </div>
        `;
    }
}

// Initialize drag and drop
const dragDropManager = new DragDropManager('.grid-stack', '.widget-palette');
```

## Serialization and Persistence Alchemy

### Save and Load Grid State Preservation Spells

```javascript
class GridStateManager {
    constructor(gridSelector, storageKey = 'gridstack-layout') {
        this.grid = GridStack.init({
            cellHeight: 80,
            verticalMargin: 10,
            animate: true
        }, gridSelector);

        this.storageKey = storageKey;
        this.setupAutoSave();
        this.loadState();
    }

    saveState() {
        const serializedData = this.grid.save();
        const gridState = {
            layout: serializedData,
            timestamp: new Date().toISOString(),
            version: '1.0'
        };

        try {
            localStorage.setItem(this.storageKey, JSON.stringify(gridState));
            console.log('Grid state saved successfully');
        } catch (error) {
            console.error('Failed to save grid state:', error);
        }
    }

    loadState() {
        try {
            const savedState = localStorage.getItem(this.storageKey);
            if (savedState) {
                const gridState = JSON.parse(savedState);
                this.grid.load(gridState.layout);
                console.log('Grid state loaded successfully');
            }
        } catch (error) {
            console.error('Failed to load grid state:', error);
        }
    }

    exportState() {
        const serializedData = this.grid.save();
        const exportData = {
            layout: serializedData,
            metadata: {
                exportDate: new Date().toISOString(),
                version: '1.0',
                gridOptions: {
                    cellHeight: this.grid.getCellHeight(),
                    column: this.grid.getColumn()
                }
            }
        };

        const dataStr = JSON.stringify(exportData, null, 2);
        const dataBlob = new Blob([dataStr], { type: 'application/json' });

        const link = document.createElement('a');
        link.href = URL.createObjectURL(dataBlob);
        link.download = `gridstack-layout-${new Date().toISOString().split('T')[0]}.json`;
        link.click();
    }

    importState(file) {
        const reader = new FileReader();
        reader.onload = (e) => {
            try {
                const importData = JSON.parse(e.target.result);
                this.grid.removeAll();
                this.grid.load(importData.layout);
                this.saveState();
                console.log('Grid state imported successfully');
            } catch (error) {
                console.error('Failed to import grid state:', error);
                alert('Invalid file format');
            }
        };
        reader.readAsText(file);
    }

    setupAutoSave() {
        let saveTimeout;

        this.grid.on('change', () => {
            clearTimeout(saveTimeout);
            saveTimeout = setTimeout(() => {
                this.saveState();
            }, 1000); // Auto-save after 1 second of inactivity
        });
    }

    resetGrid() {
        if (confirm('Are you sure you want to reset the grid? This action cannot be undone.')) {
            this.grid.removeAll();
            localStorage.removeItem(this.storageKey);
            console.log('Grid reset successfully');
        }
    }
}

// Usage
const stateManager = new GridStateManager('.grid-stack');

// Add export/import buttons
document.getElementById('export-btn').addEventListener('click', () => {
    stateManager.exportState();
});

document.getElementById('import-btn').addEventListener('change', (e) => {
    const file = e.target.files[0];
    if (file) {
        stateManager.importState(file);
    }
});

document.getElementById('reset-btn').addEventListener('click', () => {
    stateManager.resetGrid();
});
```

## Advanced Grid Feature Enchantments

### Multi-Grid Support Sorcery

```javascript
class MultiGridManager {
    constructor() {
        this.grids = new Map();
        this.setupGrids();
    }

    setupGrids() {
        // Main dashboard grid
        this.grids.set('main', GridStack.init({
            cellHeight: 80,
            verticalMargin: 10,
            acceptWidgets: '.grid-stack-item',
            dragOut: true
        }, '.main-grid'));

        // Secondary grid
        this.grids.set('secondary', GridStack.init({
            cellHeight: 60,
            verticalMargin: 8,
            acceptWidgets: '.grid-stack-item',
            dragOut: true
        }, '.secondary-grid'));

        // Setup inter-grid communication
        this.setupInterGridDragDrop();
    }

    setupInterGridDragDrop() {
        this.grids.forEach((grid, gridId) => {
            grid.on('dropped', (event, previousWidget, newWidget) => {
                console.log(`Widget moved to ${gridId} grid`);
                this.onWidgetMoved(gridId, newWidget);
            });

            grid.on('removed', (event, items) => {
                items.forEach(item => {
                    console.log(`Widget removed from ${gridId} grid`);
                });
            });
        });
    }

    onWidgetMoved(targetGridId, widget) {
        // Update widget styling based on target grid
        const targetGrid = this.grids.get(targetGridId);

        if (targetGridId === 'secondary') {
            widget.classList.add('secondary-grid-widget');
        } else {
            widget.classList.remove('secondary-grid-widget');
        }

        // Trigger any necessary updates
        this.updateWidgetForGrid(widget, targetGridId);
    }

    updateWidgetForGrid(widget, gridId) {
        const widgetType = widget.dataset.widgetType;

        // Adjust widget content based on grid
        if (gridId === 'secondary' && widgetType === 'chart') {
            // Make chart smaller for secondary grid
            const canvas = widget.querySelector('canvas');
            if (canvas) {
                canvas.style.height = '100px';
            }
        }
    }

    addWidgetToGrid(gridId, content, options = {}) {
        const grid = this.grids.get(gridId);
        if (grid) {
            return grid.addWidget(content, options);
        }
        return null;
    }

    moveWidgetBetweenGrids(widget, fromGridId, toGridId) {
        const fromGrid = this.grids.get(fromGridId);
        const toGrid = this.grids.get(toGridId);

        if (fromGrid && toGrid) {
            // Get widget data
            const widgetData = {
                content: widget.innerHTML,
                w: parseInt(widget.getAttribute('gs-w')),
                h: parseInt(widget.getAttribute('gs-h'))
            };

            // Remove from source grid
            fromGrid.removeWidget(widget);

            // Add to target grid
            const newWidget = toGrid.addWidget(widgetData.content, {
                w: widgetData.w,
                h: widgetData.h,
                autoPosition: true
            });

            this.onWidgetMoved(toGridId, newWidget);
            return newWidget;
        }
        return null;
    }
}

// Usage
const multiGridManager = new MultiGridManager();
```

### Custom Widget Types with Configuration Mysticism

```javascript
class ConfigurableWidget {
    constructor(grid, type, config = {}) {
        this.grid = grid;
        this.type = type;
        this.config = { ...this.getDefaultConfig(), ...config };
        this.element = null;
        this.id = `widget-${Date.now()}-${Math.random().toString(36).substr(2, 9)}`;
    }

    getDefaultConfig() {
        const defaults = {
            chart: {
                title: 'Chart Widget',
                chartType: 'line',
                dataSource: null,
                refreshInterval: 30000
            },
            metric: {
                title: 'Metric Widget',
                value: 0,
                unit: '',
                format: 'number',
                threshold: null
            },
            table: {
                title: 'Table Widget',
                columns: [],
                dataSource: null,
                pageSize: 10
            }
        };

        return defaults[this.type] || {};
    }

    create(options = {}) {
        const widgetContent = this.generateContent();

        this.element = this.grid.addWidget(widgetContent, {
            w: 4,
            h: 3,
            autoPosition: true,
            id: this.id,
            ...options
        });

        this.element.dataset.widgetType = this.type;
        this.element.dataset.widgetId = this.id;

        this.setupEventListeners();
        this.initialize();

        return this.element;
    }

    generateContent() {
        return `
            <div class="grid-stack-item-content configurable-widget">
                <div class="widget-header">
                    <span class="widget-title">${this.config.title}</span>
                    <div class="widget-controls">
                        <button class="widget-config" data-widget-id="${this.id}">⚙️</button>
                        <button class="widget-refresh" data-widget-id="${this.id}">🔄</button>
                        <button class="widget-close" data-widget-id="${this.id}">×</button>
                    </div>
                </div>
                <div class="widget-body" id="widget-body-${this.id}">
                    ${this.generateBodyContent()}
                </div>
            </div>
        `;
    }

    generateBodyContent() {
        switch (this.type) {
            case 'chart':
                return `<canvas id="chart-${this.id}"></canvas>`;
            case 'metric':
                return `
                    <div class="metric-display">
                        <div class="metric-value">${this.config.value}</div>
                        <div class="metric-unit">${this.config.unit}</div>
                    </div>
                `;
            case 'table':
                return `<div id="table-${this.id}" class="table-container"></div>`;
            default:
                return '<div>Unknown widget type</div>';
        }
    }

    setupEventListeners() {
        // Configuration button
        const configBtn = this.element.querySelector('.widget-config');
        configBtn.addEventListener('click', () => this.showConfigDialog());

        // Refresh button
        const refreshBtn = this.element.querySelector('.widget-refresh');
        refreshBtn.addEventListener('click', () => this.refresh());

        // Close button
        const closeBtn = this.element.querySelector('.widget-close');
        closeBtn.addEventListener('click', () => this.remove());
    }

    initialize() {
        switch (this.type) {
            case 'chart':
                this.initializeChart();
                break;
            case 'metric':
                this.initializeMetric();
                break;
            case 'table':
                this.initializeTable();
                break;
        }

        // Setup auto-refresh if configured
        if (this.config.refreshInterval) {
            this.setupAutoRefresh();
        }
    }

    initializeChart() {
        // Initialize chart based on config
        const canvas = document.getElementById(`chart-${this.id}`);
        if (canvas && this.config.dataSource) {
            // Chart initialization logic here
            console.log('Initializing chart widget:', this.id);
        }
    }

    initializeMetric() {
        this.updateMetricValue(this.config.value);
    }

    initializeTable() {
        // Initialize table based on config
        const container = document.getElementById(`table-${this.id}`);
        if (container && this.config.dataSource) {
            // Table initialization logic here
            console.log('Initializing table widget:', this.id);
        }
    }

    updateMetricValue(value) {
        const valueElement = this.element.querySelector('.metric-value');
        if (valueElement) {
            valueElement.textContent = this.formatValue(value);

            // Apply threshold styling if configured
            if (this.config.threshold) {
                this.applyThresholdStyling(value);
            }
        }
    }

    formatValue(value) {
        switch (this.config.format) {
            case 'currency':
                return new Intl.NumberFormat('en-US', {
                    style: 'currency',
                    currency: 'USD'
                }).format(value);
            case 'percentage':
                return `${value}%`;
            default:
                return value.toString();
        }
    }

    applyThresholdStyling(value) {
        const element = this.element.querySelector('.metric-value');
        element.classList.remove('threshold-low', 'threshold-high');

        if (value < this.config.threshold.low) {
            element.classList.add('threshold-low');
        } else if (value > this.config.threshold.high) {
            element.classList.add('threshold-high');
        }
    }

    showConfigDialog() {
        // Create and show configuration modal
        const modal = this.createConfigModal();
        document.body.appendChild(modal);
        modal.style.display = 'block';
    }

    createConfigModal() {
        const modal = document.createElement('div');
        modal.className = 'widget-config-modal';
        modal.innerHTML = `
            <div class="modal-content">
                <div class="modal-header">
                    <h3>Configure ${this.config.title}</h3>
                    <button class="modal-close">&times;</button>
                </div>
                <div class="modal-body">
                    ${this.generateConfigForm()}
                </div>
                <div class="modal-footer">
                    <button class="btn-cancel">Cancel</button>
                    <button class="btn-save">Save</button>
                </div>
            </div>
        `;

        // Setup modal event listeners
        modal.querySelector('.modal-close').addEventListener('click', () => {
            modal.remove();
        });

        modal.querySelector('.btn-cancel').addEventListener('click', () => {
            modal.remove();
        });

        modal.querySelector('.btn-save').addEventListener('click', () => {
            this.saveConfiguration(modal);
            modal.remove();
        });

        return modal;
    }

    generateConfigForm() {
        // Generate form based on widget type
        let formHTML = `
            <div class="form-group">
                <label>Title:</label>
                <input type="text" name="title" value="${this.config.title}">
            </div>
        `;

        switch (this.type) {
            case 'metric':
                formHTML += `
                    <div class="form-group">
                        <label>Unit:</label>
                        <input type="text" name="unit" value="${this.config.unit}">
                    </div>
                    <div class="form-group">
                        <label>Format:</label>
                        <select name="format">
                            <option value="number" ${this.config.format === 'number' ? 'selected' : ''}>Number</option>
                            <option value="currency" ${this.config.format === 'currency' ? 'selected' : ''}>Currency</option>
                            <option value="percentage" ${this.config.format === 'percentage' ? 'selected' : ''}>Percentage</option>
                        </select>
                    </div>
                `;
                break;
        }

        return formHTML;
    }

    saveConfiguration(modal) {
        const formData = new FormData(modal.querySelector('.modal-body'));
        const newConfig = {};

        for (let [key, value] of formData.entries()) {
            newConfig[key] = value;
        }

        this.updateConfig(newConfig);
    }

    updateConfig(newConfig) {
        this.config = { ...this.config, ...newConfig };

        // Update widget title
        const titleElement = this.element.querySelector('.widget-title');
        if (titleElement) {
            titleElement.textContent = this.config.title;
        }

        // Reinitialize widget with new config
        this.initialize();
    }

    setupAutoRefresh() {
        if (this.refreshTimer) {
            clearInterval(this.refreshTimer);
        }

        this.refreshTimer = setInterval(() => {
            this.refresh();
        }, this.config.refreshInterval);
    }

    refresh() {
        // Refresh widget data
        console.log(`Refreshing widget ${this.id}`);

        if (this.config.dataSource) {
            // Fetch new data and update widget
            this.fetchData().then(data => {
                this.updateWithNewData(data);
            });
        }
    }

    async fetchData() {
        if (typeof this.config.dataSource === 'string') {
            // URL data source
            const response = await fetch(this.config.dataSource);
            return response.json();
        } else if (typeof this.config.dataSource === 'function') {
            // Function data source
            return this.config.dataSource();
        }
        return null;
    }

    updateWithNewData(data) {
        switch (this.type) {
            case 'metric':
                if (typeof data === 'number') {
                    this.updateMetricValue(data);
                }
                break;
            // Add other widget type updates
        }
    }

    remove() {
        if (this.refreshTimer) {
            clearInterval(this.refreshTimer);
        }
        this.grid.removeWidget(this.element);
    }
}

// Widget Factory
class WidgetFactory {
    constructor(grid) {
        this.grid = grid;
    }

    createWidget(type, config = {}, options = {}) {
        const widget = new ConfigurableWidget(this.grid, type, config);
        return widget.create(options);
    }

    createChartWidget(config = {}) {
        return this.createWidget('chart', config, { w: 6, h: 4 });
    }

    createMetricWidget(config = {}) {
        return this.createWidget('metric', config, { w: 3, h: 2 });
    }

    createTableWidget(config = {}) {
        return this.createWidget('table', config, { w: 8, h: 6 });
    }
}

// Usage
const grid = GridStack.init();
const widgetFactory = new WidgetFactory(grid);

// Create widgets with custom configurations
widgetFactory.createMetricWidget({
    title: 'Revenue',
    value: 125430,
    unit: 'USD',
    format: 'currency',
    threshold: { low: 100000, high: 200000 },
    refreshInterval: 60000
});

widgetFactory.createChartWidget({
    title: 'Sales Trend',
    chartType: 'line',
    dataSource: '/api/sales-data',
    refreshInterval: 30000
});

## Styling and Theming Enchantments

### Custom CSS Styling Alchemy

```css
/* Custom GridStack Styles */
.grid-stack {
    background: #f5f5f5;
    border-radius: 8px;
    padding: 10px;
}

.grid-stack-item {
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    transition: box-shadow 0.3s ease;
}

.grid-stack-item:hover {
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
}

.grid-stack-item-content {
    background: white;
    border-radius: 8px;
    height: 100%;
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

/* Widget Header Styles */
.widget-header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 12px 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-radius: 8px 8px 0 0;
}

.widget-title {
    font-weight: 600;
    font-size: 14px;
}

.widget-controls {
    display: flex;
    gap: 8px;
}

.widget-controls button {
    background: rgba(255, 255, 255, 0.2);
    border: none;
    color: white;
    width: 24px;
    height: 24px;
    border-radius: 4px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.2s ease;
}

.widget-controls button:hover {
    background: rgba(255, 255, 255, 0.3);
}

/* Widget Body Styles */
.widget-body {
    flex: 1;
    padding: 16px;
    overflow: auto;
}

/* Drag Handle Styles */
.grid-stack-item.ui-draggable-dragging {
    opacity: 0.8;
    transform: rotate(5deg);
    z-index: 1000;
}

/* Resize Handle Styles */
.ui-resizable-handle {
    background: #007bff;
    opacity: 0;
    transition: opacity 0.2s ease;
}

.grid-stack-item:hover .ui-resizable-handle {
    opacity: 0.7;
}

.ui-resizable-se {
    width: 12px;
    height: 12px;
    right: 3px;
    bottom: 3px;
    border-radius: 0 0 8px 0;
}

/* Dark Theme */
.grid-stack.dark-theme {
    background: #1a1a1a;
}

.dark-theme .grid-stack-item-content {
    background: #2d2d2d;
    color: #ffffff;
}

.dark-theme .widget-header {
    background: linear-gradient(135deg, #434343 0%, #000000 100%);
}

/* Mobile Responsive Styles */
@media (max-width: 768px) {
    .grid-stack {
        padding: 5px;
    }

    .widget-header {
        padding: 8px 12px;
    }

    .widget-title {
        font-size: 12px;
    }

    .widget-controls button {
        width: 20px;
        height: 20px;
    }

    .widget-body {
        padding: 12px;
    }
}

/* Animation Styles */
.grid-stack-item.grid-stack-animate {
    transition: transform 0.3s ease, width 0.3s ease, height 0.3s ease;
}

/* Placeholder Styles */
.grid-stack-placeholder {
    background: rgba(0, 123, 255, 0.2);
    border: 2px dashed #007bff;
    border-radius: 8px;
}
```

### Theme Switcher Transformation Spells

```javascript
class ThemeManager {
    constructor(gridSelector) {
        this.gridElement = document.querySelector(gridSelector);
        this.currentTheme = localStorage.getItem('gridstack-theme') || 'light';
        this.applyTheme(this.currentTheme);
    }

    applyTheme(theme) {
        this.gridElement.classList.remove('light-theme', 'dark-theme');
        this.gridElement.classList.add(`${theme}-theme`);
        this.currentTheme = theme;
        localStorage.setItem('gridstack-theme', theme);

        // Update theme-specific colors
        this.updateThemeColors(theme);
    }

    updateThemeColors(theme) {
        const root = document.documentElement;

        if (theme === 'dark') {
            root.style.setProperty('--grid-bg', '#1a1a1a');
            root.style.setProperty('--widget-bg', '#2d2d2d');
            root.style.setProperty('--widget-text', '#ffffff');
            root.style.setProperty('--widget-border', '#404040');
        } else {
            root.style.setProperty('--grid-bg', '#f5f5f5');
            root.style.setProperty('--widget-bg', '#ffffff');
            root.style.setProperty('--widget-text', '#333333');
            root.style.setProperty('--widget-border', '#e0e0e0');
        }
    }

    toggleTheme() {
        const newTheme = this.currentTheme === 'light' ? 'dark' : 'light';
        this.applyTheme(newTheme);
    }

    getCurrentTheme() {
        return this.currentTheme;
    }
}

// Usage
const themeManager = new ThemeManager('.grid-stack');

// Add theme toggle button
document.getElementById('theme-toggle').addEventListener('click', () => {
    themeManager.toggleTheme();
});
```

## Performance Optimization Enchantments

### Lazy Loading and Virtual Scrolling Efficiency Spells

```javascript
class PerformantGridManager {
    constructor(containerId, options = {}) {
        this.container = document.querySelector(containerId);
        this.options = {
            batchSize: 10,
            loadThreshold: 5,
            virtualScrolling: false,
            ...options
        };

        this.widgets = [];
        this.loadedWidgets = 0;
        this.isLoading = false;

        this.initializeGrid();
        this.setupInfiniteScroll();
    }

    initializeGrid() {
        this.grid = GridStack.init({
            cellHeight: 80,
            verticalMargin: 10,
            animate: false, // Disable animations for better performance
            ...this.options.gridOptions
        }, this.container);
    }

    async loadWidgets(widgetData) {
        this.widgets = widgetData;
        await this.loadBatch();
    }

    async loadBatch() {
        if (this.isLoading || this.loadedWidgets >= this.widgets.length) {
            return;
        }

        this.isLoading = true;
        const startIndex = this.loadedWidgets;
        const endIndex = Math.min(startIndex + this.options.batchSize, this.widgets.length);

        const batch = this.widgets.slice(startIndex, endIndex);

        // Use requestAnimationFrame for smooth rendering
        await new Promise(resolve => {
            requestAnimationFrame(() => {
                batch.forEach(widgetData => {
                    this.createWidget(widgetData);
                });
                resolve();
            });
        });

        this.loadedWidgets = endIndex;
        this.isLoading = false;
    }

    createWidget(widgetData) {
        const widget = this.grid.addWidget(widgetData.content, {
            w: widgetData.w || 4,
            h: widgetData.h || 2,
            autoPosition: true,
            id: widgetData.id
        });

        // Lazy load widget content
        if (widgetData.lazyContent) {
            this.lazyLoadContent(widget, widgetData.lazyContent);
        }

        return widget;
    }

    async lazyLoadContent(widget, contentLoader) {
        const placeholder = widget.querySelector('.widget-body');
        placeholder.innerHTML = '<div class="loading-spinner">Loading...</div>';

        try {
            const content = await contentLoader();
            placeholder.innerHTML = content;
        } catch (error) {
            placeholder.innerHTML = '<div class="error">Failed to load content</div>';
            console.error('Failed to load widget content:', error);
        }
    }

    setupInfiniteScroll() {
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting && !this.isLoading) {
                    this.loadBatch();
                }
            });
        }, {
            rootMargin: '100px'
        });

        // Create sentinel element
        const sentinel = document.createElement('div');
        sentinel.className = 'scroll-sentinel';
        sentinel.style.height = '1px';
        this.container.appendChild(sentinel);

        observer.observe(sentinel);
    }

    optimizeForMobile() {
        // Disable animations on mobile for better performance
        if (window.innerWidth < 768) {
            this.grid.opts.animate = false;
            this.grid.opts.alwaysShowResizeHandle = false;
        }
    }

    cleanup() {
        // Clean up resources
        this.grid.destroy();
        this.widgets = [];
        this.loadedWidgets = 0;
    }
}

// Usage with performance monitoring
const performanceGrid = new PerformantGridManager('.grid-stack', {
    batchSize: 15,
    loadThreshold: 3
});

// Monitor performance
const observer = new PerformanceObserver((list) => {
    list.getEntries().forEach((entry) => {
        if (entry.name.includes('gridstack')) {
            console.log(`GridStack operation: ${entry.name} took ${entry.duration}ms`);
        }
    });
});

observer.observe({ entryTypes: ['measure'] });
```

## Wisdom of the Grid Ancients

### Code Organization Sacred Rituals

```javascript
// 1. Use a centralized grid manager
class GridManager {
    constructor() {
        this.grids = new Map();
        this.widgets = new Map();
        this.eventHandlers = new Map();
    }

    createGrid(id, options) {
        const grid = GridStack.init(options, `#${id}`);
        this.grids.set(id, grid);
        this.setupGridEvents(id, grid);
        return grid;
    }

    setupGridEvents(gridId, grid) {
        const handlers = {
            change: (event, items) => this.onGridChange(gridId, items),
            added: (event, items) => this.onWidgetAdded(gridId, items),
            removed: (event, items) => this.onWidgetRemoved(gridId, items)
        };

        Object.entries(handlers).forEach(([event, handler]) => {
            grid.on(event, handler);
        });

        this.eventHandlers.set(gridId, handlers);
    }

    onGridChange(gridId, items) {
        // Centralized change handling
        this.saveGridState(gridId);
        this.notifyGridChange(gridId, items);
    }

    // ... other methods
}

// 2. Use proper error handling
function safeGridOperation(operation, fallback = null) {
    try {
        return operation();
    } catch (error) {
        console.error('Grid operation failed:', error);
        return fallback;
    }
}

// 3. Implement proper cleanup
class WidgetLifecycleManager {
    constructor() {
        this.activeWidgets = new Set();
        this.cleanupTasks = new Map();
    }

    registerWidget(widget, cleanupFn) {
        this.activeWidgets.add(widget);
        if (cleanupFn) {
            this.cleanupTasks.set(widget, cleanupFn);
        }
    }

    unregisterWidget(widget) {
        const cleanup = this.cleanupTasks.get(widget);
        if (cleanup) {
            cleanup();
            this.cleanupTasks.delete(widget);
        }
        this.activeWidgets.delete(widget);
    }

    cleanupAll() {
        this.activeWidgets.forEach(widget => {
            this.unregisterWidget(widget);
        });
    }
}
```

### Memory Management Protective Spells

```javascript
// Proper memory management for large grids
class MemoryEfficientGrid {
    constructor(containerId) {
        this.grid = GridStack.init({
            cellHeight: 80,
            animate: false // Disable animations for better memory usage
        }, containerId);

        this.widgetPool = [];
        this.activeWidgets = new Set();
        this.setupMemoryOptimizations();
    }

    setupMemoryOptimizations() {
        // Clean up removed widgets
        this.grid.on('removed', (event, items) => {
            items.forEach(item => {
                this.cleanupWidget(item.el);
            });
        });

        // Implement widget pooling for frequently created/destroyed widgets
        this.grid.on('added', (event, items) => {
            items.forEach(item => {
                this.activeWidgets.add(item.el);
            });
        });
    }

    cleanupWidget(element) {
        // Remove event listeners
        const clonedElement = element.cloneNode(true);
        element.parentNode.replaceChild(clonedElement, element);

        // Clear references
        this.activeWidgets.delete(element);

        // Return to pool if applicable
        if (this.widgetPool.length < 10) {
            this.widgetPool.push(clonedElement);
        }
    }

    getPooledWidget() {
        return this.widgetPool.pop() || null;
    }

    destroy() {
        // Clean up all widgets
        this.activeWidgets.forEach(widget => {
            this.cleanupWidget(widget);
        });

        // Destroy grid
        this.grid.destroy();

        // Clear references
        this.widgetPool = [];
        this.activeWidgets.clear();
    }
}
```

## Common Grid Curses & Their Remedies

### Curse 1: Grid Not Initializing Malfunction
Grid may not initialize if the container element doesn't exist or has no dimensions.

**Solution:**
```javascript
function initializeGridSafely(selector, options) {
    const container = document.querySelector(selector);

    if (!container) {
        console.error(`Grid container ${selector} not found`);
        return null;
    }

    // Ensure container has dimensions
    if (container.offsetWidth === 0) {
        container.style.width = '100%';
    }

    if (container.offsetHeight === 0) {
        container.style.minHeight = '400px';
    }

    return GridStack.init(options, container);
}
```

### Curse 2: Memory Leak Dynamic Widget Corruption
Not properly cleaning up widgets can cause memory leaks.

**Solution:**
```javascript
// Always clean up when removing widgets
function removeWidgetSafely(grid, widget) {
    // Remove event listeners
    const newWidget = widget.cloneNode(true);
    widget.parentNode.replaceChild(newWidget, widget);

    // Remove from grid
    grid.removeWidget(newWidget);
}
```

### Curse 3: Performance Issues with Large Grid Burden
Too many widgets can cause performance problems.

**Solution:**
```javascript
// Use virtual scrolling and lazy loading
// Disable animations for large grids
// Implement widget pooling
// Use requestAnimationFrame for batch operations
```

### Curse 4: Responsive Layout Issue Hex
Grid may not respond properly to screen size changes.

**Solution:**
```javascript
// Implement proper responsive handling
function setupResponsiveGrid(grid) {
    const handleResize = debounce(() => {
        const width = window.innerWidth;
        let columns;

        if (width < 768) columns = 1;
        else if (width < 1024) columns = 6;
        else columns = 12;

        grid.column(columns);
    }, 250);

    window.addEventListener('resize', handleResize);
    handleResize(); // Initial call
}

function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://gridstackjs.com/" %}

{% embed url="https://github.com/gridstack/gridstack.js" %}

{% embed url="https://gridstackjs.com/demo/" %}
```
```

