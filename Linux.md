# Linux 常用命令笔记

## 磁盘管理

### `df -h`
- **功能**：查看磁盘空间使用情况（人类可读格式）
- **示例**：

## 镜像和容器

### `docker ps -a --filter "name=tao"`
- **功能**：容器名包含tao的容器

### `docker images | grep tao`
- **功能**：镜像名包含tao的镜像

## tmux使用

### `配置`
- **配置示例**：https://gist.github.com/ryerh/14b7c24dfd623ef8edc7
- **编辑**：vim ~/.tmux.conf
- **激活**：tmux source-file ~/.tmux.conf

### `常用命令`