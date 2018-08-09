Linux常用命令



find

find -name "add_position.py"  //在当前目录及子目录下查找



chown

修改所有者

chown newowner:groupname filename



chmod修改文件读写执行权限

chmod 777 filename



scp传送文件

scp -r file_dir/file_name user@server_ip:dir

sudo scp -r final_competition_data.zip apollo@219.223.236.91:~/