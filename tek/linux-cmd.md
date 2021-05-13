- sed命令——批量修改文件内容
 ```

批量替换单个文件内容

　　命令格式：sed -i 's/旧内容/新内容/g' 文件路径


sed -i 's/oldString/newString/g' file
例如：我想替换cwx.txt文件中的 java 为 linux ,可以使用以下命令：


sed  -i 's/java/linux/g'  cwx.txt
　　

批量替换多个文件内容

　　命令格式：sed -i "s/原内容/新内容/g" `grep 原内容 -rl 所在目录`    注：千万注意这个符号【`】，是【最左上角】那个符号不是单引号


sed -i "s/oldString/newString/g" `grep oldString -rl /path`
例如：我要把/test下所有文件，包含java的替换为linux，可以用以下命令： 


sed -i "s/java/linux/g" `grep java -rl /test`


https://www.cnblogs.com/caoweixiong/p/10234053.html
```
