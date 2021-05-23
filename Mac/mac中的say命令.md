---

---







* 查看所有支持的语种

  ```shell
  say -v "?"
  ```

* 让机器说话

  ```shell
  say hello
  ```

* 使用`-f`参数选择朗读的文本文件，然后用`-o`参数将朗读结果存储为某个音频文件

  ```shell
  say -f demo.txt -o demo.aiff
  ```

* 调整语速

  ```shell
  say -r 100 hello
  ```

* 切换语言

  ```shell
  //切换为中文普通话
  say -v Ting-Ting "你好啊"
  ```

  