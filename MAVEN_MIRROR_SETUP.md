# Maven阿里云镜像配置指南

为了解决国内网络访问Maven中央仓库速度慢的问题，本项目已配置阿里云Maven镜像站。

## 配置方式

### 方式一：项目级配置（推荐）

项目的 `pom.xml` 文件中已经配置了阿里云镜像，包括：

- **阿里云中央仓库**: https://maven.aliyun.com/repository/central
- **阿里云公共仓库**: https://maven.aliyun.com/repository/public  
- **阿里云Spring仓库**: https://maven.aliyun.com/repository/spring

这种方式的优点是项目自包含，任何人克隆项目后都能自动使用镜像加速。

### 方式二：全局配置

如果您希望在所有Maven项目中都使用阿里云镜像，可以配置全局settings.xml：

1. 复制项目根目录下的 `maven-settings-aliyun.xml` 文件
2. 将其重命名为 `settings.xml` 
3. 放置到以下位置之一：
   - `~/.m2/settings.xml` (用户级配置)
   - `${MAVEN_HOME}/conf/settings.xml` (全局配置)

```bash
# 创建.m2目录（如果不存在）
mkdir -p ~/.m2

# 复制配置文件
cp maven-settings-aliyun.xml ~/.m2/settings.xml
```

## 验证配置

运行以下命令验证镜像配置是否生效：

```bash
# 清理并重新编译项目
mvn clean compile

# 查看实际使用的仓库
mvn help:effective-settings
```

## 可用的阿里云Maven仓库

| 仓库名称 | 仓库地址 | 说明 |
|---------|---------|------|
| central | https://maven.aliyun.com/repository/central | Maven中央仓库镜像 |
| public | https://maven.aliyun.com/repository/public | 公共仓库组合 |
| spring | https://maven.aliyun.com/repository/spring | Spring相关依赖 |
| google | https://maven.aliyun.com/repository/google | Google Maven仓库 |
| gradle-plugin | https://maven.aliyun.com/repository/gradle-plugin | Gradle插件仓库 |
| spring-plugin | https://maven.aliyun.com/repository/spring-plugin | Spring插件仓库 |

## 性能对比

使用阿里云镜像后，依赖下载速度通常可以提升5-10倍：

- **Maven中央仓库**: ~100KB/s (国内网络)
- **阿里云镜像**: ~1-5MB/s (国内网络)

## 故障排除

### 1. 依赖下载失败

如果某些依赖在阿里云镜像中找不到，可以临时禁用镜像：

```bash
mvn clean compile -Dmaven.repo.remote=https://repo1.maven.org/maven2
```

### 2. 清理本地缓存

如果遇到依赖冲突或损坏，清理本地Maven仓库：

```bash
# 删除本地仓库缓存
rm -rf ~/.m2/repository

# 或者只删除特定依赖
rm -rf ~/.m2/repository/org/springframework
```

### 3. 查看详细日志

启用详细日志来诊断问题：

```bash
mvn clean compile -X
```

## 参考资料

- [阿里云Maven仓库官方指南](https://developer.aliyun.com/mvn/guide)
- [Maven官方文档](https://maven.apache.org/guides/)