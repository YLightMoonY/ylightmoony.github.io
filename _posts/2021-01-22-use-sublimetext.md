---
title: Learn python script
layout: post
---

> [sublime国内源](http://packagecontrol.cn/installation)

---

直接control+B无法运行带有input()的代码，所以需要使用sublimeREPL

> [用sublime运行Python](https://www.cnblogs.com/maoxianfei/p/5709229.html)

简单记录下： 

1. control+shift+p-> install package -> sublimeREPL

2. Preferences-->Browse Packages-->SublimeREPL文件夹-->config文件夹-->Python文件夹-->Default.sublime-commands

   找到所需要运行的指令

   ```json
       {
           "caption": "SublimeREPL: Python - RUN current file",
           "command": "run_existing_window_command", "args":
           {
               "id": "repl_python_run",
               "file": "config/Python/Main.sublime-menu"
           }
       },
   ```

3. Preferences-->Key Bindings User

   ```json
   [
   	{
   		"keys" : ["f5"],
           "caption": "SublimeREPL: Python - RUN current file",
           "command": "run_existing_window_command", "args":
           {
               "id": "repl_python_run",
               "file": "config/Python/Main.sublime-menu"
           }
       },
   ]
   ```



