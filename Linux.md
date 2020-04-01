### 文件操作 ###
ls 
 + -a 显示全部文件 
 + -l 详情显示
 + - 组合命令
 
 mkdir
 + -p 参数
 
 cd
 + . 当前目录
 + ../ 上一级目录
 
 rm
 + rm -r tmp/ 递归删除文件/文件夹
 
 mv
 + mv run.py tmp/ 
 + mv run.py run2.py 重命名

 touch 创建文件
 
 cp copy
 + cp run2.py run3.py
 
 cat
 + cat run2.py 显示内容
 + cat run2.py > run3.py 导入文件/覆盖
 + cat run2.py >> run3.py 追加文件
 
 echo "123" > run2.py 写入显示字符串
 
 sudo 有权限执行
 + sudo command not found 命令非系统内置
 
 ### 进阶 ###
 
 grep
 + 搜索文件内容
 + ps -a | grep bash
 
 ifconfig
 + 找到ip(ifconfig | grep inet)
 
 vim <br/>
 :wq 退出
 
 shell<br/>
 vim run.sh<br/>
 + chmod +x ./run.sh 执行脚本  ./run.sh

### 服务器交互 ###
wget 下载网页

scp 
 + 下载文件 scp user@ip:server_path local_path
 + 下载目录 scp -r user@ip:server_path local_path
 + 上传文件 scp local_path user@ip:server_path
 
 
