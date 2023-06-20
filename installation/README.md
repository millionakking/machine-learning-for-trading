# 安装说明

没有必要一次尝试安装所有库，因为这会增加遇到版本冲突的可能性。相反，我们建议您在学习过程中安装特定章节所需的库。


> 2022 年 3 月更新：`zipline-reloaded`、`pyfolio-reloaded`、`alphalens-reloaded` 和 `empyrical-reloaded` 现已在 `conda-forge` 频道上可用。频道 `ml4t` 只包含过时的版本，很快就会被删除。

> 对 M1/Silicone 芯片的 MacOS 的支持仍然不完整. 一些与新架构兼容的包只能通过 `conda`/`mamba` 获得，其他的只能通过 `pip` 获得. 因此，目前还没有单一的安装脚本——我希望随着 PyData 生态系统支持的成熟能够简化它. 现在，请创建单独的基于 `conda`/`pip` 的环境来根据需要和支持安装包。
 
> 2021 年 9 月 10 日更新：适用于 `pip`（Linux、MacOS）和 `conda`（Linux、MacOS、Windows）安装的新操作系统无关环境文件 `ml4t-base.[txt, yml]` 包括最新的 [ Zipline](https://github.com/stefan-jansen/zipline-reloaded)、[Alphalens](https://github.com/stefan-jansen/alphalens-reloaded) 和 [Pyfolio](https://github .com/stefan-jansen/pyfolio-reloaded) 版本. 这些文件与操作系统无关，因为它们仅包含主库而不包含特定于操作系统的依赖项，将最新兼容版本和特定于操作系统的依赖项的选择留给您选择的包管理器.   

> 2021 年 4 月 25 日更新：[新 Zipline 版本](https://github.com/stefan-jansen/zipline-reloaded) 允许在所有操作系统上运行没有 Docker 的回测笔记本；安装说明现在参考 Windows/MacOS/Linux 环境文件. 

> 2021 年 3 月 14 日更新：我刚刚发布了一个在 Python 3.7-3.9 上运行的 [新 Zipline 版本](https://github.com/stefan-jansen/zipline-reloaded)；请参阅[发布信息](https://github.com/stefan-jansen/zipline-reloaded/releases/tag/2.0.0rc4) 和[文档](https://zipline.ml4trading.io/)。因此，Docker 解决方案将不再是必需的，我将在 4 月份提供新的环境文件。


> 2021 年 2 月 26 日更新：2.0 版将环境数量减少到 2，并将主要 `ml4t` 的 Python 版本提高到 3.8，将 `backtest` 环境提高到 3.6。
> 下面的说明反映了这些变化。
> 
> 要将 Docker 映像更新到最新版本，请运行：
> ```docker pull appliedai/packt:latest```

本书使用 Python 3.8 和各种可以安装的 ML 和交易相关的库：

1. 在 [conda 环境](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage) 中使用 [mamba](https://github.com/mamba-org/mamba) -environments.html) 基于 [Miniconda](https://docs.conda.io/en/latest/miniconda.html) 发行版和提供的 `ml4t.yml` 环境文件，
   - 如果您遇到操作系统特定文件的问题，请改用“installation/ml4t-base.yml”文件。
   - 运行指令:
   - ```bash
     conda create -n ml4t python=3.8
     mamba env update -n ml4t -f ml4t-base.yml
     conda activate ml4t
     ```
2. 仅适用于 macOS 和 Linux：在使用 [pyenv](https://github.com/pyenv/) 创建的 Python 虚拟环境中通过 [pip](https://pip.pypa.io/en/stable/) pyenv) 或 [venv](https://docs.python.org/3/tutorial/venv.html) 使用提供的 `ml4t.txt` 需求文件。
3. 弃用：使用 [Docker](https://www.docker.com/) Desktop 从 [Docker Hub](https://www.docker.com/products/docker-hub) 拉取镜像并创建本地容器使用运行笔记本电脑所需的软件。

我们将描述如何获取源代码，然后依次列出前两个选项。然后，我们介绍如何使用 [Jupyter](https://jupyter.org/) notebook 来查看和执行代码示例。最后，我们列出了遗留的 Docker 安装说明。

## 获取代码示例

您可以通过下载 [GitHub 存储库](https://github.com/stefan-jansen/machine-learning-for-trading) 的压缩版本或通过 [克隆](https:// www.howtogeek.com/451360/how-to-clone-a-github-repository/) 其内容。后者将导致更大的下载量，因为它包含提交历史记录。

或者，您可以创建存储库的 [fork](https://guides.github.com/activities/forking/)，并在克隆其内容后从那里继续开发。

要在本地使用代码，请执行以下操作：
1. 选择您要存储代码和数据的文件系统位置。
2. 使用 `ssh` 或 `https` 链接或 [GitHub 存储库](https://github.com/stefan-jansen/machine-learning-for-trading) 上绿色 `Code` 按钮提供的下载选项，将代码克隆或解压缩到目标文件夹。
    - 克隆github库, 切换到新目录运行 `git clone https://github.com/stefan-jansen/machine-learning-for-trading.git`.
    - 如果你克隆了 repo 并且没有重命名它，根目录将被称为`machine-learning-for-trading`, 如果是压缩包解压后的根目录为`machine-learning-for-trading-master`.

   
## 如何使用“conda”环境安装所需的库

这些说明依赖于 Anaconda 的 [miniconda](https://docs.conda.io/en/latest/miniconda.html) 发行版，[mamba](https://github.com/mamba-org/mamba) 包管理器以促进依赖管理，以及 `installation/[windows|macos|linux]/ml4t.yml` 中特定于操作系统的环境文件以及固定的库版本。

或者，还有一个环境文件“installation/ml4t-base.yml”，它只包含一个没有依赖项的所需库列表；如果您改用此文件，您将获得最新版本 - 请注意，在某些时候更新的软件可能与示例不兼容。

您也可以只安装您感兴趣的笔记本所需的软件包；最新版本（截至 2021 年 3 月）应该可以使用。

### 安装 miniconda

这些notebooks依赖于基于 [miniconda3](https://docs.conda.io/en/latest/miniconda.html) 的单一虚拟环境。

您可以在 [此处](https://conda.io/projects/conda/en/latest/user-guide/install/index.html) 找到各种操作系统的详细说明。

### 从环境文件创建 conda 环境

[conda] 是 [Anaconda](https://www.anaconda.com/) python 发行版提供的包管理器。不幸的是，它目前 [状态不是很好](https://github.com/conda/conda/issues/9707)。相反，我们将使用更新更快的 [mamba](https://github.com/mamba-org/mamba) 包管理器来安装包。您可以使用以下方式安装它：
```python
conda install -n base -c conda-forge mamba
```

使用笔记本中使用的最新版本库创建[虚拟环境](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) (自 2021 年 4 月起），您只需从克隆存储库根目录中的命令行运行以下选项之一（取决于您的操作系统）：

```bash
conda env create --name ml4t python=3.8
mamba env update -n ml4t -f installation/windows/ml4t.yml 
mamba env update -n ml4t -f installation/macosx/ml4t.yml # deprecated; use ml4t-base.yml
mamba env update -n ml4t -f installation/linux/ml4t.yml 
```

另请参阅[此处](https://towardsdatascience.com/getting-started-with-python-environments-using-conda-32e9f2779307)，了解有关虚拟环境的更详细教程。

如果您想在阅读本文时使用最新的库版本创建一个新环境，请运行

```bash
conda env create -f installation/ml4t-base.yml
```

### Activate conda environment

创建它之后，您可以使用它的名称激活虚拟运行环境，在我们的例子中是“ml4t”：

```bash
conda activate ml4t
```

如要停用，只需使用

```bash
conda deactivate
```

## 使用 pip 安装库

您应该在 [虚拟环境](https://realpython.com/python-virtual-environments-a-primer/) 中安装所需的库。请参阅内置 [venv](https://docs.python.org/3/library/venv.html) 选项或 [pyenv](https://github.com/pyenv/pyenv) 的文档允许您并行运行多个 Python 版本的替代方案。

一些库需要预先安装特定于操作系统的软件，这可能取决于您机器的状态。我们在下面列出了一些常见的情况。如果遇到其他问题，请查阅导致问题的库的文档。如果这不能解决问题，请在我们的 GitHub 上提出问题，以便我们查看并相应地更新此处的说明。

### 先决条件：MacOS

MacOS 的安装需要以下可以通过 [homebrew](https://brew.sh/) 安装的库：
```bash
brew install lightgbm swig xz ta-lib
```

### 先决条件：Linux

在 Ubuntu 上，可以通过“apt”满足先决条件。对于 TA-Lib，[必要步骤](https://artiya4u.medium.com/installing-ta-lib-on-ubuntu-944d8ca24eae) 是：

```bash
# 安装构建工具
sudo apt install build-essential wget -y

# 下载并解压源代码
wget https://artiya4u.keybase.pub/TA-lib/ta-lib-0.4.0-src.tar.gz
tar -xvf ta-lib-0.4.0-src.tar.gz

# 配置源代码和构建。
cd ta-lib/
./configure --prefix=/usr
make

# 安装到系统
sudo make install
```

### 安装要求

假设您已经创建并激活了一个虚拟环境，您只需要运行（取决于您的操作系统）：
```bash
pip install -U pip setuptools wheel
pip install -r installation/macosx/ml4t.txt # for macOS; deprecated; use ml4t-base.txt
pip install -r installation/linux/ml4t.txt # for Ubuntu
```

## 安装后说明


### 获取 QUANDL API 密钥

要下载我们在下一步中用于全书多个示例的美国股票数据，请[注册](https://www.quandl.com/sign-up) 个人 Quandl 帐户以获取 API 密钥.它将显示在您的[个人资料](https://www.quandl.com/account/profile) 页面上。

如果您使用的是基于 UNIX 的系统，例如 Mac OSX，您可能希望将 API 密钥存储在环境变量中，例如 QUANDL_API_KEY，例如通过将 export QUANDL_API_KEY=<your_key> 添加到您的 .bash_profile 中。

### 提取 Zipline 数据

要运行 Zipline 回测，我们需要“提取”数据。有关更多信息，请参阅[新手教程](https://zipline.ml4trading.io/beginner-tutorial.html)。

默认情况下，Zipline 将数据存储在 ~/.zipline 目录下的用户目录中。

在命令提示符下，激活您的 `ml4t` 虚拟环境并运行：
```bash
zipline ingest -b quandl
``` 

当 Zipline 会处理大约 3,000 个股票价格系列，您应该会看到大量消息（包括一些您可以忽略的警告）。


### 使用 Jupyter 笔记本

本节介绍如何设置有助于在此环境中工作的笔记本扩展，以及如何将笔记本转换为 python 脚本（如果愿意）。

#### 设置 jupyter 扩展

jupyter notebooks 可以使用社区提供的一系列[扩展名](https://github.com/ipython-contrib/jupyter_contrib_nbextensions)。 [文档](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/) 中描述了许多有用的内容。

此 repo 中的笔记本被格式化为使用 [目录 (2)](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/nbextensions/toc2/README.html) 扩展。为了获得最佳体验，请在启动 jupyter 服务器后使用浏览器中可用的 [Nbextensions](https://github.com/Jupyter-contrib/jupyter_nbextensions_configurator) 选项卡中的配置器激活它。如果默认情况下未设置，请修改设置以选中选项“Leave h1 items out of ToC”。

#### 将 jupyter 笔记本转换为 python 脚本

本书使用 [jupyter](https://jupyter.org/) notebooks 来呈现带有大量注释和上下文信息的代码，并促进在一个地方可视化结果。一些代码示例更长，更适合作为 `python` 脚本运行；您可以通过在命令行上运行以下命令将笔记本转换为脚本：

```bash
$ jupyter nbconvert --to script [YOUR_NOTEBOOK].ipynb
```

## 遗留说明：使用 Docker 容器运行笔记本

Docker Desktop 是 MacOS 和 Windows 机器上非常流行的应用程序，因为它允许跨不同操作系统轻松共享容器化应用程序。对于本书，我们有一个 Docker 镜像，让您可以实例化一个容器来运行 Ubuntu 20.04 作为来宾操作系统，并预装 [conda 环境](https://docs.conda.io/projects/conda/en/latest /user-guide/tasks/manage-environments.html) 在 Windows 10 或 Mac OS X 上，无需担心对主机的依赖性。

### 安装 Docker 桌面

与往常一样，Mac OS X 和 Window 10 的安装有所不同，Windows 10 Home 需要额外的步骤才能启用虚拟化。

我们将分别介绍每个操作系统的安装，然后解决两种情况下必要的一些设置调整。

#### Mac OS X 上的 Docker 桌面

在 Mac OS X 上安装 Docker Desktop 非常简单：
1. 按照 Docker [文档](https://docs.docker.com/docker-for-mac/install/) 中的详细指南，从 [Docker Hub](https://hub.docker) 下载并安装 Docker Desktop .com/editions/community/docker-ce-desktop-mac/）。它还涵盖了 Docker Desktop 和 Docker Toolbox 如何 [可以共存](https://docs.docker.com/docker-for-mac/docker-toolbox/)。
2. 按照 [此处](https://aspetraining.com/resources/blog/docker-on-mac-homebrew-a-step-by-step-tutorial）。

打开终端并运行以下测试以检查 Docker 是否正常工作：
```Docker
docker run hello-world
```

查看适用于 Mac OS 的[入门](https://docs.docker.com/docker-for-mac/) 指南，熟悉关键设置和命令。

#### Windows 上的 Docker 桌面

Docker Desktop 适用于 Windows 10 家庭版和专业版；家庭版需要启用虚拟机平台的额外步骤。

##### Windows 10 家庭版：启用虚拟机平台

您现在可以使用 [适用于 Linux 的 Windows 子系统](https://fossbytes.com/what-is-windows-subsystem-for-linux-wsl/) (WSL 2) 后端在 Windows Home 计算机上安装 Docker Desktop。 Windows Home 上的 Docker Desktop 是用于 Linux 容器开发的完整版 Docker Desktop。

Windows 10 家庭版机器必须满足某些 [要求](https://docs.docker.com/docker-for-windows/install-windows-home/#system-requirements)。其中包括 Windows 10 家庭版 2004（2020 年 5 月发布）或更高版本。 Docker Desktop Edge 版本还支持 Windows 10 版本 1903 或更高版本。

按照[此处](https://docs.microsoft.com/en-us/windows/wsl/install-win10)所述启用 WSL 2，执行以下步骤：

1. 启用可选的适用于 Linux 的 Windows 子系统功能。以管理员身份打开 PowerShell 并运行：
    ```bash
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```
2. 检查您的系统是否满足[此处](https://docs.microsoft.com/en-us/windows/wsl/install-win10#requirements) 列出的要求，并在必要时更新您的 Windows 10 版本。
3. 通过以管理员身份打开 PowerShell 并运行来启用虚拟机平台可选功能：
    ```bash
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```
4. 重新启动计算机以完成 WSL 安装并更新到 WSL 2。
5. 下载并运行 Linux 内核 [更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)。系统将提示您提升权限，选择“是”以批准此安装。
6. 通过以管理员身份打开 PowerShell 并运行以下命令，在安装新的 Linux 发行版时将 WSL 2 设置为默认版本：
    ```bash
    wsl --set-default-version 2
    ```
  
##### Windows 10：Docker 桌面安装

一旦我们为 Windows Home 启用了 WSL 2，安装 Docker Desktop 的其余步骤对于 Windows 10 [Home](https://docs.docker.com/docker-for-windows/install-windows-home/) 是相同的和 [专业版、企业版或教育版](https://docs.docker.com/docker-for-windows/install-windows-home/)。有关系统要求，请参阅每个操作系统版本的链接指南。

1. 从 [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-windows/) 下载并运行（双击）安装程序。
2. 出现提示时，确保在配置页面上选择启用 Hyper-V Windows 功能选项。
3. 按照安装向导的说明授权安装程序并继续安装。
4. 安装成功后，点击关闭完成安装过程。
5. 如果您的管理员帐户与您的用户帐户不同，您必须将用户添加到 docker-users 组。以管理员身份运行计算机管理并导航到本地用户和组 > 组 > docker-users。右键单击以将用户添加到组中。注销并重新登录以使更改生效。

打开 Powershell 并运行以下测试以检查 Docker 是否正常工作：
```Docker
docker run hello-world
```

查看适用于 Windows 的[入门](https://docs.docker.com/docker-for-windows/) 指南，熟悉关键设置和命令。

### Docker 桌面设置：内存和文件共享

上面提到的每个操作系统的入门指南描述了 Docker 桌面设置。

#### 增加内存 

- 在首选项下，查找资源以了解如何增加分配给容器的内存；考虑到数据的大小，默认设置太低。增加到至少 4GB，最好是 8GB 或更多。                                                                                                                            
- 有几个示例非常占用内存，例如第 2 章中的 NASDAQ 报价数据和 SEC 文件示例，并且需要显着更高的内存分配。

#### 文件共享权限疑难解答

我们会将代码示例和数据下载到主机操作系统上的本地驱动器，但通过将本地驱动器安装为卷来从 Docker 容器运行它。这应该适用于当前版本，但如果您收到**权限错误**，请参阅 Docker 用户指南中的**文件共享**部分。 Docker GUI 允许您显式分配权限。另请参阅（稍微过时的）解释 [此处](https://rominirani.com/docker-on-windows-mounting-host-directories-d96f3f056a2c)。
  
### 获取代码示例

您可以通过下载 [GitHub 存储库](https://github.com/stefan-jansen/machine-learning-for-trading) 的压缩版本或通过 [克隆](https:// www.howtogeek.com/451360/how-to-clone-a-github-repository/) 其内容。后者将导致更大的下载量，因为它包含提交历史记录。


或者，您可以创建存储库的 [fork](https://guides.github.com/activities/forking/)，并在克隆其内容后从那里继续开发。

要在本地使用代码，请执行以下操作：
1. 选择您要存储代码和数据的文件系统位置。
2. 使用 `ssh` 或 `https` 链接或 [GitHub 存储库](https://github.com/stefan-jansen/machine-learning-for-trading) 上绿色 `Code` 按钮提供的下载选项)，将代码克隆或解压缩到目标文件夹。
    - 要克隆入门仓库，请运行 `git clone https://github.com/stefan-jansen/machine-learning-for-trading.git` 并切换到新目录。
    - 如果你克隆了 repo 并且没有重命名它，根目录将被称为 `machine-learning-for-trading`，ZIP 版本将解压到 `machine-learning-for-trading-master`。

### 获取 QUANDL API 密钥

要下载我们将在下一步中用于全书多个示例的美国股票数据，请[注册](https://www.quandl.com/sign-up) 个人 Quandl 帐户以获取 API 密钥.它将显示在您的[个人资料](https://www.quandl.com/account/profile) 页面上。

如果您使用的是基于 UNIX 的系统，例如 Mac OSX，您可能希望将 API 密钥存储在环境变量中，例如 QUANDL_API_KEY，例如通过将 export QUANDL_API_KEY=<your_key> 添加到您的 .bash_profile 中。

### 下载 Docker 镜像并运行容器

我们将使用基于 Ubuntu 20.04 操作系统和 [Anaconda](https://www.anaconda.com/ ) 的 [miniconda](https://docs.conda.io/en/latest/miniconda.html) 安装了 Python 发行版。它带有下面描述的两个 conda 环境。

使用单个 Docker 命令，我们可以同时完成几件事（有关更多详细信息，请参阅上面链接的入门指南）：
- 仅在第一次运行时：从 Docker Hub 帐户“appliedai”和带有“latest”标签的存储库“packt”中拉取 Docker 镜像
- 创建一个名为 `ml4t` 的本地容器并以交互模式运行它，转发 `jupyter` 服务器使用的端口 8888
- 将包含启动项目文件的当前目录作为卷安装在容器内的目录“/home/packt/ml4t”中
- 将环境变量 `QUANDL_API_KEY` 设置为您的密钥值（您需要为 `<your API key>` 填写），以及
- 在容器内启动一个 `bash` 终端，为用户 `packt` 生成一个新的命令提示符。

1. 打开终端或 Powershell 窗口。
2. 导航到包含您在上面获取的 [ML4T](https://github.com/stefan-jansen/machine-learning-for-trading) 代码示例的目录。
3. 在本地版本repo的根目录下，运行以下命令，同时考虑到Mac和Windows要求的路径格式不同：
    - **Mac OS**：您可以将 `pwd` 命令用作包含当前工作目录的绝对路径的 shell 变量（如果您在上一步中创建了这样的环境变量，则可以使用 `$QUANDL_API_KEY` ):
        ```docker
        docker run -it -v $(pwd):/home/packt/ml4t -p 8888:8888 -e QUANDL_API_KEY=<your API key> --name ml4t appliedai/packt:latest bash
        ```
   - **Windows**：输入当前目录的绝对路径**带正斜杠**，例如`C:/Users/stefan/Documents/machine-learning-for-trading` 而不是 `C:\Users\stefan\Documents\machine-learning-for-trading`, 在Windows中命令为：                                                                                                                                                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                                                                                                                                                                  
     ```docker
     docker run -it -v C:/Users/stefan/Documents/machine-learning-for-trading:/home/packt/ml4t -p 8888:8888 -e QUANDL_API_KEY=<your API key> --name ml4t appliedai/packt:latest bash
     ```              
4. 从容器 shell 运行 exit 以退出并停止容器。
5. 要恢复工作，您可以从 Mac OS 终端或 Windows Powershell 在根目录中运行 `docker start -a -i ml4t` 以重新启动容器并以交互模式将其附加到主机 shell（有关更多详细信息，请参阅 Docker 文档).

> 要将 Docker 映像更新到最新版本，请运行：
> ```docker pull appliedai/packt:latest```

### 从容器运行notebooks

现在您在容器内运行一个 shell，并且可以访问两个 [conda 环境](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)。运行 `conda env list` 以查看有 `base`、`ml4t`（默认）和 `backtest` 环境。

`backtest` 环境是必需的，因为最新版本的 Zipline 1.4.1 仅支持 Python 3.6 和其他各种依赖项的旧版本，这些依赖项部分也需要编译。我希望将来更新 Zipline 以在 Python 3.8 上运行。

除了使用 Zipline 直接输入由 Zipline 生成的十几个与回溯测试相关的笔记本外，我们使用环境“ml4t”。需要“回测”环境的笔记本包含通知。

> 如果您想为深度学习示例使用 GPU，如果您有合适的 [CUDA 版本](https://www.tensorflow.org/install/source#gpu)，则可以运行 `conda install tensorflow-gpu`安装。
> **或者**，您可以利用 [TensorFlow 的 Docker](https://www.tensorflow.org/install/docker) 图像并在那里安装任何其他库； DL 示例不需要安装任何过于复杂的东西。

- 由于 [nb_conda_kernels](https://github.com/Anaconda-Platform/nb_conda_kernels) 扩展（见下文），您可以使用 `conda activate <env_name>` 或使用 Jupyter Notebook 或 Jupyter Lab Kernel 菜单切换到另一个环境).
- 您可能会看到一条错误消息，提示您运行 `conda init bash`。这样做之后，使用命令 source .bashrc 重新加载 shell。

### 摄取 Zipline 数据

要运行 Zipline 回测，我们需要“摄取”数据。有关更多信息，请参阅[新手教程](https://zipline.ml4trading.io/beginner-tutorial.html)。

该图像已配置为将数据存储在您启动容器的目录中的 .zipline 目录中（该目录应该是您在上面下载的入门代码的根文件夹）。

在容器 shell 的命令提示符下，运行
```bash
conda activate backtest
zipline ingest -b quandl
``` 
Zipline 会处理大约 3,000 个股票价格系列，您应该会看到大量消息。

#### 已知的 Zipline 问题

> 我已经在 [最新的 Zipline 版本](https://github.com/stefan-jansen/zipline/commit/b33e5c955a58d888f55101874f45cd141c61d3e1) 中修复了以下国家代码问题，因此您不必再手动摆弄资产数据库.

运行回测时，您可能会遇到[错误](https://github.com/quantopian/zipline/issues/2517)，因为当前的 Zipline 版本需要在 `assets-7 的交换表中输入国家代码条目.sqlite 数据库，用于存储资产元数据。

链接的 [GitHub 问题](https://github.com/quantopian/zipline/issues/2517) 描述了如何通过打开 [SQLite 数据库](https://sqlitebrowser.org/dl/) 并输入`来解决这个问题US` 在 exchanges 表的 `country_code` 字段中。

实际上，这看起来如下：

1. 使用 [SQLite 浏览器](https://sqlitebrowser.org/dl/) 在包含最新包下载的目录中打开文件 `assets-7.sqlite`。如果您按照刚刚描述的方式运行命令，路径将如下所示（在 Linux/Max OSX 上）：`~/machine-learning-for-trading/data/.zipline/data/quandl/2020-12-29T02;06; 08.894865/`
2. 选择表“exchanges”，如下图所示：
<p align="center">
<img src="https://i.imgur.com/khq6gtX.png" title="Modifying QUANDL SQLite - Step 1" width="50%"/>
</p>
3. 输入国家代码并保存结果（关闭程序时会有提示）：
<p align="center">
<img src="https://i.imgur.com/mtdiylk.png" title="Modifying QUANDL SQLite - Step 1" width="50%"/>
</p>

就这样。不幸的是，你（必须..）每次运行 `zipline ingest -b quandl` 时都要重复这个。当您为默认的 quantopian-quandl 包运行 zipline ingest 时，此错误仍然会发生，因为此命令绕过 ingest 过程并下载由早期版本的 Zipline 生成的结果的压缩版本。

### 在 Docker 容器中使用notebooks

您可以使用传统的 [notebook](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html) 或更新的 [Jupyter Lab](https://jupyterlab.readthedocs.io/en/stable/) 界面；两者都适用于所有“conda”环境。此外，由于 `nb_conda_kernels` 包，您从 `base` 环境启动 jupyter 并从 notebook 切换环境（参见 [docs](https://github.com/Anaconda-Platform/nb_conda_kernels)。

要开始，请运行以下两个命令之一：
```bash
jupyter notebook --ip 0.0.0.0 --no-browser --allow-root
jupyter lab --ip 0.0.0.0 --no-browser --allow-root
```
每个都有“别名”快捷方式，因此您不必键入它们：
- `nb` for the `jupyter notebook` version, and 
- `lab` for the `jupyter lab` version.

容器终端将在启动 jupyter 服务器时显示一些消息。完成后，它将显示一个 URL，您应该将其粘贴到浏览器中以从当前工作目录访问 jupyter 服务器。

您可以使用下面概述的标准 conda 工作流程修改任何环境；请参阅 Docker [文档](https://docs.docker.com/storage/)，了解如何在进行更改后保留容器。
