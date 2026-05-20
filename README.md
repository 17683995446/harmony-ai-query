# 零成本鸿蒙代码AI查询工具 v2.0

把你的鸿蒙项目变成AI能懂的数据库，直接用中文问代码问题。

## 一句话说明

**纯Python实现，零外部依赖（AI查询仅需openai），复制粘贴就能用。**

---

## 准备工作

### 1. 安装Python 3.10+

```bash
python --version
```

### 2. 安装依赖

```bash
pip install openai -i https://mirrors.huaweicloud.com/repository/pypi/simple/
```

> 代码解析器是纯Python实现，不需要安装任何额外的解析库。

### 3. 设置AI API Key（可选但推荐）

```bash
export SILICONFLOW_API_KEY="你的API Key"
```

> 免费注册获取：https://cloud.siliconflow.cn
> 不设置API Key也可以使用，但只能手动输入SQL查询。

---

## 使用方法

### 交互式查询

```bash
python harmony_code_ai.py /你的/鸿蒙/项目/路径
```

### 直接查询

```bash
python harmony_code_ai.py /你的/鸿蒙/项目/路径 --query "列出所有入口页面"
```

### 查看统计

```bash
python harmony_code_ai.py /你的/鸿蒙/项目/路径 --stats
```

### 强制重新解析

```bash
python harmony_code_ai.py /你的/鸿蒙/项目/路径 --rebuild
```

### 命令行参数

| 参数 | 说明 |
|------|------|
| `project` | 鸿蒙项目路径（必填） |
| `--rebuild` | 强制重新解析 |
| `--query, -q` | 直接执行查询 |
| `--stats` | 显示统计信息 |
| `--api-key` | 硅基流动API Key |
| `--model` | AI模型名称（默认: Qwen/Qwen2.5-7B-Instruct） |

---

## 常用查询示例

```
列出所有入口页面
UserService在哪个文件
哪些方法调用了getUserInfo
HomePage包含哪些子组件
所有组件有哪些
哪些类有继承关系
```

也可以直接输入SQL：

```sql
SELECT name, file FROM classes WHERE is_entry = 1
SELECT caller_class, callee FROM calls WHERE callee_class != ''
SELECT parent, child FROM components
```

---

## 数据库结构

工具在项目目录下创建 `.code_insight/code.db`，包含以下表：

| 表名 | 字段 | 说明 |
|------|------|------|
| **files** | path | 文件路径 |
| **classes** | name, file, kind, is_comp, is_entry, extends | 类/结构体/接口/枚举 |
| **methods** | name, class_name, params, return_type | 方法 |
| **calls** | caller, caller_class, callee, callee_class | 调用关系 |
| **components** | parent, child | 组件嵌套关系 |
| **fields** | name, class_name | 字段/状态变量 |
| **imports** | raw, module, components | 导入语句 |

---

## 关键优势

- **零外部解析依赖** - 纯Python ArkTS解析器，不需要安装 arkanalyzer
- **零成本AI** - 使用硅基流动免费API（Qwen2.5-7B）
- **本地运行** - 代码不上传，安全私密
- **SQL安全** - 自动校验AI生成的SQL，防止注入和破坏性操作
- **增量解析** - 支持缓存，避免重复解析
- **命令行工具** - 支持 argparse 参数
- **健壮性** - 完善的异常处理和边界情况处理

---

## 支持的ArkTS特性

- 装饰器：@Entry, @Component, @State, @Prop, @Provide, @Link, @Consume, @Watch, @Builder, @Extend, @Styles, @StorageLink, @StorageProp, @ObjectLink, @Observed, @V2
- 生命周期：aboutToAppear, aboutToDisappear, onPageShow, onPageHide, onBackPress
- 继承和接口：class extends, implements
- 泛型、异步、回调等

---

## 许可证

MIT License
