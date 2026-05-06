## 1. 架构设计

```mermaid
graph TB
    "前端展示层" --> "数据模拟层"
    "数据模拟层" --> "静态资源层"

    subgraph "前端展示层"
        "首页总览"
        "农情监测"
        "视频监控"
        "智能设备"
        "作业管理"
        "预警中心"
        "数据报告"
    end

    subgraph "数据模拟层"
        "Mock数据JSON"
        "模拟实时更新"
        "图表数据生成"
    end

    subgraph "静态资源层"
        "HTML/CSS/JS"
        "图标与图片"
        "字体资源"
    end
```

## 2. 技术说明

- **前端**：纯HTML5 + CSS3 + JavaScript（单文件方案，无需构建工具）
- **CSS框架**：Tailwind CSS（CDN引入）
- **图表库**：Chart.js（CDN引入，用于数据可视化）
- **图标库**：Lucide Icons（CDN引入）
- **字体**：Noto Sans SC（Google Fonts CDN）
- **后端**：无（纯前端展示，使用Mock数据模拟）
- **数据库**：无（内置JSON数据）

### 技术选型理由

1. **纯HTML方案**：用户要求HTML兼容多端，单文件部署简单，可直接在任意服务器运行
2. **Tailwind CSS CDN**：快速实现响应式布局，无需构建流程
3. **Chart.js**：轻量级图表库，支持响应式，适合数据可视化展示
4. **Mock数据**：模拟真实数据展示效果，后续可对接真实API

## 3. 路由定义

| 路由锚点 | 用途 |
|----------|------|
| #home | 首页总览，展示农场核心指标与快捷入口 |
| #monitor | 农情监测，四情数据实时展示 |
| #video | 视频监控，多路视频画面 |
| #device | 智能设备，设备状态与控制 |
| #operation | 作业管理，灌溉/农机/植保调度 |
| #alert | 预警中心，异常预警与处置 |
| #report | 数据报告，日报/周报/产量预测 |

## 4. API定义

本项目为纯前端展示，使用内置Mock数据。后续对接真实API时，数据结构定义如下：

### 4.1 农情监测数据

```typescript
interface WeatherData {
  temperature: number;
  humidity: number;
  windSpeed: number;
  windDirection: string;
  rainfall: number;
  lightIntensity: number;
  timestamp: string;
}

interface SoilData {
  layer: number;
  temperature: number;
  moisture: number;
  ph: number;
  conductivity: number;
  nitrogen: number;
  phosphorus: number;
  potassium: number;
}

interface PestData {
  pestType: string;
  count: number;
  threshold: number;
  level: 'normal' | 'warning' | 'danger';
  image?: string;
}
```

### 4.2 设备状态数据

```typescript
interface DeviceStatus {
  id: string;
  name: string;
  type: string;
  status: 'online' | 'offline' | 'fault';
  lastUpdate: string;
  location: { lat: number; lng: number };
}
```

### 4.3 作业数据

```typescript
interface IrrigationTask {
  id: string;
  fieldId: string;
  fieldName: string;
  progress: number;
  waterVolume: number;
  status: 'planned' | 'running' | 'completed';
}

interface MachineryTask {
  id: string;
  vehicleName: string;
  taskType: string;
  progress: number;
  trajectory: Array<{ lat: number; lng: number }>;
  status: 'planned' | 'running' | 'completed';
}
```

## 5. 文件结构

```
/workspace/
├── index.html          # 主页面（单文件方案，包含所有HTML/CSS/JS）
├── doc/
│   └── 无人农场梳理0506.xlsx  # 原始需求文档
└── .trae/
    └── documents/
        ├── prd.md      # 产品需求文档
        └── tech.md     # 技术架构文档
```

## 6. 响应式断点设计

| 断点 | 宽度范围 | 布局策略 |
|------|----------|----------|
| 手机 | < 768px | 单列布局，底部Tab导航，卡片纵向堆叠 |
| 平板 | 768px - 1199px | 两列布局，顶部折叠导航 |
| 桌面 | ≥ 1200px | 多列布局，左侧固定导航+主内容区 |

## 7. 性能优化策略

- CDN加载第三方库，利用浏览器缓存
- 图片使用懒加载
- 图表按需渲染，非当前页面不初始化
- CSS动画优先使用transform和opacity，避免重排
- Mock数据内置，减少网络请求
