# jupyter notebook 配置

参考：https://blog.csdn.net/simple_the_best/article/details/77005400



密码：apollo



粗略配置过程：

```shell
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~$ jupyter notebook --generate-config
Writing default config to: /home/apollo/.jupyter/jupyter_notebook_config.py
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~$ cd ~/.jupyter/
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~/.jupyter$ ll
total 36
drwx------ 2 apollo apollo  4096 7月   3 10:20 ./
drwxr-xr-x 6 apollo apollo  4096 7月   3 10:20 ../
-rw-rw-r-- 1 apollo apollo 26863 7月   3 10:20 jupyter_notebook_config.py
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~/.jupyter$ jupyter notebook password
Enter password: 
Verify password: 
[NotebookPasswordApp] Wrote hashed password to /home/apollo/.jupyter/jupyter_notebook_config.json
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~/.jupyter$ sudo vim /home/apollo/.jupyter/jupyter_notebook_config.py
[sudo] password for apollo: 
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~/.jupyter$ sudo vim /home/apollo/.jupyter/jupyter_notebook_config.json
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~/.jupyter$ sudo vim /home/apollo/.jupyter/jupyter_notebook_config.py
```



/home/apollo/.jupyter/jupyter_notebook_config.json中内容：

```
{
  "NotebookApp": {
    "password": "sha1:85c1ad02ff38:3d9a0080844ab8d1fafc381ab3acd15d63dd6c5a"
  }
}
```



后台运行:

```
nohup jupyter notebook
```

