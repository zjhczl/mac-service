要在macOS上开机自动执行特定的命令，如启动`frpc`服务，你可以创建一个`launchd`服务。以下是详细步骤：

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
