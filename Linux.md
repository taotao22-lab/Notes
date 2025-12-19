# Linux 常用命令笔记

## 1. 文件操作

### `df -h`
- **功能**：查看磁盘空间使用情况（人类可读格式）
- **示例**：

### `ls -l | wc -l`
- **功能**：查看当前目录下文件和目录总数（不包括隐藏项）
- **查看指定文件夹**：`ls -l 文件夹路径 | wc -l`

### `ln -s <source> <target>`
- **功能**：创建指向源文件/目录的软链接
- **删除链接**：`rm <target>`（不影响源文件）

## 2. 镜像和容器

### `docker ps -a --filter "name=tao"`
- **功能**：容器名包含tao的容器

### `docker images | grep tao`
- **功能**：镜像名包含tao的镜像

### `docker commit my_container my_image:1.0`
- **功能**：将容器保存为镜像

## 3. tmux使用

### 插件配置
- **配置示例**：https://gist.github.com/ryerh/14b7c24dfd623ef8edc7
- **编辑**：`vim ~/.tmux.conf`
- **激活**：`tmux source-file ~/.tmux.conf`

### 会话管理
- `tmux new -s <session-name>` 创建新会话
- `tmux attach -t <session-name>` 进入指定会话
- `tmux detach` 退出当前会话（会话在后台继续运行）
- `tmux kill-session -t <session-name>` 关闭指定会话
- `tmux switch -t <session-name>` 切换会话
- `tmux rename-session -t <old-name> <new-name>` 重命名会话
- `tmux ls` 查看所有会话

### 窗口操作
- `Ctrl+b c` 创建新窗口
- `Ctrl+b &` 关闭当前窗口
- `Ctrl+b w` 窗口列表
- `Ctrl+b p` 切换到上一个窗口
- `Ctrl+b n` 切换到下一个窗口
- `Ctrl+b <number>` 切换到指定编号窗口
- `Ctrl+b ,` 重命名当前窗口

### 窗格操作
- `Ctrl+b %` 垂直分割窗格
- `Ctrl+b "` 水平分割窗格
- `Ctrl+b <arrow>` 按方向键切换窗格
- `Ctrl+b x` 关闭当前窗格
- `Ctrl+b z` 最大化/恢复当前窗格
- `Ctrl+b Ctrl+<arrow>` 调整窗格大小
- `Ctrl+b Space` 切换窗格布局
- `Ctrl+b ;` 切换到上一个活动的窗格

### 其他常用
- `Ctrl+b d` 分离会话（后台运行）
- `Ctrl+b [` 进入复制模式（用方向键移动，q退出）
- `Ctrl+b t` 显示时钟
- `Ctrl+b ?` 查看所有快捷键
- `Ctrl+b :` 进入命令模式