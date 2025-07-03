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


##### 2. 使用conda安装python虚拟环境
