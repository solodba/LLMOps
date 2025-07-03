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
> ![1.png](https://github.com/solodba/LLMOps/blob/main/day01/images/1.png)  
>  
> ![2.png](https://github.com/solodba/LLMOps/blob/main/day01/images/2.png)  
>  
> ![3.png](https://github.com/solodba/LLMOps/blob/main/day01/images/3.png)  

> 登录终端流程:  
> ![4.png](https://github.com/solodba/LLMOps/blob/main/day01/images/4.png)    
>  
> ![5.png](https://github.com/solodba/LLMOps/blob/main/day01/images/5.png)  
>  
> ![6.png](https://github.com/solodba/LLMOps/blob/main/day01/images/6.png)  

> 修改终端root登录密码:  
> ![7.png](https://github.com/solodba/LLMOps/blob/main/day01/images/7.png) 
>   
> ![8.png](https://github.com/solodba/LLMOps/blob/main/day01/images/8.png)  

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

# conda初始化bash
root@autodl-container-0e1541aaed-4c54cc69:~# 
root@autodl-container-0e1541aaed-4c54cc69:~# conda init bash
no change     /root/miniconda3/condabin/conda
no change     /root/miniconda3/bin/conda
no change     /root/miniconda3/bin/conda-env
no change     /root/miniconda3/bin/activate
no change     /root/miniconda3/bin/deactivate
no change     /root/miniconda3/etc/profile.d/conda.sh
no change     /root/miniconda3/etc/fish/conf.d/conda.fish
no change     /root/miniconda3/shell/condabin/Conda.psm1
no change     /root/miniconda3/shell/condabin/conda-hook.ps1
no change     /root/miniconda3/lib/python3.10/site-packages/xontrib/conda.xsh
no change     /root/miniconda3/etc/profile.d/conda.csh
modified      /root/.bashrc

==> For changes to take effect, close and re-open your current shell. <==

root@autodl-container-0e1541aaed-4c54cc69:~# exec bash
+-----------------------------------------------AutoDL-----------------------------------------------------+
目录说明:
╔═════════════════╦════════╦════╦═════════════════════════════════════════════════════════════════════════╗
║目录             ║名称    ║速度║说明                                                                     ║
╠═════════════════╬════════╬════╬═════════════════════════════════════════════════════════════════════════╣
║/                ║系 统 盘║一般║实例关机数据不会丢失，可存放代码等。会随保存镜像一起保存。               ║
║/root/autodl-tmp ║数 据 盘║ 快 ║实例关机数据不会丢失，可存放读写IO要求高的数据。但不会随保存镜像一起保存 ║
╚═════════════════╩════════╩════╩═════════════════════════════════════════════════════════════════════════╝
CPU ：14 核心
内存：90 GB
GPU ：NVIDIA GeForce RTX 3090, 1
存储：
  系 统 盘/               ：2% 422M/30G
  数 据 盘/root/autodl-tmp：1% 480M/50G
+----------------------------------------------------------------------------------------------------------+
*注意: 
1.系统盘较小请将大的数据存放于数据盘或文件存储中，重置系统时数据盘和文件存储中的数据不受影响
2.清理系统盘请参考：https://www.autodl.com/docs/qa1/
3.终端中长期执行命令请使用screen等工具开后台运行，确保程序不受SSH连接中断影响：https://www.autodl.com/docs/daemon/
(base) root@autodl-container-0e1541aaed-4c54cc69:~# 
```

##### 3. pip安装vllm推理引擎
```
(base) root@autodl-container-0e1541aaed-4c54cc69:~# conda activate /root/autodl-tmp/vllm
(/root/autodl-tmp/vllm) root@autodl-container-0e1541aaed-4c54cc69:~# 
(/root/autodl-tmp/vllm) root@autodl-container-0e1541aaed-4c54cc69:~# pip list
Package    Version
---------- -------
pip        25.1
setuptools 78.1.1
wheel      0.45.1
(/root/autodl-tmp/vllm) root@autodl-container-0e1541aaed-4c54cc69:~# pip install modelscope vllm
(/root/autodl-tmp/vllm) root@autodl-container-0e1541aaed-4c54cc69:~# pip list | grep modelscope
modelscope                               1.27.1
(/root/autodl-tmp/vllm) root@autodl-container-0e1541aaed-4c54cc69:~# pip list | grep vllm
vllm                                     0.9.1
```

##### 4. 下载并启动deepseek-ai/DeepSeek-R1-0528-Qwen3-8B
>国内通过modelscope(魔搭社区)下载大语言模型文件，网址: https://www.modelscope.cn/my/overview  
国外通过Hugging Face下载大语言模型文件，网址: https://huggingface.co  
>  
> 通过modelscope(魔搭社区)网站找到deepseek-ai/DeepSeek-R1-0528-Qwen3-8B大语言模型文件  
> ![9.png](https://github.com/solodba/LLMOps/blob/main/day01/images/9.png)  
> 
> ![10.png](https://github.com/solodba/LLMOps/blob/main/day01/images/10.png)  
>   
> ![11.png](https://github.com/solodba/LLMOps/blob/main/day01/images/11.png)  
>  
> ![12.png](https://github.com/solodba/LLMOps/blob/main/day01/images/12.png)  

通过命令下载deepseek-ai/DeepSeek-R1-0528-Qwen3-8B大语言模型文件
```
# 修改下载文件存放的位置， 默认存放在家目录.cache隐藏文件夹中
(base) root@autodl-container-0e1541aaed-4c54cc69:~# vim .bashrc
# /etc/network_turbo文件是AutoDL平台提供的vpn加速下载文件，提供外网下载加速功能
source /etc/network_turbo
# 设置Hugging Face下载文件的存放位置，不用创建目录，会自动创建
export HF_HOME=/root/autodl-tmp/hf_cache
# 设置modelscope下载文件的存放位置，不用创建目录，会自动创建
export MODELSCOPE_CACHE=/root/autodl-tmp/ms_cache
(base) root@autodl-container-0e1541aaed-4c54cc69:~# exec bash
(base) root@autodl-container-0e1541aaed-4c54cc69:~# conda activate /root/autodl-tmp/vllm
(/root/autodl-tmp/vllm) root@autodl-container-0e1541aaed-4c54cc69:~# VLLM_USE_MODELSCOPE=true vllm serve deepseek-ai/DeepSeek-R1-0528-Qwen3-8B --tensor-parallel-size 1 --max-model-len 32768 --api-key vllm-codehorse --served-model-name vllm-deepseek-1.5 --host 0.0.0.0 --port 8000

# 注释
--tensor-parallel-size	张量并行组的数量, 和显卡个数有关
--max-model-len 模型上下文长度，如未指定，会从模型配置自动推导
--api-key 如果提供，服务器将要求在请求头中提供此密钥
--served-model-name API中使用的模型名称，可提供多个名称
--host 主机名
--port 端口号，默认为 8000

# 可以发现使用max-model-len=32768运行deepseek-ai/DeepSeek-R1-0528-Qwen3-8B大模型报错，如下:
ERROR 07-03 14:17:08 [core.py:515] EngineCore failed to start.
ERROR 07-03 14:17:08 [core.py:515] Traceback (most recent call last):
ERROR 07-03 14:17:08 [core.py:515]   File "/root/autodl-tmp/vllm/lib/python3.11/site-packages/vllm/v1/engine/core.py", line 506, in run_engine_core
ERROR 07-03 14:17:08 [core.py:515]     engine_core = EngineCoreProc(*args, **kwargs)
ERROR 07-03 14:17:08 [core.py:515]                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
ERROR 07-03 14:17:08 [core.py:515]   File "/root/autodl-tmp/vllm/lib/python3.11/site-packages/vllm/v1/engine/core.py", line 390, in __init__
ERROR 07-03 14:17:08 [core.py:515]     super().__init__(vllm_config, executor_class, log_stats,
ERROR 07-03 14:17:08 [core.py:515]   File "/root/autodl-tmp/vllm/lib/python3.11/site-packages/vllm/v1/engine/core.py", line 83, in __init__
ERROR 07-03 14:17:08 [core.py:515]     self._initialize_kv_caches(vllm_config)
ERROR 07-03 14:17:08 [core.py:515]   File "/root/autodl-tmp/vllm/lib/python3.11/site-packages/vllm/v1/engine/core.py", line 145, in _initialize_kv_caches
ERROR 07-03 14:17:08 [core.py:515]     kv_cache_configs = [
ERROR 07-03 14:17:08 [core.py:515]                        ^
ERROR 07-03 14:17:08 [core.py:515]   File "/root/autodl-tmp/vllm/lib/python3.11/site-packages/vllm/v1/engine/core.py", line 146, in <listcomp>
ERROR 07-03 14:17:08 [core.py:515]     get_kv_cache_config(vllm_config, kv_cache_spec_one_worker,
ERROR 07-03 14:17:08 [core.py:515]   File "/root/autodl-tmp/vllm/lib/python3.11/site-packages/vllm/v1/core/kv_cache_utils.py", line 940, in get_kv_cache_config
ERROR 07-03 14:17:08 [core.py:515]     check_enough_kv_cache_memory(vllm_config, kv_cache_spec, available_memory)
ERROR 07-03 14:17:08 [core.py:515]   File "/root/autodl-tmp/vllm/lib/python3.11/site-packages/vllm/v1/core/kv_cache_utils.py", line 572, in check_enough_kv_cache_memory
ERROR 07-03 14:17:08 [core.py:515]     raise ValueError(
ERROR 07-03 14:17:08 [core.py:515] ValueError: To serve at least one request with the models's max seq len (32768), (4.50 GiB KV cache is needed, which is larger than the available KV cache memory (4.47 GiB). Based on the available memory, the estimated maximum model length is 32560. Try increasing `gpu_memory_utilization` or decreasing `max_model_len` when initializing the engine.

报错原因：
当max-model-len设置为32768时，启动起来就需要4.5G的KV缓存，大于可用缓存4.47G了，所以需要把max-model-len调小，程序预测值为32560

# 重新启动大模型
(/root/autodl-tmp/vllm) root@autodl-container-0e1541aaed-4c54cc69:~# VLLM_USE_MODELSCOPE=true vllm serve deepseek-ai/DeepSeek-R1-0528-Qwen3-8B --tensor-parallel-size 1 --max-model-len 32560 --api-key vllm-codehorse --served-model-name vllm-deepseek-1.5 --host 0.0.0.0 --port 8000
```
