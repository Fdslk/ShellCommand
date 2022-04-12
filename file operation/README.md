# file operation

## definition

* 什么是Linux的文件系统？
* cat
  * cat
    * -n 对输出的内容加上行号
    * -b 对输出的内容（除空白行外）都加上行号
    * /dev/null > [file name] 清空当前file的内容
* chgrp 改变文件或目录的所属群组
  * -v/-c 改变当前文件的群组
    * eg：如果想将当前目录下的README文件移动到bin group中
      * chgrp -c bin REMDME.md
      * ![error notification](https://user-images.githubusercontent.com/6279298/162874839-056be677-25a3-4789-947b-0fc1a96f1865.png)
      * 查看当前用户的所属的用户组 ```group```
      ![当前用户所属用户组](https://user-images.githubusercontent.com/6279298/162874947-1d422664-7729-45e5-8aa5-9e4603b63419.png)
      * 运行一下命令将用户添加至指定用户组

      ```
      sudo dseditgroup -o edit -a username -t user groupname
      ```

      * 使用场景
        * 用户权限隔离
        * **咋实现的~可以深究一下**

  * -R 循环递归处理，将当前目录下的其他的文件采用同样的处理
* chmod **(change mode)**
  * r - 4, w - 2, x - 1

| user      | group | other user     |
| :---        |    :----:   |          ---: |
|  u     | g       | o   |

* chown **(chanage owner)**
  * tip: 必须root用户才能执行该命令
* 比较两个文件的不同
* diff
  * diff
  ```
  diff README.md ../README.md -y -W 50
  ```

  ![并排比较](https://user-images.githubusercontent.com/6279298/162893983-ea9da932-b846-4b49-bc1c-e3c22d0caf3c.png)
  * tips：**|** 表示同行有不同，**<** 后文件比前文件少一行，**>**后文件比前文件多一行

  * diff file1 file2 | diffstat, 统计两个文件的变化字数

  * cmp
    * 输出两个文件第一处不相同的位置
![diff](https://user-images.githubusercontent.com/6279298/162893460-62b9b0be-b466-48b0-833a-c02bd7bca2c1.png)

  * file: 检索文件类型
    * -b 只输出文件类型，不输出文件名称
    * -i 输出文件MIME
  * find 查找文件，并执行bash命令
    * syntax:
    ```
    find   path   -option   [   -print ]   [ -exec   -ok   command ]   {} \;
    ```

    * -mindepth 1 -maxdepth 1 指定文件查找深度
    * 应用
      * 利用find命令执行多repository的git pull
      ```
      find . -mindepth 1 -maxdepth 1 -type d -print -exec git -C {} pull \;
      ```
      * 寻找当前目录下子目录（最小，最大深度为1），文件类型为directory，**{}** 作为find命令的输出目录，**;** 作为 -exec的终结符，**/** 作为Semicolon的转义字符
  * ln **(link file)** 使文件和一固定的文件目录建立链接，然后就可以直接使用该文件，避免了文件的拷贝，减少重复的磁盘占用
    * ```ln -s filename link name```
    ![soft link](https://user-images.githubusercontent.com/6279298/162903067-a272cf38-8d12-4a4b-84de-bb17677eaa46.png)
    * 怎么实现的??
