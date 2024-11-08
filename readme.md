要在macOS上开机自动执行特定的命令，如启动`frpc`服务，你可以创建一个`launchd`服务。以下是详细步骤：
## 需要登录
### 步骤1：创建Plist配置文件

1. 打开终端（Terminal）应用程序。
2. 使用文本编辑器创建一个新的`.plist`文件，例如`com.zj.frpc.plist`：

   ```bash
   nano ~/Library/LaunchAgents/com.zj.frpc.plist
   ```

3. 在打开的编辑器中输入以下内容，确保替换路径为你的实际路径：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
       <key>Label</key>
       <string>com.zj.frpc</string>
       <key>ProgramArguments</key>
       <array>
           <string>/Users/zj/server/frp/frpc</string>
           <string>-c</string>
           <string>/Users/zj/server/frp/frpc.ini</string>
       </array>
       <key>RunAtLoad</key>
       <true/>
       <key>KeepAlive</key>
       <true/>
       <key>StandardErrorPath</key>
       <string>/Users/zj/server/frp/frpc.err</string>
       <key>StandardOutPath</key>
       <string>/Users/zj/server/frp/frpc.out</string>
   </dict>
   </plist>
   ```

   - `Label`：服务的唯一标识符。
   - `ProgramArguments`：要执行的命令和参数。
   - `RunAtLoad`：设置为`true`表示在加载时运行服务。
   - `KeepAlive`：设置为`true`表示保持服务运行，即使命令执行完成。
   - `StandardErrorPath`和`StandardOutPath`：设置日志文件路径，用于记录服务的输出和错误。

4. 保存并退出编辑器（在nano中按`Ctrl+X`，然后按`Y`确认保存）。

### 步骤2：加载服务

在终端中运行以下命令来加载服务：

```bash
launchctl load ~/Library/LaunchAgents/com.zj.frpc.plist
```

### 步骤3：启动服务

如果你想立即启动服务，可以使用以下命令：

```bash
launchctl start com.zj.frpc
```

### 步骤4：验证服务状态

使用以下命令查看服务的状态：

```bash
launchctl list | grep frpc
```

### 步骤5：查看日志

你可以通过查看`StandardErrorPath`和`StandardOutPath`指定的日志文件来获取服务的输出和错误信息：

```bash
cat /Users/zj/server/frp/frpc.err
cat /Users/zj/server/frp/frpc.out
```

通过这些步骤，你可以在macOS上创建一个开机自启动的服务来执行`frpc`命令。这样可以确保每次开机时自动启动`frpc`服务。


## 不需要登录
如果你想将服务写成`LaunchDaemons`，这意味着服务将在系统启动时自动启动，而不需要用户登录。以下是创建`LaunchDaemons`服务的步骤：

### 步骤1：创建Plist配置文件

1. 打开终端（Terminal）应用程序。
2. 使用文本编辑器创建一个新的`.plist`文件，例如`com.zj.frpc.plist`：

   ```bash
   sudo nano /Library/LaunchDaemons/com.zj.frpc.plist
   ```

   使用`sudo`是因为`/Library/LaunchDaemons/`目录通常需要管理员权限。

3. 在打开的编辑器中输入以下内容，确保替换路径为你的实际路径：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
       <key>Label</key>
       <string>com.zj.frpc</string>
       <key>ProgramArguments</key>
       <array>
           <string>/Users/zj/server/frp/frpc</string>
           <string>-c</string>
           <string>/Users/zj/server/frp/frpc.ini</string>
       </array>
       <key>RunAtLoad</key>
       <true/>
       <key>KeepAlive</key>
       <true/>
       <key>StandardErrorPath</key>
       <string>/Users/zj/server/frp/frpc.err</string>
       <key>StandardOutPath</key>
       <string>/Users/zj/server/frp/frpc.out</string>
   </dict>
   </plist>
   ```

   - `Label`：服务的唯一标识符。
   - `ProgramArguments`：要执行的命令和参数。
   - `RunAtLoad`：设置为`true`表示在加载时运行服务。
   - `KeepAlive`：设置为`true`表示保持服务运行，即使命令执行完成。
   - `StandardErrorPath`和`StandardOutPath`：设置日志文件路径，用于记录服务的输出和错误。

4. 保存并退出编辑器（在nano中按`Ctrl+X`，然后按`Y`确认保存）。

### 步骤2：加载服务

在终端中运行以下命令来加载服务：

```bash
sudo launchctl load /Library/LaunchDaemons/com.zj.frpc.plist
```

### 步骤3：启动服务

如果你想立即启动服务，可以使用以下命令：

```bash
sudo launchctl start com.zj.frpc
```

### 步骤4：验证服务状态

使用以下命令查看服务的状态：

```bash
sudo launchctl list | grep frpc
```

### 步骤5：查看日志

你可以通过查看`StandardErrorPath`和`StandardOutPath`指定的日志文件来获取服务的输出和错误信息：

```bash
cat /Users/zj/server/frp/frpc.err
cat /Users/zj/server/frp/frpc.out
```

### 停止和删除服务

如果你需要停止或删除这个`LaunchDaemons`服务，可以使用以下命令：

停止服务：

```bash
sudo launchctl stop com.zj.frpc
```

卸载服务：

```bash
sudo launchctl unload /Library/LaunchDaemons/com.zj.frpc.plist
```

删除.plist文件：

```bash
sudo rm /Library/LaunchDaemons/com.zj.frpc.plist
```

通过这些步骤，你可以在macOS上创建一个开机自启动的服务来执行`frpc`命令，并且可以通过`launchctl`命令来管理这个服务。
