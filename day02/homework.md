### 一. 安装部署Miniconda，熟练进行virtual env管理
#### 1. 下载Miniconda安装包
> 本次Miniconda的安装环境是win10  
> Miniconda下载地址: https://www.anaconda.com/docs/getting-started/miniconda/install  
>
> 下载windows版本的Miniconda  
> ![1.png](https://github.com/solodba/LLMOps/blob/main/day02/images/1.png) 
>
> ![2.png](https://github.com/solodba/LLMOps/blob/main/day02/images/2.png)  
```
# 使用win+R快捷键，输入powershell，打开powershell终端
# 本次下载安装包路径为D:\
wget "https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe" -outfile "D:\Miniconda3-latest-Windows-x86_64.exe"
```

#### 2. 安装Miniconda
> 创建安装目录D:\miniconda
> ![3.png](https://github.com/solodba/LLMOps/blob/main/day02/images/3.png)  
>   
> ![4.png](https://github.com/solodba/LLMOps/blob/main/day02/images/4.png)  
>   
> ![5.png](https://github.com/solodba/LLMOps/blob/main/day02/images/5.png)  
>   
> ![6.png](https://github.com/solodba/LLMOps/blob/main/day02/images/6.png)  
>   
> ![7.png](https://github.com/solodba/LLMOps/blob/main/day02/images/7.png)  
>   
> ![8.png](https://github.com/solodba/LLMOps/blob/main/day02/images/8.png)  
>   
> ![9.png](https://github.com/solodba/LLMOps/blob/main/day02/images/9.png)  
>   
> ![10.png](https://github.com/solodba/LLMOps/blob/main/day02/images/10.png)  
>   
> ![11.png](https://github.com/solodba/LLMOps/blob/main/day02/images/11.png)  

#### 3. 配置Miniconda环境变量
> ![12.png](https://github.com/solodba/LLMOps/blob/main/day02/images/12.png)  
> 
> ![13.png](https://github.com/solodba/LLMOps/blob/main/day02/images/13.png)  
> 
> ![14.png](https://github.com/solodba/LLMOps/blob/main/day02/images/14.png)  
> 
> ![15.png](https://github.com/solodba/LLMOps/blob/main/day02/images/15.png)  
>  

初始化conda环境
```
# 使用win+R快捷键，输入powershell，打开powershell终端
PS C:\Users\Administrator> conda -V
conda 25.5.1

PS C:\Users\Administrator> conda init --system --all
WARNING conda.common.path.windows:_path_to(100): cygpath is not available, fallback to manual path conversion
WARNING conda.common.path.windows:_path_to(100): cygpath is not available, fallback to manual path conversion
WARNING conda.common.path.windows:_path_to(100): cygpath is not available, fallback to manual path conversion
no change     D:\miniconda\Scripts\conda.exe
no change     D:\miniconda\Scripts\conda-env.exe
no change     D:\miniconda\Scripts\conda-script.py
no change     D:\miniconda\Scripts\conda-env-script.py
no change     D:\miniconda\condabin\conda.bat
no change     D:\miniconda\Library\bin\conda.bat
no change     D:\miniconda\condabin\_conda_activate.bat
no change     D:\miniconda\condabin\rename_tmp.bat
no change     D:\miniconda\condabin\conda_auto_activate.bat
no change     D:\miniconda\condabin\conda_hook.bat
no change     D:\miniconda\Scripts\activate.bat
no change     D:\miniconda\condabin\activate.bat
no change     D:\miniconda\condabin\deactivate.bat
modified      D:\miniconda\Scripts\activate
modified      D:\miniconda\Scripts\deactivate
modified      D:\miniconda\etc\profile.d\conda.sh
modified      D:\miniconda\etc\fish\conf.d\conda.fish
no change     D:\miniconda\shell\condabin\Conda.psm1
modified      D:\miniconda\shell\condabin\conda-hook.ps1
no change     D:\miniconda\Lib\site-packages\xontrib\conda.xsh
modified      D:\miniconda\etc\profile.d\conda.csh
modified      /etc/profile.d/conda.sh
modified      C:\Users\Administrator\.config\fish\config.fish
modified      C:\ProgramData\xonsh\xonshrc
modified      C:\Windows\System32\WindowsPowerShell\v1.0\profile.ps1
modified      HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun
no change     HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem\LongPathsEnabled

==> For changes to take effect, close and re-open your current shell. <==
```

#### 4. 使用conda创建virtual env
```
## 使用win+R快捷键，输入powershell，打开powershell终端
## 查看所有virtual env
(base) PS C:\Users\Administrator> conda env list

# conda environments:
#
base                 * D:\miniconda

## 创建路径D:\venvs\llm的虚拟环境
(base) PS C:\Users\Administrator> conda create -p 'D:\venvs\llm' python=3.12
(base) PS C:\Users\Administrator> conda env list

# conda environments:
#
base                 * D:\miniconda
                       D:\venvs\llm
```

### 二. 部署JupyterLab，可以熟练使用JupyterLab编辑、运行Python程序
```
## 在虚拟路径为D:\venvs\llm的虚拟环境中部署JupyterLab
(base) PS C:\Users\Administrator> conda env list

# conda environments:
#
base                 * D:\miniconda
                       D:\venvs\llm

(base) PS C:\Users\Administrator> conda activate D:\venvs\llm
(D:\venvs\llm) PS C:\Users\Administrator>
(D:\venvs\llm) PS C:\Users\Administrator> conda env list

# conda environments:
#
base                   D:\miniconda
                     * D:\venvs\llm

## 使用conda安装jupyterlab和jupyter
(D:\venvs\llm) PS C:\Users\Administrator> conda install jupyterlab
(D:\venvs\llm) PS C:\Users\Administrator> conda install jupyter

## 使用pip安装jupyter中文包
(D:\venvs\llm) PS C:\Users\Administrator> pip install jupyterlab-language-pack-zh-CN
```