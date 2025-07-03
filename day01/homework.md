#### 一、使用vLLM部署deepseek-ai/DeepSeek-R1-0528-Qwen3-8B

##### 1. 购买服务器
> 由于本人电脑显卡显存偏低，所以在AutoDL算力云上购买1台服务器即可。
> 购买地址: https://autodl.com/home   

> 购买配置:    
> CPU: 14 核，Xeon(R) Gold 6330   
> GPU: RTX 3090(24GB) * 1  
> 内存: 90 GB  
> 系统盘: 30 GB  
> 数据盘: 50 GB  
> GPU驱动: 570.124.04  
> CUDA版本: 11.8   
> 系统: ubuntu22.04  

> 购买流程：  
![1.png](https://github.com/solodba/LLMOps/blob/main/day01/images/1.png)  
![2.png](https://github.com/solodba/LLMOps/blob/main/day01/images/2.png)  
![3.png](https://github.com/solodba/LLMOps/blob/main/day01/images/3.png)  

> 登录终端流程:  
![4.png](https://github.com/solodba/LLMOps/blob/main/day01/images/4.png)    
![5.png](https://github.com/solodba/LLMOps/blob/main/day01/images/5.png)  
![6.png](https://github.com/solodba/LLMOps/blob/main/day01/images/6.png)  

> 修改终端root登录密码:  
![4.png](https://github.com/solodba/LLMOps/blob/main/day01/images/7.png)  
![4.png](https://github.com/solodba/LLMOps/blob/main/day01/images/8.png)   
```
和上面图形界面修改一样的密码
root@autodl-container-0e1541aaed-4c54cc69:~# passwd root
New password: 
Retype new password: 
passwd: password updated successfully
root@autodl-container-0e1541aaed-4c54cc69:~# 
root@autodl-container-0e1541aaed-4c54cc69:~# 

这样就可以使用mobaxterm、securecrt等其他登录软件登录了。
```

##### 2. conda安装python虚拟环境
```
# 安装python 3.11的虚拟环境vllm, 主要负责运行和管理vllm
root@autodl-container-0e1541aaed-4c54cc69:~# conda --version
conda 22.11.1
root@autodl-container-0e1541aaed-4c54cc69:~# 
root@autodl-container-0e1541aaed-4c54cc69:~# df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          30G   54M   30G   1% /
/dev/nvme1n1    7.0T  3.7T  3.3T  53% /autodl-pub
AutoFS:fs1      4.0T  1.4T  2.6T  35% /autodl-pub/data
tmpfs            64M     0   64M   0% /dev
shm              45G     0   45G   0% /dev/shm
/dev/sda2       440G   17G  401G   4% /usr/bin/nvidia-smi
tmpfs           378G   12K  378G   1% /proc/driver/nvidia
tmpfs           378G  4.0K  378G   1% /etc/nvidia/nvidia-application-profiles-rc.d
tmpfs           378G     0  378G   0% /proc/asound
tmpfs           378G     0  378G   0% /proc/acpi
tmpfs           378G     0  378G   0% /proc/scsi
tmpfs           378G     0  378G   0% /sys/firmware
tmpfs           378G     0  378G   0% /sys/devices/virtual/powercap
root@autodl-container-0e1541aaed-4c54cc69:~# 
root@autodl-container-0e1541aaed-4c54cc69:~# conda create -p /root/autodl-tmp/vllm python=3.11

# 安装python 3.11的虚拟环境open-webui, 主要负责运行和管理open-webui
root@autodl-container-0e1541aaed-4c54cc69:~# conda create -p /root/autodl-tmp/open-webui python=3.11

# 查看和确认安装的两个python虚拟环境
root@autodl-container-0e1541aaed-4c54cc69:~# cd /root/autodl-tmp/
root@autodl-container-0e1541aaed-4c54cc69:~/autodl-tmp# ls
open-webui  vllm
root@autodl-container-0e1541aaed-4c54cc69:~/autodl-tmp# 
```
