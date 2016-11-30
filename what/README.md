# EEGLAB 能干什么？

上一部分讲了学习 EEGLAB 所需要的准备工作，这一部分先谈谈 EEGLAB 提供了什么功能，能满足我们在脑电数据分析过程中的哪些需求。

EEGLAB 工具包自身的提供的解决方法主要包括 ERP（Event-related potentials）和 ERO（event-related oscillations）的分析，其特色为利用 ICA（Independent component analysis）对脑电信号和噪音进行分离，实现了较好的去噪效果。EEGLAB 的长处不在于源分析以及涉及到连接性的分析，但通过一些插件提供了相关支持。EEGLAB 本身功能较为单一，其强大之处在于给其他开发或者研究者提供了插件接口，从而丰富了其应用面，形成了一个小生态。

基本的脑电分析可以通过 EEGLAB 的 GUI 手动操作实现，但是其灵活性和工作效率相当低，一般来说不推荐手动操作。这也是本教程的目的，尝试去讲解如何利用 EEGLAB 内置函数和 MATLAB 的基本函数和流程控制实现脑电数据分析的自动化。

## 1. EEGLAB 支持多种原始数据格式

脑电设备多种多样，EEGLAB 能很好的支持常见的原始数据格式。支持的数据格式详细目录参考[文档](https://sccn.ucsd.edu/wiki/A1:_Importing_and_Exporting_Data_(Continuous_Epoched_Data)。其中，有一些 EEGLAB 没提供支持的数据格式可以通过 Biogig toolbox 得到支持。导入原始数据的函数为 `pop_load*.m` 或者 `eeg_load*.m`。补充说明一下，`pop_` 开头的函数其实是可以调出对话框进行手动操作的函数，如果在命令行不带任何参数，将会弹出对话窗，让你手动选择或者输入参数。然后，再调用底层一些的函数进行操作。而 `eeg_` 前缀的函数不会弹出任务窗。有些原始数据可能无法直接通过 EEGLAB 导入，这时可能需要先转换为 EEGLAB 或者 MATLAB 能够读取的格式。例如 EGI 的脑电数据格式就需要先转换为 Netstation binary file 文件格式（`*.raw`），然后才能导入至 EEGLAB 分析。

## 2. EEGLAB 提供了灵活的预处理方案

EEGLAB 很好的支持常见的预处理步骤如降采样率、滤波、重参考、去伪迹、分段（或者剖分）。并且每一个步骤都有对应的函数。常见的函数位于 `eeglab/fucntions/popfunc/` 路径下。下面是常见的预处理的函数和对应的功能的一个表格。表格仅作为实例给初学者以参考，对 EEGLAB 的常见函数有个初步认识。

EEGLAB 内置函数 | 功能 | 实例
:--------------|:-----|:-----
`pop_resample()` | 重采样 | `EEG = pop_resample(EEG, 250);`
`pop_reref()` | 重参考 | `EEG = pop_reref(EEG, []); % 基于所有通道的平均参考`
`pop_chanedit()` | 编辑通道空间位置文件 | `EEG = pop_chanedit(EEG, 'lookup', dirLocFile); % 导入位置文件`
`pop_eegfiltnew()`* | 滤波 | `EEG = pop_eegfiltnew(EEG, 0.01, []); % 0.01Hz 高通滤波`
`pop_epoch()` | 分段 | `EEG = pop_epoch(EEG, eventSelect, timeRange);`
`pop_select()` | 选择或删除数据 | `EEG = pop_select(EEG, 'nochannel', channelIndex); % 删除通道`
`pop_rejepoch()` | 删除 epoch | `EEG = pop_rejepoch(EEG, trialLogical, 0);`
`pop_rmbase()` | 基线矫正 | `EEG = pop_rmbase(EEG, baselineTimeRange);`
`pop_runica()` | ICA | `EEG = pop_runica(EEG, 'extended', 1, 'pca', Rank);`

## 3. EEGLAB 方便你直观地浏览数据

分析数据必须了解数据，知道数据“长”什么样。`Plot>Channel data (scroll)` 可以随时浏览数据，这是从时间维度去看数据长什么样。还可以绘制所有通道的数据的功率谱，从频率维度去看数据长什么样（`Plot>Channel spectra and maps`）。[Chapter 03: Plotting Channel Spectra and Maps](https://sccn.ucsd.edu/wiki/Chapter_03:_Plotting_Channel_Spectra_and_Maps) EEGLAB 的在线教程详细说明了这个功能。浏览数据看起来简单枯燥但又十分重要的一件事情。对于每一个被试的数据整体状况如何，你可以在线记录时进行观察和记录下来。离线分析时，仍然有必要观察数据的面貌。对数据形成一个大概的印象，有助于选择更合理的预处理的策略。

---

待续

## 4. EEGLAB 支持高性能并行计算

## 5. EEGLAB 提供了基本的可视化方案

## 6. EEGLAB 提供了组分析方案

## 7. EEGLAB 提供了源分析方案

## 8. EEGLAB 提供了基本的统计功能

