---
title: 训练和注册 TensorFlow 模型
titleSuffix: Azure Machine Learning service
description: 本文介绍如何训练和注册使用 Azure 机器学习服务的 TensorFlow 模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.date: 05/28/2019
ms.custom: seodec18
ms.openlocfilehash: 314917ce91407206d786b191df118893696ac82c
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417121"
---
# <a name="train-and-register-tensorflow-models-at-scale-with-azure-machine-learning-service"></a>训练和 TensorFlow 模型大规模注册 Azure 机器学习服务

本文介绍如何训练和注册使用 Azure 机器学习服务的 TensorFlow 模型。 我们将使用常用[MNIST 数据集](http://yann.lecun.com/exdb/mnist/)分类手写的数字使用基于 TensorFlow 的深度神经网络。

使用 Azure 机器学习服务时，你将能够快速横向扩展的开源培训作业使用的弹性云计算资源。 您还可以跟踪训练运行、 版本模型、 部署模型，以及更多。 

无论要开发从从头 TensorFlow 模型还是到云中，正在将现有模型，您可以构建与 Azure 机器学习服务的生产就绪模型。

## <a name="prerequisites"></a>必备组件

- 安装用于 Python 的 Azure 机器学习 SDK
- 可选：创建工作区配置文件
- 下载[示例脚本文件](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)`mnist-tf.py`和 `utils.py`

可以按照[Python SDK 安装程序指南](setup-create-workspace.md#sdk)有关设置环境的分步说明。 示例培训文件可以位于我们[GitHub 示例页](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)及其本指南的扩展，Juypter Notebook 版本。

## <a name="set-up-the-experiment"></a>设置的实验

### <a name="import-packages"></a>导入包

首先，我们将需要导入所需的 Python 库。

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>初始化工作区

工作区对象是服务的顶级资源。 它为您提供集中的位置来使用您创建的所有内容。

如果你在完成中的可选步骤[先决条件部分](#prerequisites)，可以使用`Workspace.from_config()`快速创建工作区对象从配置文件中存储的详细信息。

```Python
ws = Workspace.from_config()
```

您可以还可以创建一个工作区显式。

```Python
ws = Workspace.create(name='<workspace-name>',
                      subscription_id='<azure-subscription-id>',
                      resource_group='<choose-a-resource-group>',
                      create_resource_group=True,
                      location='<select-location>' # For example: 'eastus2'
                      )
```

### <a name="create-an-experiment"></a>创建试验

创建试验和用于保存训练脚本的文件夹。 在此示例中，创建名为"tf mnist"实验

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
```

### <a name="upload-dataset-and-scripts"></a>上传数据集和脚本

[数据存储](how-to-access-data.md)是一个位置可以存储和访问装载或将数据复制到计算目标的数据。 每个工作区提供了默认数据存储。 这样用户可以轻松地访问在定型期间，我们将上传数据和训练脚本。

1. 下载本地的 MNIST 数据集

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. 将 MNIST 数据集上传到默认数据存储。

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. TensorFlow 培训脚本上, 传`tf_mnist.py`，并帮助程序文件， `utils.py`。

    ```Python
    shutil.copy('./tf_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>创建计算目标

创建计算目标为 TensorFlow 作业上运行。 在此示例中，我们创建启用了 GPU 的 AmlCompute 群集。 有关可用的培训一系列计算目标，请参阅[这篇文章](how-to-set-up-training-targets.md#compute-targets-for-training)

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

## <a name="create-a-tensorflow-estimator"></a>创建 TensorFlow 估算器

[TensorFlow 估算器](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)提供简单的方法启动的计算目标上的 TensorFlow 培训作业。 它将自动提供已安装 TensorFlow 的 docker 映像。

可以通过传递通过其名称中的结果 docker 映像中包含其他 pip 或 conda 包`pip_packages`和`conda_packages`参数。

```Python
script_params = {
    '--data-folder': ws.get_default_datastore().as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.01
}

est = TensorFlow(source_directory=script_folder,
                 script_params=script_params,
                 compute_target=compute_target,
                 entry_script='tf_mnist.py',
                 use_gpu=True)
```

## <a name="submit-a-run"></a>提交运行

[运行对象](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py)运行作业时，完成后提供的运行历史记录的接口。

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

执行运行时，它将经历以下阶段：

- **正在准备**:TensorFlow 估算器根据创建 docker 映像。 图像上传到工作区的容器注册表并缓存的更高版本运行。 日志还流式传输到运行历史记录，可以查看，以监视进度。

- **缩放**：群集将尝试纵向扩展如果 Batch AI 群集需要更多的节点不是当前可执行运行。

- **Running**：脚本文件夹中的所有脚本都上载到计算目标、 数据存储已装载或复制，并执行 entry_script。 从 stdout 输出和。 日志文件夹的流式传输到运行历史记录和可用于监视运行。

- **后期处理**：。 / 输出运行的文件夹复制到运行历史记录。

## <a name="register-or-download-a-model"></a>注册或下载模型

一旦已训练模型，可以将其注册到你的工作区中。 模型注册允许您存储和版本来简化工作区中的模型[管理和部署的模型](concept-model-management-and-deployment.md)。

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

此外可以使用 Run 对象来下载该模型的本地副本。 在训练脚本`mnist-tf.py`，TensorFlow 保护程序对象会一直保持到本地文件夹 （本地到计算目标） 模型。 我们可以使用 Run 对象下载副本。

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>分布式训练

[ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)估算器还在 CPU 和 GPU 群集之间支持分布式的培训。 你可以轻松运行分布式的 TensorFlow 作业和 Azure 机器学习服务将管理您的业务流程。

Azure 机器学习服务支持 TensorFlow 的分布式定型的两个方法：

* [基于 MPI](https://www.open-mpi.org/)分布式定型使用[Horovod](https://github.com/uber/horovod) framework
* 本机[分布式 TensorFlow](https://www.tensorflow.org/deploy/distributed)使用参数服务器方法

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod)是开放源代码框架，用于分布式的培训开发 Uber 的。 它提供了分布式 GPU TensorFlow 作业的简便途径。

若要使用 Horovod，指定`mpi`为`distributed_training`TensorFlow 估算器构造函数参数中的。 Horovod 将安装，以便在培训脚本中使用。

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',
                      use_gpu=True)
```

### <a name="parameter-server"></a>参数服务器

此外，你还可以运行[本机分布式 TensorFlow](https://www.tensorflow.org/deploy/distributed)，它使用参数服务器模型。 在此方法中，你将在一组参数服务器和工作线程中进行训练。 工作线程在训练期间计算梯度，而参数服务器聚合梯度。

若要使用的参数 server 方法，指定`ps`为`distributed_training`TensorFlow 估算器构造函数参数中的。

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='ps',
                      use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="note-on-tfconfig"></a>请注意 `TF_CONFIG`

您还需要的网络地址和端口的群集[ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec)，因此，Azure 机器学习设置`TF_CONFIG`为你的环境变量。

`TF_CONFIG` 环境变量是一个 JSON 字符串。 下面是介绍参数服务器变量的示例：

```JSON
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

TensorFlow 的高级别[ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API，TensorFlow 将分析这`TF_CONFIG`变量和生成群集为您的规范。

对于 TensorFlow 的较低级别 core Api 进行培训，分析`TF_CONFIG`变量和生成`tf.train.ClusterSpec`培训代码中。 在[此示例](https://aka.ms/aml-notebook-tf-ps)中，你将在训练脚本  中执行此操作，如下所示：

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="next-steps"></a>后续步骤

在本文中，将训练和注册 Azure 机器学习服务的 TensorFlow 模型。 若要了解如何部署模型，可以继续学习我们的模型部署文章。

> [!div class="nextstepaction"]
> [如何以及在何处部署模型](how-to-deploy-and-where.md)
