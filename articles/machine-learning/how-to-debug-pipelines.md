---
title: 调试 ML 管道并排查其问题
titleSuffix: Azure Machine Learning
description: 使用 Python 调试 Azure 机器学习管道。 了解开发管道时的常见陷阱，以及有助于在远程执行之前和期间调试脚本的提示。 了解如何使用 Visual Studio Code 以交互方式调试机器学习管道。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: lobrien
ms.author: laobri
ms.date: 08/28/2020
ms.topic: conceptual
ms.custom: troubleshooting, devx-track-python
ms.openlocfilehash: 0f051e5b5711cec9fd8e72ec2b84c18f80430a0a
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2020
ms.locfileid: "89018053"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>对机器学习管道进行调试和故障排除
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文介绍如何在[AZURE 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)和[Azure 机器学习设计器 (预览) ](https://docs.microsoft.com/azure/machine-learning/concept-designer)中排除和调试[机器学习管道](concept-ml-pipelines.md)。 

## <a name="troubleshooting-tips"></a>故障排除提示

下表包含管道开发期间出现的一些常见问题，以及可能的解决方法。

| 问题 | 可能的解决方法 |
|--|--|
| 无法将数据传递给 `PipelineData` 字典 | 确保已在脚本中创建了一个目录，该目录对应于管道预期步骤要将数据输出到的位置。 大多数情况下，输入参数将定义输出目录，然后你需要显式创建该目录。 使用 `os.makedirs(args.output_dir, exist_ok=True)` 创建输出目录。 有关演示此设计模式的评分脚本示例，请参阅[该教程](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script)。 |
| 依赖项 bug | 如果你看到远程管道中的依赖关系错误在本地测试时未发生，请确认你的远程环境依赖项和版本与测试环境中的相同。 请参阅[生成、缓存和重复使用环境](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse)|
| 计算目标出现不明确的错误 | 请尝试删除并重新创建计算目标。 重新创建计算目标非常快速，可以解决某些暂时性问题。 |
| 管道未重复使用步骤 | 默认已启用步骤重复使用，但是，请确保未在管道步骤中禁用它。 如果已禁用重复使用，则步骤中的 `allow_reuse` 参数将设置为 `False`。 |
| 管道不必要地重新运行 | 若要确保步骤仅在基础数据或脚本更改时重新运行，请为每个步骤分离源代码目录。 如果对多个步骤使用同一个源目录，则可能会遇到不必要的重新运行。 在管道步骤对象中使用 `source_directory` 参数以指向该步骤的隔离目录，并确保未对多个步骤使用同一个 `source_directory` 路径。 |


## <a name="debugging-techniques"></a>调试技术

有三种用于调试管道的主要方法： 

* 调试本地计算机上的单个管道步骤
* 使用日志记录和 Application Insights 来隔离并诊断问题的根源
* 将远程调试器附加到在 Azure 中运行的管道

### <a name="debug-scripts-locally"></a>本地调试脚本

管道中最常见的失败之一是域脚本不按预期运行，或者包含难以调试的远程计算上下文中的运行时错误。

管道本身无法在本地运行，但在本地计算机上的隔离位置运行脚本可以更快地进行调试，因为无需等待计算和环境生成过程完成。 执行此操作需要完成一些开发工作：

* 如果数据位于云数据存储中，则需要下载数据并使其可供脚本使用。 使用较小的数据样本能够很好地在运行时减少系统开销，并快速获取有关脚本行为的反馈
* 如果你正在尝试模拟某个中间管道步骤，可能需要手动生成特定脚本预期前一步骤提供的对象类型
* 还需要定义自己的环境，并复制远程计算环境中定义的依赖项

在本地环境中运行脚本安装后，执行如下所述的调试任务就会容易得多：

* 附加自定义调试配置
* 暂停执行和检查对象状态
* 捕获运行时之前不会公开的类型或逻辑错误

> [!TIP] 
> 确认脚本按预期运行后，合理的下一步是在单步管道中运行该脚本，然后尝试在包含多个步骤的管道中运行该脚本。

## <a name="configure-write-to-and-review-pipeline-logs"></a>配置、写入和查看管道日志

在开始生成管道之前，在本地测试脚本是调试主要代码段和复杂逻辑的适当方式，但在某个时间点，你可能需要在执行实际管道运行本身期间调试脚本，尤其是在诊断与管道步骤交互期间发生的行为时。 我们建议在步骤脚本中充分使用 `print()` 语句，以便可以查看远程执行期间的对象状态和预期值，就像在调试 JavaScript 代码时一样。

### <a name="logging-options-and-behavior"></a>日志记录选项和行为

下表提供了针对管道的各个调试选项的信息。 这不是一个详尽的列表，因为除了此处显示的 Azure 机器学习、Python 和 OpenCensus 以外，还有其他选项。

| 库                    | 类型   | 示例                                                          | 目标                                  | 资源                                                                                                                                                                                                                                                                                                                    |
|----------------------------|--------|------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure 机器学习 SDK | 指标 | `run.log(name, val)`                                             | Azure 机器学习门户 UI             | [如何跟踪试验](how-to-track-experiments.md)<br>[azureml.core.Run 类](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=experimental)                                                                                                                                                 |
| Python 打印/日志记录    | 日志    | `print(val)`<br>`logging.info(message)`                          | 驱动程序日志、Azure 机器学习设计器 | [如何跟踪试验](how-to-track-experiments.md)<br><br>[Python 日志记录](https://docs.python.org/2/library/logging.html)                                                                                                                                                                       |
| OpenCensus Python          | 日志    | `logger.addHandler(AzureLogHandler())`<br>`logging.log(message)` | Application Insights - 跟踪                | [在 Application Insights 中调试管道](how-to-debug-pipelines-application-insights.md)<br><br>[OpenCensus Azure Monitor Exporters](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)（OpenCensus Azure Monitor 导出程序）<br>[Python 日志记录指南](https://docs.python.org/3/howto/logging-cookbook.html) |

#### <a name="logging-options-example"></a>日志记录选项示例

```python
import logging

from azureml.core.run import Run
from opencensus.ext.azure.log_exporter import AzureLogHandler

run = Run.get_context()

# Azure ML Scalar value logging
run.log("scalar_value", 0.95)

# Python print statement
print("I am a python print statement, I will be sent to the driver logs.")

# Initialize python logger
logger = logging.getLogger(__name__)
logger.setLevel(args.log_level)

# Plain python logging statements
logger.debug("I am a plain debug statement, I will be sent to the driver logs.")
logger.info("I am a plain info statement, I will be sent to the driver logs.")

handler = AzureLogHandler(connection_string='<connection string>')
logger.addHandler(handler)

# Python logging with OpenCensus AzureLogHandler
logger.warning("I am an OpenCensus warning statement, find me in Application Insights!")
logger.error("I am an OpenCensus error statement with custom dimensions", {'step_id': run.id})
``` 

### <a name="finding-and-reading-pipeline-log-files"></a>查找和读取管道日志文件

日志文件 `70_driver_log.txt` 包含： 

* 脚本执行期间输出的所有语句
* 脚本的堆栈跟踪 

若要在门户中查找此日志文件和其他日志文件，请先单击工作区中的管道运行。

![管道运行列表页](./media/how-to-debug-pipelines/pipelinerun-01.png)

导航到管道运行详细信息页。

![管道运行详细信息页](./media/how-to-debug-pipelines/pipelinerun-02.png)

单击特定步骤的模块。 导航到“日志”选项卡。其他日志包含有关环境映像生成过程和步骤准备脚本的信息。

![管道运行详细信息页日志选项卡](./media/how-to-debug-pipelines/pipelinerun-03.png)

> [!TIP]
> 可以在工作区中的“终结点”选项卡中找到“已发布的管道”的运行。 可以在“试验”或“管道”中找到“未发布的管道”的运行。 

有关从中进行日志记录和跟踪的详细信息 `ParallelRunStep` ，请参阅 [ParallelRunStep 的调试和故障排除](how-to-debug-parallel-run-step.md)。

## <a name="logging-in-azure-machine-learning-designer-preview"></a>Azure 机器学习设计器中登录 (预览版) 

对于在设计器中创建的管道，可以在创作页或管道运行详细信息页中找到 70_driver_log 文件。

### <a name="enable-logging-for-real-time-endpoints"></a>为实时终结点启用日志记录

若要在设计器中排除和调试实时终结点，必须使用 SDK 启用应用程序见解日志记录。 使用日志记录可排查和调试模型部署和使用问题。 有关详细信息，请参阅[对部署的模型进行日志记录](how-to-enable-logging.md#logging-for-deployed-models)。 

### <a name="get-logs-from-the-authoring-page"></a>从创作页获取日志

当你提交管道运行并停留在创作页时，可以找到每个模块完成运行时，为每个模块生成的日志文件。

1. 在创作画布中选择已完成运行的模块。
1. 在模块的右窗格中，转到“输出 + 日志”选项卡。
1. 展开右窗格，然后选择 70_driver_log.txt 并在浏览器中查看该文件。 还可以在本地下载日志。

    ![设计器中展开的输出窗格](./media/how-to-debug-pipelines/designer-logs.png)

### <a name="get-logs-from-pipeline-runs"></a>从管道运行获取日志

还可以在管道运行详细信息页找到特定运行的日志文件，文件位于工作室的“管道”或“试验”部分。 

1. 选择在设计器中创建的一个管道运行。

    ![管道运行页](./media/how-to-debug-pipelines/designer-pipelines.png)

1. 在预览窗格中选择模块。
1. 在模块的右窗格中，转到“输出 + 日志”选项卡。
1. 展开右窗格，在浏览器中查看 70_driver_log.txt 文件，或选择该文件，在本地下载日志。

> [!IMPORTANT]
> 若要从管道运行详细信息页更新管道，必须将管道运行克隆到新管道草稿。 管道运行是管道的快照。 它类似于日志文件，并且无法更改。 

## <a name="application-insights"></a>Application Insights
有关以此方式使用 OpenCensus Python 库的详细信息，请参阅此指南：[在 Application Insights 中对机器学习管道进行调试和故障排除](how-to-debug-pipelines-application-insights.md)

## <a name="interactive-debugging-with-visual-studio-code"></a>交互式调试与 Visual Studio Code

在某些情况下，可能需要以交互方式调试 ML 管道中使用的 Python 代码。 通过使用 Visual Studio Code (VS Code) 和 debugpy，可以在代码在定型环境中运行时附加到代码。 有关详细信息，请访问 [VS Code 指南中的交互式调试](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-machine-learning-pipelines)。

## <a name="next-steps"></a>后续步骤

* 有关 [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) 包和 [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) 包的帮助信息，请参阅 SDK 参考。

* 请参阅[设计器异常和错误代码](algorithm-module-reference/designer-error-codes.md)的列表。
