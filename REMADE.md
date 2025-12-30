# Java内存马检测工具 (Memory Shell Detector)

一款用于检测、查看和移除Java应用中内存马（Memory Shell）的安全工具。

## 功能特性

### 检测能力

#### 容器型内存马

- **Tomcat**: Filter、Servlet、Listener、Valve
- **Jetty**: Filter、Servlet、Handler
- **WebLogic**: Filter、Servlet
- **JBoss/WildFly (Undertow)**: Filter、Servlet
- **Resin**: Filter、Servlet

#### 框架型内存马

- **Spring MVC/Spring Boot**: Controller、Interceptor
- **Shiro**: Filter

#### Agent型内存马

- 冰蝎 (Behinder)
- 哥斯拉 (Godzilla)
- 蚁剑 (AntSword)
- 天蝎 (Suo5)
- 通过 Instrumentation/ClassFileTransformer 修改的类

### 检测原理

1. **无class文件检测**: 动态注入的内存马在磁盘上没有对应的.class文件
2. **类名关键字检测**: evil、shell、cmd、backdoor、hack、exploit、payload、behinder、godzilla、antsword、memshell、webshell、inject、malicious、trojan、reverse
3. **字节码特征检测**: Runtime.exec、ProcessBuilder、ScriptEngine、ClassLoader.defineClass、JNDI lookup、反序列化等危险调用
4. **类路径检测**: 不在标准jar/war/classes目录中的类

### 操作功能

- **进程扫描**: 自动发现系统中运行的Java进程
- **代码反编译**: 查看可疑类的源代码，支持语法高亮
- **内存马移除**: 支持移除 Filter、Servlet、Listener、Valve、Controller、Interceptor
- **报告导出**: 支持JSON和文本格式的检测报告

## 环境要求

- JDK 1.8+

## 快速开始


编译完成后会生成以下JAR文件：

- `detector-cli/target/memory-shell-detector-cli.jar` - 命令行工具
- `detector-gui/target/memory-shell-detector-gui.jar` - 图形界面工具
- `detector-agent/target/detector-agent-1.0.0-SNAPSHOT.jar` - Java Agent

**注意**: Agent JAR 需要和 CLI/GUI JAR 放在同一目录下使用。

### 图形界面使用

```bash
java -jar memory-shell-detector-gui.jar
```

GUI界面功能：

1. 左侧面板显示Java进程列表，双击可扫描
2. 右上面板显示检测结果，按类型分组（Filter/Servlet/Listener/Valve/Controller/Interceptor/Agent等）
3. 可疑内存马以红色高亮显示
4. 右下面板显示反编译代码
5. 工具栏提供刷新、扫描、查看、移除、导出等操作

### 命令行使用

#### 列出Java进程

```bash
java -jar memory-shell-detector-cli.jar -l
```

#### 扫描指定进程

```bash
java -jar memory-shell-detector-cli.jar -s <PID>
```

#### 查看可疑类代码

```bash
java -jar memory-shell-detector-cli.jar -v <类名> -p <PID>
```

#### 移除内存马

```bash
java -jar memory-shell-detector-cli.jar -r <类名> -p <PID>
```

#### 导出检测报告

```bash
# JSON格式
java -jar memory-shell-detector-cli.jar --report report.json -p <PID> -f json

# 文本格式
java -jar memory-shell-detector-cli.jar --report report.txt -p <PID> -f text
```

## 命令行参数

| 参数                  | 说明                 |
| --------------------- | -------------------- |
| `-l, --list`          | 列出所有Java进程     |
| `-s, --scan <PID>`    | 扫描指定进程         |
| `-v, --view <类名>`   | 查看类的反编译代码   |
| `-r, --remove <类名>` | 移除指定的内存马     |
| `-p, --pid <PID>`     | 指定目标进程ID       |
| `--report <文件>`     | 导出检测报告         |
| `-f, --format <格式>` | 报告格式: json/text  |
| `--hex`               | 以十六进制显示字节码 |
| `--force`             | 强制执行（跳过确认） |
| `--verbose`           | 显示详细输出         |
| `-h, --help`          | 显示帮助信息         |

## 支持移除的内存马类型

| 类型        | 支持移除                        |
| ----------- | ------------------------------- |
| Filter      | ✅                               |
| Servlet     | ✅                               |
| Listener    | ✅                               |
| Valve       | ✅                               |
| Controller  | ✅                               |
| Interceptor | ✅                               |
| Agent       | ❌ (Agent型无法移除，需重启应用) |

## 风险等级

| 等级     | 说明                     |
| -------- | ------------------------ |
| CRITICAL | 确认为内存马，需立即处理 |
| HIGH     | 高度可疑，建议人工确认   |
| MEDIUM   | 中等风险，可能是正常组件 |
| LOW      | 低风险，基本可排除       |
| SAFE     | 安全，已在白名单中       |


## 注意事项

1. **权限要求**: 扫描其他进程需要足够的系统权限
2. **JDK版本**: 目标进程和检测工具需使用相同或兼容的JDK版本
3. **移除风险**: 移除内存马可能影响应用正常运行，建议先备份
4. **Agent型**: Agent型内存马无法通过本工具移除，需要重启应用
5. **重启Tomcat**: 修改Agent后需要重启Tomcat才能生效（Agent会被JVM缓存）

## 许可证

MIT License
