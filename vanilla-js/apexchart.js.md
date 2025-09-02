---
description: >-
  ApexCharts is a modern charting library that helps developers create beautiful
  and interactive visualizations for web applications with a simple API.
---

# ApexChart.js

## Introduction

ApexCharts is a modern charting library that helps developers create beautiful and interactive visualizations for web applications. It provides a wide variety of chart types, is highly customizable, and works seamlessly with popular frameworks like React, Vue, and Angular.

## Use Cases

1. **Dashboard Analytics**: Create comprehensive dashboards with multiple chart types for business intelligence
2. **Real-time Data Visualization**: Display live data updates with smooth animations
3. **Financial Charts**: Build stock charts, candlestick charts, and financial indicators
4. **Statistical Reports**: Generate charts for data analysis and reporting
5. **Mobile-Responsive Charts**: Create charts that work perfectly on all device sizes

## Installation

### CDN

```html
<script src="https://cdn.jsdelivr.net/npm/apexcharts"></script>
```

### NPM

```bash
npm install apexcharts
```

### Yarn

```bash
yarn add apexcharts
```

## Basic Setup

### HTML Structure

```html
<!DOCTYPE html>
<html>
<head>
    <title>ApexCharts Example</title>
    <script src="https://cdn.jsdelivr.net/npm/apexcharts"></script>
</head>
<body>
    <div id="chart"></div>
    <script src="script.js"></script>
</body>
</html>
```

### Basic Line Chart

```javascript
// Basic configuration
const options = {
  series: [{
    name: 'Sales',
    data: [30, 40, 35, 50, 49, 60, 70, 91, 125]
  }],
  chart: {
    type: 'line',
    height: 350
  },
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']
  },
  title: {
    text: 'Monthly Sales Data',
    align: 'center'
  }
};

// Create and render chart
const chart = new ApexCharts(document.querySelector("#chart"), options);
chart.render();
```

## Chart Types

### Line Charts

```javascript
const lineChartOptions = {
  series: [
    {
      name: 'Revenue',
      data: [44, 55, 57, 56, 61, 58, 63, 60, 66]
    },
    {
      name: 'Profit',
      data: [76, 85, 101, 98, 87, 105, 91, 114, 94]
    }
  ],
  chart: {
    type: 'line',
    height: 350,
    zoom: {
      enabled: true
    }
  },
  dataLabels: {
    enabled: false
  },
  stroke: {
    curve: 'smooth',
    width: 3
  },
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']
  },
  colors: ['#008FFB', '#00E396'],
  markers: {
    size: 6,
    hover: {
      size: 8
    }
  }
};
```

### Bar Charts

```javascript
const barChartOptions = {
  series: [{
    name: 'Sales',
    data: [400, 430, 448, 470, 540, 580, 690, 1100, 1200, 1380]
  }],
  chart: {
    type: 'bar',
    height: 350
  },
  plotOptions: {
    bar: {
      borderRadius: 4,
      horizontal: false,
      columnWidth: '55%',
      endingShape: 'rounded'
    }
  },
  dataLabels: {
    enabled: true,
    formatter: function (val) {
      return val + "k";
    }
  },
  xaxis: {
    categories: ['Q1', 'Q2', 'Q3', 'Q4', 'Q5', 'Q6', 'Q7', 'Q8', 'Q9', 'Q10']
  },
  colors: ['#FF6B6B']
};
```

### Area Charts

```javascript
const areaChartOptions = {
  series: [{
    name: 'Website Traffic',
    data: [31, 40, 28, 51, 42, 109, 100]
  }, {
    name: 'Mobile Traffic',
    data: [11, 32, 45, 32, 34, 52, 41]
  }],
  chart: {
    type: 'area',
    height: 350,
    stacked: false
  },
  fill: {
    type: 'gradient',
    gradient: {
      opacityFrom: 0.6,
      opacityTo: 0.8
    }
  },
  dataLabels: {
    enabled: false
  },
  stroke: {
    curve: 'smooth'
  },
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul']
  }
};
```

### Pie Charts

```javascript
const pieChartOptions = {
  series: [44, 55, 13, 43, 22],
  chart: {
    type: 'pie',
    height: 350
  },
  labels: ['Team A', 'Team B', 'Team C', 'Team D', 'Team E'],
  colors: ['#008FFB', '#00E396', '#FEB019', '#FF4560', '#775DD0'],
  legend: {
    position: 'bottom'
  },
  plotOptions: {
    pie: {
      donut: {
        size: '0%'
      }
    }
  },
  responsive: [{
    breakpoint: 480,
    options: {
      chart: {
        width: 200
      },
      legend: {
        position: 'bottom'
      }
    }
  }]
};
```

### Donut Charts

```javascript
const donutChartOptions = {
  series: [44, 55, 41, 17, 15],
  chart: {
    type: 'donut',
    height: 350
  },
  labels: ['Desktop', 'Mobile', 'Tablet', 'Smart TV', 'Others'],
  plotOptions: {
    pie: {
      donut: {
        size: '70%',
        labels: {
          show: true,
          total: {
            show: true,
            label: 'Total',
            formatter: function (w) {
              return w.globals.seriesTotals.reduce((a, b) => {
                return a + b;
              }, 0);
            }
          }
        }
      }
    }
  },
  colors: ['#1f77b4', '#ff7f0e', '#2ca02c', '#d62728', '#9467bd']
};

## Advanced Chart Types

### Candlestick Charts (Financial)

```javascript
const candlestickOptions = {
  series: [{
    name: 'Stock Price',
    data: [
      { x: new Date(2023, 0, 1), y: [51.98, 56.29, 51.59, 53.85] },
      { x: new Date(2023, 0, 2), y: [53.66, 54.99, 51.35, 52.95] },
      { x: new Date(2023, 0, 3), y: [52.96, 53.78, 51.54, 52.48] },
      { x: new Date(2023, 0, 4), y: [52.54, 52.79, 47.88, 49.24] },
      { x: new Date(2023, 0, 5), y: [49.10, 52.86, 47.70, 52.78] }
    ]
  }],
  chart: {
    type: 'candlestick',
    height: 350
  },
  title: {
    text: 'Stock Price Movement',
    align: 'left'
  },
  xaxis: {
    type: 'datetime'
  },
  yaxis: {
    tooltip: {
      enabled: true
    }
  },
  plotOptions: {
    candlestick: {
      colors: {
        upward: '#00B746',
        downward: '#EF403C'
      }
    }
  }
};
```

### Heatmap Charts

```javascript
const heatmapOptions = {
  series: [
    {
      name: 'Jan',
      data: [
        { x: 'W1', y: 22 },
        { x: 'W2', y: 29 },
        { x: 'W3', y: 13 },
        { x: 'W4', y: 32 }
      ]
    },
    {
      name: 'Feb',
      data: [
        { x: 'W1', y: 43 },
        { x: 'W2', y: 43 },
        { x: 'W3', y: 23 },
        { x: 'W4', y: 35 }
      ]
    },
    {
      name: 'Mar',
      data: [
        { x: 'W1', y: 35 },
        { x: 'W2', y: 21 },
        { x: 'W3', y: 47 },
        { x: 'W4', y: 31 }
      ]
    }
  ],
  chart: {
    type: 'heatmap',
    height: 350
  },
  dataLabels: {
    enabled: false
  },
  colors: ['#008FFB'],
  plotOptions: {
    heatmap: {
      shadeIntensity: 0.5,
      colorScale: {
        ranges: [
          { from: -30, to: 5, color: '#00A100' },
          { from: 6, to: 20, color: '#128FD9' },
          { from: 21, to: 45, color: '#FFB200' },
          { from: 46, to: 55, color: '#FF0000' }
        ]
      }
    }
  }
};
```

### Radar Charts

```javascript
const radarOptions = {
  series: [{
    name: 'Skills Assessment',
    data: [80, 50, 30, 40, 100, 20]
  }],
  chart: {
    type: 'radar',
    height: 350
  },
  xaxis: {
    categories: ['JavaScript', 'Python', 'Java', 'C++', 'React', 'Vue']
  },
  fill: {
    opacity: 0.2
  },
  markers: {
    size: 6
  },
  colors: ['#FF6B6B']
};
```

## Real-time Data Updates

### Dynamic Data Updates

```javascript
class RealTimeChart {
  constructor(containerId) {
    this.chart = null;
    this.containerId = containerId;
    this.data = [];
    this.init();
  }

  init() {
    const options = {
      series: [{
        name: 'Real-time Data',
        data: this.data
      }],
      chart: {
        type: 'line',
        height: 350,
        animations: {
          enabled: true,
          easing: 'linear',
          dynamicAnimation: {
            speed: 1000
          }
        }
      },
      xaxis: {
        type: 'datetime',
        range: 60000 // 1 minute range
      },
      yaxis: {
        max: 100
      },
      stroke: {
        curve: 'smooth'
      }
    };

    this.chart = new ApexCharts(document.querySelector(this.containerId), options);
    this.chart.render();
  }

  addDataPoint(value) {
    const timestamp = new Date().getTime();
    this.data.push([timestamp, value]);

    // Keep only last 60 data points
    if (this.data.length > 60) {
      this.data.shift();
    }

    this.chart.updateSeries([{
      data: this.data
    }]);
  }

  startRealTimeUpdates() {
    setInterval(() => {
      const randomValue = Math.floor(Math.random() * 100);
      this.addDataPoint(randomValue);
    }, 1000);
  }
}

// Usage
const realTimeChart = new RealTimeChart('#realtime-chart');
realTimeChart.startRealTimeUpdates();
```

### WebSocket Integration

```javascript
class WebSocketChart {
  constructor(containerId, websocketUrl) {
    this.chart = null;
    this.websocket = null;
    this.containerId = containerId;
    this.websocketUrl = websocketUrl;
    this.data = [];
    this.init();
    this.connectWebSocket();
  }

  init() {
    const options = {
      series: [{
        name: 'Live Data',
        data: this.data
      }],
      chart: {
        type: 'area',
        height: 350,
        animations: {
          enabled: true,
          easing: 'linear',
          dynamicAnimation: {
            speed: 1000
          }
        }
      },
      xaxis: {
        type: 'datetime'
      },
      fill: {
        type: 'gradient',
        gradient: {
          shadeIntensity: 1,
          opacityFrom: 0.7,
          opacityTo: 0.9
        }
      }
    };

    this.chart = new ApexCharts(document.querySelector(this.containerId), options);
    this.chart.render();
  }

  connectWebSocket() {
    this.websocket = new WebSocket(this.websocketUrl);

    this.websocket.onmessage = (event) => {
      const data = JSON.parse(event.data);
      this.updateChart(data);
    };

    this.websocket.onclose = () => {
      console.log('WebSocket connection closed. Attempting to reconnect...');
      setTimeout(() => this.connectWebSocket(), 3000);
    };
  }

  updateChart(newData) {
    this.data.push([new Date().getTime(), newData.value]);

    // Keep only last 100 data points
    if (this.data.length > 100) {
      this.data.shift();
    }

    this.chart.updateSeries([{
      data: this.data
    }]);
  }
}
```

## Customization and Styling

### Custom Themes

```javascript
const customTheme = {
  chart: {
    background: '#1a1a1a',
    foreColor: '#ffffff'
  },
  colors: ['#00E396', '#008FFB', '#FEB019', '#FF4560', '#775DD0'],
  grid: {
    borderColor: '#40475D'
  },
  xaxis: {
    axisBorder: {
      color: '#40475D'
    },
    axisTicks: {
      color: '#40475D'
    }
  },
  yaxis: {
    axisBorder: {
      color: '#40475D'
    },
    axisTicks: {
      color: '#40475D'
    }
  }
};

const darkThemeOptions = {
  series: [{
    name: 'Sales',
    data: [30, 40, 35, 50, 49, 60, 70, 91, 125]
  }],
  chart: {
    type: 'line',
    height: 350,
    ...customTheme.chart
  },
  colors: customTheme.colors,
  grid: customTheme.grid,
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep'],
    ...customTheme.xaxis
  },
  yaxis: customTheme.yaxis
};
```

### Custom Tooltips

```javascript
const customTooltipOptions = {
  series: [{
    name: 'Revenue',
    data: [44, 55, 57, 56, 61, 58, 63, 60, 66]
  }],
  chart: {
    type: 'line',
    height: 350
  },
  tooltip: {
    custom: function({ series, seriesIndex, dataPointIndex, w }) {
      const value = series[seriesIndex][dataPointIndex];
      const category = w.globals.labels[dataPointIndex];

      return `
        <div class="custom-tooltip">
          <div class="tooltip-header">${category}</div>
          <div class="tooltip-body">
            <span class="tooltip-label">Revenue:</span>
            <span class="tooltip-value">$${value}k</span>
          </div>
        </div>
      `;
    }
  },
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']
  }
};

## Events and Interactions

### Chart Events

```javascript
const interactiveOptions = {
  series: [{
    name: 'Sales',
    data: [30, 40, 35, 50, 49, 60, 70, 91, 125]
  }],
  chart: {
    type: 'line',
    height: 350,
    events: {
      click: function(event, chartContext, config) {
        console.log('Chart clicked:', config);
      },
      dataPointSelection: function(event, chartContext, config) {
        console.log('Data point selected:', config);
        const { dataPointIndex, seriesIndex } = config;
        const value = chartContext.w.config.series[seriesIndex].data[dataPointIndex];
        alert(`Selected value: ${value}`);
      },
      legendClick: function(chartContext, seriesIndex, config) {
        console.log('Legend clicked for series:', seriesIndex);
      },
      markerClick: function(event, chartContext, { seriesIndex, dataPointIndex, config }) {
        console.log('Marker clicked:', { seriesIndex, dataPointIndex });
      }
    }
  },
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']
  }
};
```

### Brush Chart (Zoom and Pan)

```javascript
const brushChartOptions = {
  series: [{
    name: 'Stock Price',
    data: generateTimeSeriesData() // Your time series data
  }],
  chart: {
    type: 'line',
    height: 230,
    id: 'main-chart',
    brush: {
      target: 'brush-chart',
      enabled: true
    },
    selection: {
      enabled: true,
      xaxis: {
        min: new Date('19 Jun 2017').getTime(),
        max: new Date('14 Aug 2017').getTime()
      }
    }
  },
  xaxis: {
    type: 'datetime'
  }
};

const brushOptions = {
  series: [{
    name: 'Stock Price',
    data: generateTimeSeriesData() // Same data as main chart
  }],
  chart: {
    type: 'line',
    height: 130,
    id: 'brush-chart',
    brush: {
      target: 'main-chart',
      enabled: true
    },
    selection: {
      enabled: true
    }
  },
  xaxis: {
    type: 'datetime',
    tooltip: {
      enabled: false
    }
  },
  yaxis: {
    tickAmount: 2
  }
};
```

## Responsive Design

### Mobile-Responsive Configuration

```javascript
const responsiveOptions = {
  series: [{
    name: 'Sales',
    data: [30, 40, 35, 50, 49, 60, 70, 91, 125]
  }],
  chart: {
    type: 'line',
    height: 350
  },
  responsive: [
    {
      breakpoint: 480,
      options: {
        chart: {
          width: '100%',
          height: 300
        },
        legend: {
          position: 'bottom'
        },
        xaxis: {
          labels: {
            rotate: -45
          }
        }
      }
    },
    {
      breakpoint: 768,
      options: {
        chart: {
          height: 400
        },
        plotOptions: {
          bar: {
            horizontal: false
          }
        }
      }
    }
  ],
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']
  }
};
```

## Animations and Transitions

### Custom Animation Settings

```javascript
const animatedOptions = {
  series: [{
    name: 'Growth',
    data: [30, 40, 35, 50, 49, 60, 70, 91, 125]
  }],
  chart: {
    type: 'bar',
    height: 350,
    animations: {
      enabled: true,
      easing: 'easeinout',
      speed: 800,
      animateGradually: {
        enabled: true,
        delay: 150
      },
      dynamicAnimation: {
        enabled: true,
        speed: 350
      }
    }
  },
  plotOptions: {
    bar: {
      borderRadius: 4,
      horizontal: false
    }
  },
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']
  }
};
```

## Data Management

### Dynamic Data Loading

```javascript
class ChartDataManager {
  constructor(chartId, initialOptions) {
    this.chart = new ApexCharts(document.querySelector(chartId), initialOptions);
    this.chart.render();
  }

  async loadData(url) {
    try {
      const response = await fetch(url);
      const data = await response.json();
      this.updateChart(data);
    } catch (error) {
      console.error('Error loading data:', error);
    }
  }

  updateChart(newData) {
    this.chart.updateSeries([{
      name: 'Updated Data',
      data: newData
    }]);
  }

  updateOptions(newOptions) {
    this.chart.updateOptions(newOptions);
  }

  addSeries(seriesData) {
    this.chart.appendSeries(seriesData);
  }

  removeSeries(seriesName) {
    // Find series index by name
    const seriesIndex = this.chart.w.config.series.findIndex(s => s.name === seriesName);
    if (seriesIndex !== -1) {
      this.chart.toggleSeries(seriesName);
    }
  }

  exportChart(format = 'png') {
    this.chart.dataURI().then((uri) => {
      const link = document.createElement('a');
      link.href = uri.imgURI;
      link.download = `chart.${format}`;
      link.click();
    });
  }
}

// Usage
const chartManager = new ChartDataManager('#dynamic-chart', {
  series: [],
  chart: { type: 'line', height: 350 }
});

// Load data from API
chartManager.loadData('/api/chart-data');
```

### Data Filtering and Sorting

```javascript
class FilterableChart {
  constructor(containerId, rawData) {
    this.containerId = containerId;
    this.rawData = rawData;
    this.filteredData = rawData;
    this.chart = null;
    this.init();
  }

  init() {
    const options = {
      series: [{
        name: 'Values',
        data: this.filteredData
      }],
      chart: {
        type: 'column',
        height: 350
      },
      xaxis: {
        categories: this.filteredData.map((_, index) => `Item ${index + 1}`)
      }
    };

    this.chart = new ApexCharts(document.querySelector(this.containerId), options);
    this.chart.render();
  }

  filterData(minValue, maxValue) {
    this.filteredData = this.rawData.filter(value => value >= minValue && value <= maxValue);
    this.updateChart();
  }

  sortData(ascending = true) {
    this.filteredData = [...this.filteredData].sort((a, b) =>
      ascending ? a - b : b - a
    );
    this.updateChart();
  }

  updateChart() {
    this.chart.updateSeries([{
      name: 'Values',
      data: this.filteredData
    }]);

    this.chart.updateOptions({
      xaxis: {
        categories: this.filteredData.map((_, index) => `Item ${index + 1}`)
      }
    });
  }

  resetData() {
    this.filteredData = [...this.rawData];
    this.updateChart();
  }
}
```

## Best Practices

### Performance Optimization

```javascript
// 1. Use appropriate chart types for data size
const performantOptions = {
  series: [{
    name: 'Large Dataset',
    data: largeDataArray // 1000+ points
  }],
  chart: {
    type: 'line', // Better for large datasets than bar charts
    height: 350,
    animations: {
      enabled: false // Disable animations for large datasets
    },
    zoom: {
      enabled: true,
      type: 'x',
      autoScaleYaxis: true
    }
  },
  stroke: {
    width: 1 // Thinner lines for better performance
  },
  markers: {
    size: 0 // Disable markers for large datasets
  },
  dataLabels: {
    enabled: false // Disable data labels for performance
  }
};

// 2. Lazy loading for multiple charts
function lazyLoadChart(containerId, options) {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const chart = new ApexCharts(entry.target, options);
        chart.render();
        observer.unobserve(entry.target);
      }
    });
  });

  observer.observe(document.querySelector(containerId));
}

// 3. Memory management
function destroyChart(chart) {
  if (chart) {
    chart.destroy();
    chart = null;
  }
}
```

### Accessibility

```javascript
const accessibleOptions = {
  series: [{
    name: 'Sales Data',
    data: [30, 40, 35, 50, 49, 60, 70, 91, 125]
  }],
  chart: {
    type: 'line',
    height: 350
  },
  title: {
    text: 'Monthly Sales Report',
    style: {
      fontSize: '16px'
    }
  },
  xaxis: {
    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep'],
    title: {
      text: 'Months'
    }
  },
  yaxis: {
    title: {
      text: 'Sales (in thousands)'
    }
  },
  // Add ARIA labels
  chart: {
    description: 'A line chart showing monthly sales data from January to September'
  }
};

// Add keyboard navigation
document.addEventListener('keydown', function(event) {
  if (event.target.closest('.apexcharts-canvas')) {
    // Handle keyboard navigation
    switch(event.key) {
      case 'ArrowLeft':
        // Navigate to previous data point
        break;
      case 'ArrowRight':
        // Navigate to next data point
        break;
    }
  }
});
```

## Common Pitfalls

### Issue 1: Chart Not Rendering
Charts may not render if the container element doesn't exist or has no dimensions.

**Solution:**
```javascript
// Ensure container exists and has dimensions
function createChart(containerId, options) {
  const container = document.querySelector(containerId);

  if (!container) {
    console.error(`Container ${containerId} not found`);
    return;
  }

  // Ensure container has dimensions
  if (container.offsetWidth === 0 || container.offsetHeight === 0) {
    container.style.width = '100%';
    container.style.height = '400px';
  }

  const chart = new ApexCharts(container, options);
  chart.render();
  return chart;
}
```

### Issue 2: Memory Leaks with Dynamic Updates
Not properly cleaning up charts can cause memory leaks.

**Solution:**
```javascript
class ChartManager {
  constructor() {
    this.charts = new Map();
  }

  createChart(id, options) {
    // Destroy existing chart if it exists
    if (this.charts.has(id)) {
      this.charts.get(id).destroy();
    }

    const chart = new ApexCharts(document.querySelector(`#${id}`), options);
    chart.render();
    this.charts.set(id, chart);
    return chart;
  }

  destroyChart(id) {
    if (this.charts.has(id)) {
      this.charts.get(id).destroy();
      this.charts.delete(id);
    }
  }

  destroyAll() {
    this.charts.forEach(chart => chart.destroy());
    this.charts.clear();
  }
}
```

### Issue 3: Responsive Issues
Charts may not resize properly on window resize.

**Solution:**
```javascript
// Auto-resize charts on window resize
window.addEventListener('resize', debounce(() => {
  // Resize all charts
  document.querySelectorAll('.apexcharts-canvas').forEach(canvas => {
    const chart = ApexCharts.getChartByID(canvas.id);
    if (chart) {
      chart.windowResizeHandler();
    }
  });
}, 250));

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

## References

{% embed url="https://apexcharts.com/" %}

{% embed url="https://apexcharts.com/docs/" %}

{% embed url="https://github.com/apexcharts/apexcharts.js" %}
```
```
