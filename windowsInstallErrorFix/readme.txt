问题说明：
在 windows 系统的电脑上，
执行 npm install 会报错，错误如下：
------------------------------------------------------------------------------------------------------------
Traceback (most recent call last):
  File "build.py", line 574, in <module>
    test_proc = subprocess.Popen(test_args, stdin=subprocess.PIPE, stdout=subprocess.PIPE)
  File "D:\Python\lib\subprocess.py", line 394, in __init__
    errread, errwrite)
  File "D:\Python\lib\subprocess.py", line 644, in _execute_child
    startupinfo)
WindowsError: [Error 2]
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! scratch-blocks@0.1.0 prepublish: `python build.py && webpack`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the scratch-blocks@0.1.0 prepublish script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
------------------------------------------------------------------------------------------------------------


解决办法，参考：https://github.com/LLK/scratch-blocks/issues/1620
-1 将 windowsInstallErrorFix 目录下的三个文件：blockly_compressed_horizontal.js，blockly_compressed_vertical.js，build.py 复制到项目的根目录下
-2 注意：如果是 mac 和 windows 一起开发，提交代码的时候，不要提交 build.py 文件；mac 下的 build.py 是正常的，不需要修改
-3 重新执行 npm install


说明：
blockly_compressed_horizontal.js，blockly_compressed_vertical.js 这两个文件是 mac 上执行脚本（npm install）得到的
不负责任的猜测：core 目录下的代码会影响上述两个文件。也就是说，只要我们不修改 core 目录下的代码，就可以直接从 mac 上 copy
我们目前没有修改 core 目录下代码的需求，这是 blockly 的核心代码


都改了哪些代码？
build.py 修改了：
331 行改为：proc = subprocess.Popen(args, stdin=subprocess.PIPE, stdout=subprocess.PIPE, shell=True)
574 行改为：test_proc = subprocess.Popen(test_args, stdin=subprocess.PIPE, stdout=subprocess.PIPE, shell=True)
