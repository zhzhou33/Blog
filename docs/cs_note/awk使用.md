- awk内置变量

  | 内置变量           | 解释                              |
  | ------------------ | --------------------------------- |
  | $n                 | 指定分割符后，当前记录的第n个字段 |
  | $0                 | 完成的输入记录                    |
  | FS                 | 字段分割符，默认是空格            |
  | NF(Num of Fields)  | 分割后，当前行一共有多少字段      |
  | NR(Num of Records) | 当前记录数，行数                  |
  | man awk            | 使用帮助                          |
  |                    |                                   |

  ```shell
  # 输出文本第一列，第二列
  awk '{print $1,$2}' alex.txt
  # 输出整行信息
  awk '{print}' alex.txt 
  awk '{print $0}' alex.txt
  ```

  | -F   | 指定分割字段                |
  | ---- | --------------------------- |
  | -v   | 定义或修改一个wak内部的变量 |
  | -f   | 从脚本中读取awk命令         |
  |      |                             |

  ```shell
  # 显示文件第二行
  awk 'NR==2' pwd.txt
  # 显示文件2-5行
  awk 'NR==2,NR==5' pwd.txt
  # 以:为分隔符显示文件第一列，倒数第二列，最后一列
  awk -F ':' '{print $1,$(NF-1),$NF}' pwd.txt
  ```

- awk分割符有两种

  - 输入分割符，默认空格、空白符号，英文field separator，变量是FS
  - 输入分割符，out field separator 简称OFS

  ```shell
  # 指定#作为分割符，输出第一列、第三列
  awk -F '#' '{print $1, $3}' text.txt
  awk -v FS='#' '{print $1, $3}' text.txt
  # 输入以#作为分割符，输出以---作为分割符，输出一三列
  awk -v FS='#' -v OFS='---' '{print $1,$3 }'
  ```

- awk输出分割符

  - awk是否存在分隔符，在于`{print $1,$3 }`逗号的区别

    ```shell
    # 添加逗号，以空格分隔
    awk -v FS='#' '{print $1,$3 }' test.txt
    # 不加逗号，不间隔
    awk -v FS='#' '{print $1$3 }' test.txt
    ```

| 内置变量 | 解释                         |
| -------- | ---------------------------- |
| FS       | 指定输入分隔符               |
| OFS      | 指定输出分隔符               |
| RS       | 指定输入的换行符             |
| ORS      | 指定输出的换行符             |
| NF       | 当前行字段的个数             |
| NR       | 行号，当前处理的文本行的行号 |
| FNR      | 各文件分别计数的行号         |
| FILENAME | 当前文件名                   |

- 内置变量不用使用$符号

  ```shell
  # 输出每行行号，以及字段总个数
  awk '{print NR,NF}' test.txt
  # 输出每行行号，以及指定的列
  awk '{print NR,$1,$3}' test.txt
  
  # 处理多个文件显示行号, FNR会对每个文件分别记录行号
  awk '{print FNR,$0}' alex.txt pwd.txt
  
  # RS自定义空格作为行分隔符
  awk -v RS=' ' '{print NR,$0}' test.txt
  
  # ORS是默认输出换行符，我们可以更改换行符，以下命令将文本中的换行符替换为@@@
  awk -v ORS='@@@' '{print NR,$0}' test.txt
  
  # 显示正在处理的文件名
  awk '{print FILENAME,FNR,$0}' test.txt
  ```

- 自定义变量

  - 使用`-v varNme=value`

    ```shell
    # 同时输出自定义变量和文本内容
    awk -v name="hello" 'BEGIN{print name}{print $0}' alex.txt
    hello
    alex1 alex2 alex3 alex4 alex5
    alex6 alex7 alex8 alex9 alex10
    ```

  - ```shell
    awk 'BEGIN{h="hello";w="world"; print h, w}'
    hello world
    ```

  - 间接引用shell变量

    ```shell
    hw=helloworld
    awk -v myvar=$hw 'BEGIN{print myvar}'
    helloworld
    ```

    

