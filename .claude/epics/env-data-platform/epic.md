---
name: env-data-platform
status: backlog
created: 2025-09-16T08:15:46Z
progress: 0%
prd: .claude/prds/env-data-platform.md
github: [Will be updated when synced to GitHub]
---

# Epic: 环境数据集成平台 (Environmental Data Integration Platform)

## Overview

基于现有Doubao设计系统原型，构建企业级环境数据集成平台。采用微服务架构，前端基于Vue3+TypeScript，后端使用Spring Boot，支持国产化部署。核心特点是低技术门槛、高可用性和模块化扩展。

## Architecture Decisions

### 技术栈选择
- **前端**: Vue3 + TypeScript + Vite + Pinia (轻量级，与原型Doubao设计系统兼容)
- **后端**: Spring Boot + MyBatis-Plus (成熟稳定，信创兼容)
- **数据库**: MySQL 8.0 + Redis (关系型+缓存，支持国产化替换)
- **消息队列**: RocketMQ (阿里开源，高可靠)
- **任务调度**: XXL-JOB (可视化，易维护)
- **API网关**: Spring Cloud Gateway (统一认证和限流)
- **监控**: Prometheus + Grafana + ELK Stack

### 设计模式
- **微服务架构**: 按功能域拆分，独立部署和扩展
- **DDD领域建模**: 数据源、ETL、目录、API、监控五大领域
- **插件化设计**: 数据源适配器和ETL算子支持插件扩展
- **事件驱动**: 使用事件总线解耦服务间通信

### 国产化适配
- 支持达梦/人大金仓数据库替换MySQL
- 兼容麒麟/统信操作系统
- 使用国产JDK（龙芯、华为毕昇等）
- 容器化支持国产Docker（PouchContainer）

## Technical Approach

### Frontend Components
- **主导航Shell**: 基于原型iframe架构升级为现代SPA路由
- **数据源管理**: 连接配置表单+状态监控看板
- **ETL设计器**: 拖拽式流程图+配置面板（基于G6图形引擎）
- **数据目录**: 树形分类+搜索过滤+详情页面
- **API管理**: 接口列表+文档展示+权限配置
- **监控Dashboard**: 实时图表+告警中心+日志查询

### Backend Services
```
env-platform-gateway     # API网关服务
├── env-platform-auth    # 认证授权服务
├── env-platform-datasource # 数据源管理服务
├── env-platform-etl     # ETL处理引擎服务
├── env-platform-catalog # 数据目录服务
├── env-platform-api     # API生成和管理服务
└── env-platform-monitor # 监控告警服务
```

**核心数据模型**:
- DataSource: 数据源连接信息和状态
- EtlJob: ETL任务配置和执行记录
- DataAsset: 数据资产元数据和血缘关系
- ApiEndpoint: API端点定义和访问统计
- AlertRule: 告警规则和通知配置

### Infrastructure
- **部署方式**: Docker Compose本地 + K8s政务云
- **数据存储**: 主库MySQL + 时序数据InfluxDB + 文件存储MinIO
- **缓存策略**: Redis集群，API响应缓存+会话存储
- **负载均衡**: Nginx反向代理+健康检查
- **备份恢复**: 数据库定时备份+配置文件版本控制

## Implementation Strategy

### 开发阶段
**Phase 1: 核心基础 (2个月)**
- 认证授权和用户管理
- 数据源管理和HJ212协议适配
- 基础数据目录功能

**Phase 2: 数据处理 (2个月)**
- ETL可视化设计器
- 任务调度和执行引擎
- 数据质量监控

**Phase 3: 服务化 (1.5个月)**
- API自动生成和管理
- 权限控制和限流
- 接口文档系统

**Phase 4: 智能运维 (0.5个月)**
- 监控Dashboard完善
- 智能告警和问题诊断
- 性能优化和部署

### 风险缓解
- **技术风险**: 核心功能优先开发，复杂算法采用开源方案
- **集成风险**: 提前验证HJ212协议和主要数据源连接
- **性能风险**: 早期进行压力测试，及时调整架构

### 测试策略
- **单元测试**: 核心业务逻辑覆盖率>80%
- **集成测试**: 数据源连接和ETL流程端到端测试
- **性能测试**: 模拟1000并发用户，验证响应时间要求
- **安全测试**: SQL注入、权限绕过等安全漏洞扫描

## Task Breakdown Preview

基于现有原型界面，简化实现路径的任务分解：

- [ ] **基础设施搭建**: 项目架构、数据库设计、CI/CD流水线
- [ ] **用户认证系统**: 登录认证、权限管理、SSO集成
- [ ] **数据源管理模块**: HJ212协议支持、连接配置、状态监控
- [ ] **ETL处理引擎**: 可视化设计器、调度执行、数据转换
- [ ] **数据目录系统**: 元数据管理、分类标签、搜索查询
- [ ] **API服务平台**: 接口生成、文档管理、权限控制
- [ ] **监控告警系统**: 实时监控、智能告警、日志分析
- [ ] **前端界面集成**: 基于Doubao设计系统的统一界面
- [ ] **部署配置优化**: 国产化兼容、性能调优、安全加固
- [ ] **测试验收交付**: 功能测试、性能测试、用户培训

## Dependencies

### External Dependencies
- **HJ212协议规范**: 需要完整的协议文档和测试设备
- **政务网络环境**: VPN访问、防火墙配置、域名解析
- **第三方服务**: 短信网关API、邮件服务器、时间同步服务
- **开源组件许可**: 确认所有开源组件的许可证兼容性

### Internal Dependencies
- **产品团队**: UI/UX设计稿、原型交互逻辑确认
- **运维团队**: 服务器资源、中间件部署、监控配置
- **业务团队**: 环保标准规范、测试数据、用户验收

### Technical Dependencies
- **基础中间件**: MySQL、Redis、RocketMQ等中间件就绪
- **监控组件**: Prometheus、Grafana、ELK Stack部署
- **容器平台**: Docker/K8s环境和镜像仓库
- **代码仓库**: GitLab/Git仓库和CI/CD工具

## Success Criteria (Technical)

### 性能基准
- **API响应时间**: 95%请求<500ms，99%请求<1s
- **并发处理能力**: 支持100并发用户，1000并发API调用
- **数据处理量**: 日处理300万条监测数据，峰值5000条/分钟
- **系统可用性**: 月可用率≥99.5%，故障恢复时间<15分钟

### 质量门禁
- **代码质量**: SonarQube评分≥A级，代码覆盖率≥75%
- **安全扫描**: 无高危漏洞，中危漏洞修复率≥95%
- **性能测试**: 负载测试通过，内存泄漏检查通过
- **兼容性测试**: 支持主流浏览器，国产化环境测试通过

### 验收标准
- **功能完整性**: PRD中95%功能需求实现
- **用户体验**: 新用户4小时内完成基础操作培训
- **数据准确性**: 数据传输零丢失，ETL处理准确率≥99%
- **文档完整性**: 部署文档、用户手册、API文档齐全

## Estimated Effort

### 总体时间线
- **项目周期**: 6个月（26周）
- **开发时间**: 20周实际开发，6周测试和部署
- **团队配置**: 7人开发团队（2前端+3后端+1测试+1运维）

### 资源需求
- **开发人月**: 约35人月
- **硬件成本**: 测试环境+预生产环境服务器
- **第三方服务**: 短信、邮件、监控等年费约10万元

### 关键路径
1. **数据源适配器开发** (并行开发，风险最高)
2. **ETL引擎核心算法** (复杂度最高，影响后续开发)
3. **前端界面集成** (依赖设计稿，影响用户体验)
4. **性能优化和调试** (部署前关键，影响上线时间)

### 里程碑节点
- **Week 8**: 核心基础功能完成，可演示数据源接入
- **Week 16**: ETL和目录功能完成，可演示端到端流程
- **Week 22**: API和监控功能完成，系统功能完整
- **Week 26**: 测试验收完成，正式部署上线

## Tasks Created
- [ ] 001.md - 基础设施搭建 (parallel: true, effort: L)
- [ ] 002.md - 用户认证系统 (parallel: true, effort: M)
- [ ] 003.md - 数据源管理模块 (parallel: true, effort: L)
- [ ] 004.md - ETL处理引擎 (parallel: true, effort: XL)
- [ ] 005.md - 数据目录系统 (parallel: true, effort: L)
- [ ] 006.md - API服务平台 (parallel: true, effort: L)
- [ ] 007.md - 监控告警系统 (parallel: true, effort: M)
- [ ] 008.md - 前端界面集成 (parallel: true, effort: L)
- [ ] 009.md - 部署配置优化 (parallel: false, effort: M)
- [ ] 010.md - 测试验收交付 (parallel: false, effort: L)

Total tasks: 10
Parallel tasks: 8
Sequential tasks: 2
Estimated total effort: 60-85 days (considering overlap from parallel development)