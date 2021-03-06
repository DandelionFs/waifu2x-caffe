



作者：lltcggie

该软件是通过使用[Caffe](http://caffe.berkeleyvision.org/)仅重写图像转换软件“ [waifu2x](https://github.com/nagadomi/waifu2x) ”的转换功能 而为Windows构建的软件。它可以由CPU转换，但是如果您使用CUDA（或cuDNN），则转换速度可以比CPU快。

GUI支持英语，日语，简体中文，繁体中文，韩语，土耳其语，西班牙语，俄语和法语。

您可以[从此版本页面](https://github.com/lltcggie/waifu2x-caffe/releases)上的“下载”部分下载软件。

## 所需环境

至少需要以下环境才能运行此软件。

- 操作系统：Windows Vista或更高版本的64位（没有32位exe）
- 内存：可用内存1GB或更多（但是，取决于要转换的图像大小）
- GPU：具有Compute Capability 3.0或更高版本的NVIDIA GPU（与CPU转换时不需要）
- 必须安装Microsoft Visual C ++ 2015可再发行组件包更新3（x64版本）（必需）
  - [点击这里获取](https://www.microsoft.com/ja-jp/download/details.aspx?id=53587)套餐
  - `ダウンロード`按下按钮后，`vcredist_x64.exe`选择下载并安装。
  - 如果找不到，请尝试搜索“ Visual C ++ 2015可再发行组件包更新3”。

使用cuDNN转换时

- GPU：具有Compute Capability 3.0或更高版本的NVIDIA GPU

如果您想了解GPU的计算能力，请检查[此页面](https://developer.nvidia.com/cuda-gpus)。

## 关于cuDNN

cuDNN是用于高速机器学习的库，只能与NVIDIA GPU一起使用。您可以不使用cuDNN而使用CUDA进行转换，但是使用cuDNN具有以下优点。

- 根据所用GPU的类型，可以更快地转换图像
- 可以减少VRAM的使用（至少少于CUDA的一半。差异越大，划分大小越大）

尽管cuDNN具有这样的优点，但是由于许可证的原因，无法分发操作所需的文件。
因此，如果要使用cuDNN，请从此[页面](https://developer.nvidia.com/cuDNN)下载Windows二进制文件（v5.1 RC或更高版本），并将“ cudnn64_7.dll”放入waifu2x-caffe文件夹中。
如果在软件运行时插入dll，请重新启动软件。
（要下载cuDNN，您需要向NVIDIA开发人员和CUDA注册的开发人员进行注册。CUDA的注册开发人员可能会有一个（简单的）评论，因此，如果注册，您将无法立即下载cuDNN。空无一人。）

作者环境中处理速度和VRAM使用情况的测量结果如下。

- GPU：GTX 980 Ti
- VRAM：6GB
- 处理内容：1000 * 1000 PNG 4ch图像噪声消除和扩展，JPEG噪声消除等级1，扩展率2.00，未使用TTA模式
- 处理时间测量方法：CUI版本测量平均处理时间10次。但是，该过程在开始时预先执行了两次（为了不包括初始化所需的时间）
- VRAM使用率计算方法：（GUI版本在处理过程中使用的最大VRAM）-（启动GUI版本后的VRAM使用率）

cuDNN RGB模型

| 分区大小 | 处理时间       | VRAM使用量（MB） |
| -------- | -------------- | ---------------- |
| 100      | 00：00：03.170 | 278              |
| 125      | 00：00：02.745 | 279              |
| 200      | 00：00：02.253 | 365              |
| 250      | 00：00：02.147 | 446              |
| 500      | 00：00：01.982 | 1110             |

CUDA RGB模型

| 分区大小 | 处理时间       | VRAM使用量（MB）     |
| -------- | -------------- | -------------------- |
| 100      | 00：00：06.192 | 724                  |
| 125      | 00：00：05.504 | 724                  |
| 200      | 00：00：04.642 | 1556                 |
| 250      | 00：00：04.436 | 2345                 |
| 500      | 无法估量       | 无法测量（超过6144） |

cuDNN Up RGB模型

| 分区大小 | 处理时间       | VRAM使用量（MB） |
| -------- | -------------- | ---------------- |
| 100      | 00：00：02.831 | 328              |
| 125      | 00：00：02.573 | 329              |
| 200      | 00：00：02.261 | 461              |
| 250      | 00：00：02.150 | 578              |
| 500      | 00：00：01.991 | 1554             |

CUDA Up RGB模型

| 分区大小 | 处理时间       | VRAM使用量（MB）     |
| -------- | -------------- | -------------------- |
| 100      | 00：00：03.669 | 788                  |
| 125      | 00：00：03.382 | 787                  |
| 200      | 00：00：02.965 | 1596                 |
| 250      | 00：00：02.852 | 2345                 |
| 500      | 无法估量       | 无法测量（超过6144） |

## 使用方法（GUI版本）

“ Waifu2x-caffe.exe”是一个GUI软件。通过双击开始。或者，使用资源管理器将文件或文件夹拖放到“ waifu2x-caffe.exe”，然后将使用上次启动时的设置执行转换。在这种情况下，取决于设置，如果转换成功，则对话框将自动关闭。您也可以使用GUI在命令行上设置选项。有关详细信息，请参见命令行选项（公用）和命令行选项（GUI版本）部分。

启动后，将图像或文件夹拖放到“输入路径”字段中以自动设置“输出路径”字段。如果要更改输出目标，请更改“输出路径”列。

您可以根据自己的喜好更改转换设置。

## 输入/输出设置

 与文件输入/输出有关的一组设置项目。





### “输入路径”

指定要转换的文件的路径。

如果指定了文件夹，则将转换带有“要在该文件夹中转换的扩展名”（包括子文件夹）的文件。

您可以通过拖动指定多个文件和文件夹。

在这种情况下，文件和文件夹结构将输出到新文件夹中。

（在输入路径列中，显示“（（多文件）”）。输出文件夹名称由您用鼠标保持的文件和文件夹名称生成。）

单击浏览按钮选择文件时，可以选择一个文件，一个文件夹或多个文件。





### “输出路径”

指定保存转换后的图像的路径。

在“输入路径”中指定文件夹后，转换后的文件将保存在指定的文件夹中（不更改文件夹结构）。 如果指定的文件夹不存在，它将自动创建。





### “扩展名以在文件夹中转换”

如果“输入路径”是文件夹，请在文件夹中指定要转换图像的扩展名。

预设值为`png：jpg：jpeg：tif：tiff：bmp：tga`。

分隔符为`：`。

大小写无所谓。

Example.png：jpg：jpeg：tif：tiff：bmp：tga





### “输出扩展”

指定转换图像的格式。

根据此处指定的格式，可为``输出图像质量设置''和``输出深度比特率''设置的值会有所不同。





### “输出图像质量设置”

指定转换后图像的质量。

可以设置的值为整数。

可以指定的值的范围和含义取决于在``输出扩展''中设置的格式。

* .jpg：值范围（0到100），数字越大，图像质量越高。
* .webp：值的范围（1到100）数字越大，图像质量就越高100
* .tga：值范围（0到1）0表示不压缩，1表示RLE压缩





### “输出深度位”

指定转换图像每个通道的位数。

可以指定的值取决于在“输出扩展名”中设置的格式。





## 转换图像质量/处理设置

它是与文件转换处理方法和图像质量有关的一组设置项目。





### “转换模式”

  指定转换模式。

* 降噪和放大倍率：执行降噪和放大倍率。

* 放大：放大

* 噪音消除：执行噪音消除

* 噪声消除（自动检测）和放大：放大。 仅当输入为JPEG图像时，才执行降噪 “按JPEG音频清除类别”





### “ JPEG噪声消除级别”

指定降噪等级。 较高的级别可以更有效地消除噪点，但可能会导致图像更平坦。





### “放大”

放大后设定尺寸。

* 通过放大倍率指定：以指定的放大倍率放大图像

* 转换后指定的宽度：放大到指定的宽度，同时保持图像的纵横比（像素）

* 由转换后的高度指定：放大到指定的高度，同时保持图像的纵横比（像素）

* 指定转换后的宽度和高度：放大为指定的宽度和高度。 指定为“ 1920x1080”（单位为像素）

对于大于2倍的放大倍率（仅一次用于消除噪音），请增加2倍，直到超过指定的放大倍率为止；如果放大倍数不是2的幂，则最后缩小 执行该过程。 因此，转换后的结果可能是平面图片。





### “模型”

指定要使用的模型。

最佳模型取决于要转换的图像，因此我们建议您尝试各种模型。

* 2D插图（RGB模型）：转换图像的所有RGB的2D插图模型

* 照片/动画（照片模型）：照片/动画模型

* 2D插图（UpRGB模型）：与相同或更高图像质量的2D插图（RGB模型）相比，转换速度更快的模型。 但是，消耗的内存（VRAM）量大于RGB模型的内存量，因此，如果在转换过程中发生强制终止，请调整分区大小。

* 照片/动画（Up Photo模型）：与具有相同或更高图像质量的照片/动画（Photo模型）相比，转换速度更高的模型。 但是，它比Photo模型消耗更多的内存（VRAM），因此，如果在转换过程中被强制终止，请调整分区大小。

* 2D插图（Y模型）：仅转换图像亮度的2D插图模型

* 2D插图（UpResNet10模型）：一种比2D插图（UpRGB模型）具有更高图像质量的模型。 请注意，如果分割大小不同，此模型将更改输出结果。

* 2D插图（CUnet模型）：可以使用随附的模型转换具有最高图像质量的2D插图的模型。 请注意，如果分割大小不同，此模型将更改输出结果。





### “使用TTA模式”

指定是否使用TTA（测试时间增强）模式。

使用TTA模式时，转换速度要慢8倍，但PSNR（图像评估指标之一）约为0.15。





## 处理速度设定

它是一组影响图像转换处理速度的设置项目。





### “分割大小”

在内部进行分割和处理时，指定宽度（以像素为单位）。

如何确定最佳值（过程以最快的速度结束），请参见“拆分大小”部分。

上方用“ -------”分隔的是输入图像的垂直和水平尺寸的除数，

较低的是从“ crop_size_list.txt”读取的常规分区大小。

请注意，如果分区大小太大，则所需的内存量（如果使用GPU，则为VRAM）超过了PC上可用的内存，并且软件强制终止。

这将在一定程度上影响处理速度，因此，通过指定文件夹转换大量相同图像尺寸的图像时，建议在转换之前检查最佳分割尺寸。

但是，请注意，取决于型号，更改分割大小可能会更改输出结果。

（在这种情况下，将使用默认的拆分大小，并且可以通过调整批处理大小来提高处理速度。）





### “批量”

在内部集中进行处理时，请指定尺寸。

增加批次大小可以提高处理速度。

请注意，所需的内存量以及分区大小不会超过PC上可用的内存。





## 操作设定

它是一组设置，总结了不太可能更改的操作设置。





### “在文件输入时自动转换开始设置”

设置当通过引用按钮指定输入文件还是拖放时自动开始转换。

如果将输入文件作为参数提供给exe，则此项的设置内容无效。

* 不自动启动：输入文件时不自动启动转换

* 输入一个文件后开始：输入一个文件后自动开始转换

* 输入一个文件夹或多个文件后开始：输入一个文件夹或多个文件后自动开始转换。 如果仅在调整转换设置时要转换单个图像文件，请这样做。





### “二手处理器”

指定执行转换的处理器。

* CUDA（如果可用，则为cuDNN）：使用CUDA（GPU）进行转换（如果cuDNN可用，则使用cuDNN）

* CPU：仅使用CPU进行转换





### “请勿覆盖输出文件”

如果此设置为ON，则如果图像写入目的地中存在具有相同名称的文件，则将不会执行转换。





### “在启动时使用参数设置”

将输入文件作为参数提供给exe时设置操作。

* 启动时转换：启动时自动开始转换

* 成功退出：如果转换结束时未失败，则自动退出





### “使用的GPU否”

您可以指定有多个GPU时要使用的设备编号。 在CPU模式下或指定了无效的设备编号时，它将被忽略。





### “输入参考固定文件夹”

按下输入参考按钮时首先显示的文件夹固定为此处设置的文件夹。





### “参考输出时的文件夹”

转换后图像的输出目标文件夹固定为此处设置的文件夹。

另外，单击输出参考按钮时首先显示的文件夹将固定为此处设置的文件夹。





## 其他

它是一组其他设置项目。





### “ UI语言”

设置UI语言。

首次启动时，将选择与PC语言设置相同的语言。 （如果没有，则为英文）





### “ CuDNN检查”

您可以通过按“ cuDNN检查”按钮检查cuDNN是否可以使用。

如果cuDNN不可用，将显示原因。



单击“执行”按钮开始转换。如果要在途中取消，请按“取消”按钮。但是，在实际停止之前会有时间滞后。进度条显示更改多张图像时的进度。日志显示了估计的剩余时间，这是处理多个具有相同高度和宽度的文件时的估计时间。因此，当文件的大小不同时，并且要处理的图像数为2个或更小时，仅显示“未知”是没有用的。





## 使用方法（CUI版本）

“ Waifu2x-caffe-cui.exe”是命令行工具。 `コマンドプロンプト(命令提示符)`启动并输入以下命令以使用。

以下命令将用法信息打印到屏幕上。

```
waifu2x-caffe-cui.exe --help
```

以下命令是执行图像转换的命令示例。

```
waifu2x-caffe-cui.exe -i mywaifu.png -m noise_scale --scale_ratio 1.6 --noise_level 2
```

执行上述操作后，`mywaifu(noise_scale)(Level2)(x1.600000).png`将转换结果保存到其中。

有关命令列表和每个命令的详细信息，请参阅命令行选项（公用）和命令行选项（CUI版本）部分。

## 命令行选项（常见）

使用此软件，可以指定以下选项。在GUI版本中，如果指定并启动了输入文件以外的命令行选项，则该选项文件当前未保存。对于GUI版本中未指定的选项，将使用上次的选项。

### -l <字符串>，--input_extention_list <字符串>

当input_file是文件夹时，在该文件夹中指定要转换的图像的扩展名。

预设值为`png：jpg：jpeg：tif：tiff：bmp：tga`。

分隔符为`：`。

Example.png：jpg：jpeg：tif：tiff：bmp：tga





### -e <字符串>，--output_extention <字符串>

当input_file是文件夹时，在该文件夹中指定要转换的图像的扩展名。

预设值为`png：jpg：jpeg：tif：tiff：bmp：tga`。

分隔符为`：`。

Example.png：jpg：jpeg：tif：tiff：bmp：tga





### -m <噪声|标度|噪声标度>，--mode <噪声|标度|噪声标度>

指定转换模式。 如果未指定，则选择“ noise_scale”。

* 降噪：执行降噪（确切地说，使用降噪模型执行图像转换）

* 比例尺：放大（准确地说，在放大现有算法之后，使用该模型执行图像转换以放大图像）

* noise_scale：执行降噪和放大（降噪后，继续放大处理）

* auto_scale：缩放。 仅当输入为JPEG图像时，才执行降噪





### -s <十进制值>，--scale_ratio <十进制值>

指定放大图像的次数。 默认值为2.0，但是您可以指定2.0以外的其他值。

如果指定了scale_width或scale_height，则将优先使用。

如果您指定的数字不是2.0，则执行以下处理。

* 首先，重复2倍放大倍率以覆盖指定的放大倍率，必要且足够。

* 如果指定的不是2的幂，则放大的图像将缩小到指定的放大倍率。





### -w <整数>，--scale_width <整数>

放大到指定的宽度，同时保持图像的纵横比（以像素为单位）。

如果与scale_height同时指定，则图像将被放大以具有指定的宽度和高度。





### -h <整数>，--scale_height <整数>

放大到指定的高度，同时保持图像的高宽比（像素）。

如果与scale_width同时指定，则图像将被放大以具有指定的宽度和高度。





### -n <0 | 1 | 2 | 3>，--noise_level <0 | 1 | 2 | 3>

指定降噪等级。 由于降噪模型仅适用于0到3级，

请指定0、1或2或3。

默认值为“ 0”。



### -p <cpu | gpu | cudnn>，--process <cpu | gpu | cudnn>

指定用于处理的处理器。 默认值为“ gpu”。

* cpu：使用CPU执行转换。

* GPU：使用CUDA（GPU）进行转换。 仅适用于Windows版本，如果cuDNN可用，请使用cuDNN。

* cudnn：使用cuDNN进行转换。





### -c <整数>，--crop_size <整数>

指定分割大小。 默认值为“ 128”。





### -q <整数>，--output_quality <整数>

设置转换后图像的图像质量。 默认值为-1

可以指定的值及其含义取决于在``输出扩展''中设置的格式。

如果为-1，则将使用每种图像格式的默认值。





### -d <整数>，--output_depth <整数>

指定转换图像每个通道的位数。 默认值为“ 8”。

可以指定的值取决于在“输出扩展名”中设置的格式。





### -b <整数>，--batch_size <整数>

指定小批量大小。 默认值为“ 1”。

最小批处理大小是图像按“分割大小”划分并同时处理的块数。 例如，如果您指定“ 2”，它将每2个块转换一次。

迷你批处理大小越大，GPU使用率就越高，就像拆分大小越大，但是拆分大小越大，测量时的效果越好。

（例如，如果拆分大小为“ 64”，而最小批处理大小为“ 4”，则当拆分大小为“ 128”且最小批处理大小为“ 1”时，处理过程更快地结束。）





### --gpu <整数>

指定用于处理的GPU设备编号。 默认值为“ 0”。

请注意，GPU设备编号从0开始。

如果没有GPU用于处理，则忽略。

如果指定了不存在的GPU设备编号，它将在默认GPU上执行。





### -t <0 | 1>，--tta <0 | 1>

如果指定为“ 1”，则使用TTA模式。 默认值为“ 0”。





### -，--ignore_rest

指定此选项后，将忽略所有选项。

对于脚本批处理文件。





## 命令行选项（GUI版本）

在GUI版本中，不适用于选项规范的参数被识别为输入文件。输入文件可以同时指定为文件，文件夹，多个文件和文件夹。



### -o，-output_folder

将路径设置为要保存转换后图像的文件夹。

将转换后的文件保存在指定的文件夹中。

转换文件的命名约定与在GUI中设置输入文件时自动确定的输出文件名相同。

如果未指定，它将与第一个输入文件保存在同一文件夹中。





### --auto_start <0 | 1>

如果指定“ 1”，则转换将在启动时自动开始。





### --auto_exit <0 | 1>

如果指定“ 1”，则转换在启动时自动完成，如果转换成功，它将自动结束。





### --no_overwrite <0 | 1>

如果指定为“ 1”，则在图像写入目的地存在同名文件时，将不会执行转换。





### -y <upconv_7_anime_style_art_rgb | upconv_7_photo | anime_style_art_rgb |照片| anime_style_art_y | upresnet10 | cunet>，-model_type <upconv_7_anime_style_art_rgb | upconv_7_photo | anime_net_art_art_g

指定要使用的模型。

它对应于GUI中的设置项目“ Model”，如下所示。

* upconv_7_anime_style_art_rgb：2D插图（UpRGB模型）

* upconv_7_photo：照片/动画（UpPhoto模型）

* anime_style_art_rgb：2D插图（RGB模型）

* 照片：照片/动画（照片模型）

* anime_style_art_y：2D插图（Y模型）

* upresnet10：2D插图（UpResNet10模型）

* cunet：2D插图（CUnet模型）





## 命令行选项（CUI版本）

### - 版

打印版本信息并退出。





### -？， - 帮帮我

显示用法信息并退出。

当您想查看如何轻松使用它时，请使用。





### -i <字符串>，--input_file <字符串>

（必需）要转换的图像的路径

指定文件夹后，该文件夹下的所有图像文件将被转换并输出到由output_file指定的文件夹。





### -o，-输出文件

您要在其中保存转换后的图像的文件的路径

（当input_file是文件夹时）保存转换后图像的文件夹路径

（当input_file是图像文件时）请确保输入扩展名（例如末尾的.png）。

如果未指定，将自动确定文件名并保存文件。

文件名确定规则是

```
[原始图像文件名]``（型号名称）``（模式名称）``（降噪等级（在降噪模式下））``（放大倍率（在放大模式下））''（输出 位数（8位除外）``.output extension
```

看起来像。

保存的位置基本上与输入图像位于同一目录。





### --model_dir <字符串>

指定存储模型的目录的路径。 默认值为`models / cunet`。

以下型号为标准配置。

*`models / anime_style_art_rgb`：2D插图（RGB模型）

*`models / anime_style_art`：2D插图（Y模型）

*`models / photo`：照片/动画（照片模型）

*`models / upconv_7_anime_style_art_rgb`：2D插图（UpRGB模型）

*`models / upconv_7_photo`：照片/动画（UpPhoto模型）

*`models / upresnet10`：2D插图（UpResNet10模型）

*`models / cunet`：2D插图（CUnet模型）

*`models / ukbench`：老式的摄影模型（仅包含放大模型，无法去除噪音）

基本上，您不必指定它。 使用默认模型或您自己的模型以外的模型时，请指定它。





### --crop_w <整数>

指定分割尺寸（宽度）。 如果未设置，将使用crop_size的值。

如果指定输入图像宽度的除数，则转换速度可能更快。





### --crop_h <整数>

指定分割尺寸（垂直宽度）。 如果未设置，将使用crop_size的值。

如果指定输入图像高度的除数，则转换速度可能更快。





## 分区大小

转换图像时，waifu2x-caffe（也称为waifu2x）将图像划分为固定大小，将其一一转换，最后将它们组合为一个图像。我会。分割大小（crop_size）是分割此图像时的宽度（以像素为单位）。

如果即使CUDA正在转换，GPU也不耗尽（GPU使用率接近100％），增加此值可能会导致处理更早结束。（因为可能会用完 GPU），请在查看[GPU-Z的](http://www.techpowerup.com/gpuz/) GPU负载（GPU使用率）和已用内存（VRAM使用率）时进行调整。另外，请参考以下特性。

- 更大的数字并不总是意味着更快
- 如果分割大小是图像的垂直和水平大小的除数（或除数少的数），则减少了浪费的计算量，并提高了速度。（在某些情况下，似乎不适用于此条件的数字会变为最快。）
- 如果将数字加倍，则理论上使用的内存量将是4倍（实际上是3到4倍），因此请注意不要丢弃该软件。特别是，CUDA比cuDNN消耗更多的内存，因此请当心。

## 关于带有Alpha通道的图像

该软件还支持使用Alpha通道放大图像。但是，请注意，放大没有alpha通道的图像所需的时间大约是原来的两倍，因为此过程是要自己放大alpha通道。但是，如果Alpha通道是由单一颜色组成的，则可以在与没有Alpha通道相同的时间进行扩展。

## 语言文件的格式

语言文件格式为JSON。如果创建新的语言文件，则将语言设置添加到'lang / LangList.txt'。'lang / LangList.txt'格式为TSV（制表符分隔值）。

- LangName：语言名称
- LangID：主要语言[请参阅MSDN](https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693.aspx)
- SubLangID：子语言[请参阅MSDN](https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693.aspx)
- FileName：语言文件名

例如

- 日语LangID：0x11（LANG_JAPANESE），SubLangID：0x01（SUBLANG_JAPANESE_JAPAN）
- 英文（美国）LangID：0x09（LANG_ENGLISH），SubLangID：0x01（SUBLANG_ENGLISH_US）
- 英文（英国）LangID：0x09（LANG_ENGLISH），SubLangID：0x02（SUBLANG_ENGLISH_UK）





## 注意

不保证该软件。请用户自行决定使用它。创建者不承担任何义务。





## 致谢

原来[Waifu2x](https://github.com/nagadomi/waifu2x)，并进行生产的模式，谁在MIT许可下发布的[过激论者](https://twitter.com/ultraistter)的，该Waifu2x在原有的基础[Waifu2x，转换](https://github.com/WL-Amigo/waifu2x-converter-cpp)谁创造了[阿米戈](https://twitter.com/WL_Amigo)的写作S（README和LICENSE.TXT，使用这样漂亮的代码，我被允许参考OpenCV[Waifu2x-Chainer](https://github.com/tsurumeso/waifu2x-chainer)执行原始模型的制作，以创建一个已经以MIT许可[tsurumeso](https://github.com/tsurumeso)的身份发布的模型，谢谢。

此外，@ paul70078用于将消息翻译成英语，@yoonhakcher用于将消息翻译成简体中文，@mzhboy用于请求简体中文翻译的请求，以及韩文消息。翻译为@ kenin0726，建议对朝鲜语翻译@aruhirin进行改进，将消息翻译为繁体中文@ lizardon1995，@ yoonhakcher，土耳其语翻译感谢@Scharynche提出的请求，@ Serized提出了法语翻译，并
感谢JYUNYA提供了该图标的GUI版本。