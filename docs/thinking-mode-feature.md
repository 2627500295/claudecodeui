# 思考模式功能文档
*Claude WebUI - Thinking Mode Feature Documentation*

---

## 📋 需求文档 (Requirements)

### 功能概述
在Claude WebUI中实现思考模式选择功能，允许用户在发送消息时指定Claude的思考强度级别。

### 业务需求
- **BR-01**: 用户可以选择不同的思考模式来控制Claude的回答质量和深度
- **BR-02**: 提供直观的UI控件让用户轻松切换思考模式
- **BR-03**: 思考模式选择应与现有的模型选择和权限模式保持一致的用户体验
- **BR-04**: 思考模式参数需要传递给后端服务进行处理

### 功能需求
- **FR-01**: 支持5个思考模式级别：Auto, Think, Think Hard, Think Harder, UltraThink
- **FR-02**: 提供可视化指示器显示当前选择的思考强度
- **FR-03**: 支持点击切换模式，循环选择所有可用模式
- **FR-04**: 与WebSocket消息集成，将思考模式参数发送给后端
- **FR-05**: 响应式设计，适配移动端和桌面端

### 非功能需求
- **NFR-01**: UI响应时间 < 100ms
- **NFR-02**: 与现有代码风格保持一致
- **NFR-03**: 支持暗色/亮色主题
- **NFR-04**: 无障碍访问支持（键盘导航、屏幕阅读器）

### 技术需求
- **TR-01**: 使用React Hooks进行状态管理
- **TR-02**: 使用Tailwind CSS进行样式设计
- **TR-03**: 类型定义：`"auto" | "think" | "think_hard" | "think_harder" | "ultrathink"`
- **TR-04**: 与现有WebSocket通信协议集成

---

## 🎨 设计文档 (Design)

### 架构设计

#### 组件层级结构
```
ChatInterface
├── Model Selector (existing)
├── Thinking Mode Selector (new)
└── Permission Mode Selector (existing)
```

#### 数据流
```
User Click → handleThinkingModeChange() → setThinkingMode() → UI Update
User Submit → sendMessage() → WebSocket → Backend
```

### UI/UX设计

#### 视觉设计
- **位置**: 位于权限模式选择器左侧
- **布局**: 水平排列的按钮组
- **尺寸**: 与现有控件保持一致 (`px-3 py-1.5`)
- **间距**: 使用 `gap-3` 保持统一间距

#### 颜色方案
| 模式 | 主色调 | 背景色 (亮色) | 背景色 (暗色) |
|------|--------|---------------|---------------|
| Auto | Gray | bg-gray-50 | bg-gray-700 |
| Think | Blue | bg-blue-50 | bg-blue-900/20 |
| Think Hard | Cyan | bg-cyan-50 | bg-cyan-900/20 |
| Think Harder | Indigo | bg-indigo-50 | bg-indigo-900/20 |
| UltraThink | Purple | bg-purple-50 | bg-purple-900/20 |

#### 可视化指示器
- **设计**: 5个递增高度的条形图标
- **高度**: `h-2, h-3, h-3.5, h-4, h-5`
- **宽度**: 统一 `w-1`
- **激活状态**: 根据选择的模式点亮对应数量的条形
- **间距**: `gap-0.5`

### 交互设计
- **触发方式**: 点击切换
- **切换逻辑**: 循环切换所有5个模式
- **反馈**: 即时视觉变化 + Hover效果
- **提示文本**: "Click to change thinking mode"

---

## 🔧 实现文档 (Implementation)

### 代码结构

#### 状态管理
```javascript
// State definition
const [thinkingMode, setThinkingMode] = useState('auto');

// Handler function
const handleThinkingModeChange = () => {
  const modes = ['auto', 'think', 'think_hard', 'think_harder', 'ultrathink'];
  const currentIndex = modes.indexOf(thinkingMode);
  const nextIndex = (currentIndex + 1) % modes.length;
  setThinkingMode(modes[nextIndex]);
};
```

#### UI组件实现
```javascript
{/* Thinking Mode Selector */}
<button
  type="button"
  onClick={handleThinkingModeChange}
  className={/* Dynamic styling based on thinkingMode */}
  title="Click to change thinking mode"
>
  <div className="flex items-center gap-2">
    {/* Visual indicator bars */}
    <div className="flex items-center gap-0.5">
      {/* 5 bars with conditional activation */}
    </div>
    <span>
      {/* Mode label */}
    </span>
  </div>
</button>
```

#### 后端集成
```javascript
// Message sending integration
sendMessage({
  type: 'claude-command',
  command: input,
  options: {
    // ... existing options
    thinkingMode: thinkingMode, // Add thinking mode parameter
    // ... other options
  }
});
```

### 关键实现细节

#### 1. 条件样式系统
- 使用三元运算符链进行条件样式应用
- 支持亮色和暗色主题
- Hover状态增强用户体验

#### 2. 可视化指示器逻辑
```javascript
// Example for bar activation logic
['think', 'think_hard', 'think_harder', 'ultrathink'].includes(thinkingMode) 
  ? 'bg-current' : 'bg-gray-300'
```

#### 3. 响应式设计
- 使用Tailwind CSS的响应式类
- 移动端友好的触摸目标尺寸
- 自适应文本和图标大小

### 性能考虑
- **状态更新**: 使用React的批处理优化
- **重渲染**: 使用条件渲染减少不必要的更新
- **CSS**: 利用Tailwind的JIT模式优化构建体积

---

## 📝 任务文档 (Tasks)

### 开发任务清单

#### ✅ Phase 1: 基础实现
- [x] **T-001**: 添加思考模式状态管理
  - 添加 `thinkingMode` 状态变量
  - 添加 `setThinkingMode` 状态更新函数
  - 初始值设置为 `'auto'`

- [x] **T-002**: 实现模式切换逻辑
  - 创建 `handleThinkingModeChange` 函数
  - 实现循环切换逻辑
  - 支持5个模式：auto, think, think_hard, think_harder, ultrathink

- [x] **T-003**: 构建UI组件
  - 创建思考模式选择按钮
  - 实现条形指示器
  - 添加模式标签显示

#### ✅ Phase 2: 样式和布局
- [x] **T-004**: 位置布局
  - 将组件放置在权限模式选择器左侧
  - 保持与其他控件的一致性
  - 响应式布局适配

- [x] **T-005**: 视觉样式
  - 实现5种不同颜色方案
  - 添加Hover和焦点状态
  - 支持暗色主题

- [x] **T-006**: 可视化指示器
  - 创建5个不同高度的条形
  - 实现条件激活逻辑
  - 颜色和状态同步

#### ✅ Phase 3: 功能集成
- [x] **T-007**: 后端参数传递
  - 在 `sendMessage` 中添加 `thinkingMode` 参数
  - 确保与现有选项正确集成
  - 验证数据传输

- [x] **T-008**: 类型规范更新
  - 更新思考模式值为规范格式
  - 确保类型一致性
  - 代码重构优化

#### 📋 Phase 4: 测试和优化 (Future)
- [ ] **T-009**: 单元测试
  - 状态管理测试
  - 事件处理测试
  - UI渲染测试

- [ ] **T-010**: 集成测试
  - WebSocket消息传递测试
  - 跨浏览器兼容性测试
  - 移动端响应式测试

- [ ] **T-011**: 用户体验测试
  - 可用性测试
  - 无障碍访问测试
  - 性能基准测试

### 部署清单
- [ ] **D-001**: 代码审查
- [ ] **D-002**: 测试环境验证
- [ ] **D-003**: 生产环境部署
- [ ] **D-004**: 用户文档更新
- [ ] **D-005**: 性能监控设置

### 维护任务
- [ ] **M-001**: 定期代码质量检查
- [ ] **M-002**: 用户反馈收集和分析
- [ ] **M-003**: 性能优化迭代
- [ ] **M-004**: 新功能扩展评估

---

## 🔍 测试策略

### 功能测试
1. **模式切换测试**: 验证所有5个模式的正确切换
2. **视觉指示器测试**: 确认条形指示器正确显示
3. **颜色主题测试**: 验证亮色和暗色主题支持
4. **响应式测试**: 确认移动端和桌面端显示

### 集成测试
1. **WebSocket集成**: 验证思考模式参数正确传递
2. **状态持久化**: 测试页面刷新后状态保持
3. **其他控件兼容性**: 确认与模型选择和权限模式无冲突

### 用户体验测试
1. **可用性测试**: 用户能够直观理解和使用功能
2. **无障碍测试**: 键盘导航和屏幕阅读器支持
3. **性能测试**: UI响应时间和内存使用

---

## 📊 技术规格

### 技术栈
- **Frontend**: React 18+ with Hooks
- **Styling**: Tailwind CSS
- **TypeScript**: 类型定义支持
- **Communication**: WebSocket

### 数据格式
```typescript
interface ThinkingModeOptions {
  thinkingMode: "auto" | "think" | "think_hard" | "think_harder" | "ultrathink";
}

interface MessageOptions {
  projectPath: string;
  cwd: string;
  sessionId?: string;
  resume?: boolean;
  toolsSettings: any;
  permissionMode: string;
  model: string;
  thinkingMode: string; // New parameter
  images?: any[];
}
```

### 性能指标
- **首次渲染**: < 50ms
- **模式切换**: < 100ms  
- **内存占用**: < 1MB 增量
- **构建体积**: < 5KB 增量

---

## 🚀 部署说明

### 前置条件
- Node.js 16+
- 现有Claude WebUI环境
- Tailwind CSS配置

### 安装步骤
1. 确保所有依赖已安装
2. 代码已集成到 `src/components/ChatInterface.jsx`
3. 样式已通过Tailwind CSS编译
4. WebSocket后端支持新的 `thinkingMode` 参数

### 验证步骤
1. 启动开发服务器
2. 检查思考模式控件是否正确显示
3. 测试模式切换功能
4. 验证消息发送时参数正确传递
5. 确认响应式设计在各设备上正常工作

---

*文档版本: 1.0*  
*最后更新: 2025-01-15*  
*作者: Claude Code Assistant*