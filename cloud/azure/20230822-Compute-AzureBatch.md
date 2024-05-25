# Azure Batch
**HPC**这个词不知道同学们有没有听说过。它的全称是**High-performance computing**（高性能计算)，也叫做**Big Compute**。简单来说，它是由很多强算力的虚拟机组成的集群，这个算力包括CPU或者GPU，主要是给那些需要大量的算力运行的程序服务的。这些程序包括像是基因检测，石油勘探模拟，气候模型计算，图片处理，金融风险建模等等。这些程序动则就需要成百甚至上千上万的CPU/GUP来处理它们的计算量。很显然，我们前面提到的这些Azure服务都无法满足它。那今天我们介绍的**Azure Batch**，它就是Azure里的HPC服务。</br>

传统的HPC集群的安装部署需要在本地数据库中心里圈出许多虚拟机，然后在这些虚拟机上安装一些HPC的代理服务，这样当客户递交计算申请的时候，HPC服务就能把这些计算任务分配给安装代理服务的虚拟机上，把计算量分担出去。这个项目在传统的IT里其实是需要巨大的计算机成本投入的。但是在云上，就能非常好的利用云的可伸缩性，用多少资源，付多少费用，可以很大程度上降低前期的固定成本投入，而且还能和Azure上的其他服务相互配合，比如Azure Storage Account, Azure Data Factory等，是一个很好的业务模型。</br>

这里我们来介绍一个最简单的Azure Batch工作流程，方便你理解Azure Batch的工作方式。
1. 首先第一步，用户把需要处理的文件上传到Azure Storage Account里。这个文件可以是金融模型数据或者视频文件等等，也就是你要处理的信息。还可以包括一些你要处理数据的可执行脚本，比如说视频转译脚本文件。
2. 在你的Azure Batch账号里创建虚拟机集群以及选择虚拟机性能，比如需要多少台虚拟机，是Windows还是Linux得系统，还有哪些特殊的软件需要安装。然后创建一个使用这个软件执行数据的任务。
3. 接下来这些任务就会从第一步上传的Storage Account里下载数据文件到被分配的虚拟机上。
4. 然后这些数据就会在虚拟机上进行数据处理。在这个过程当中，虚拟机上的应用或服务会通过HTTPS向Azure Batch服务实时传递处理的进展状况。
5. 数据处理完成后，处理的结果就会被上传回Azure Storege。当然，如果你要是想的话，也可以直接到虚拟机上去下载结果。
6. 最后用户接到完成的通知后可以通过客户端应用或服务下载处理结果。

以上只是最简单的工作流程，还有一些功能其实Azure Batch也会提供的，比如在每个计算节点上进行多任务并行处理来缩短处理时间，或者你可以定义一些在任务开始之前或完成之后来执行的任务，可以为任务做准备，最后结束后清理数据现场。