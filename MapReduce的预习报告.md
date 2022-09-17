# MapReduce 
    MapReduce是一种用于在大型商用硬件集群中对海量数据实施可靠度、高容错的分布式计算的框架，也是一种经典的并行计算模型。
    
## MapReduce编程模型
### 简介
    MapReduce是一种思想或是一种编程模型。
    MapReduce编程模型主要由两个抽象类构成，即Mapper类和Reducer类。
    在数据格式上，Mapper接受<key,value>格式的数据流，并产生一系列同样是<key,value>形式的输出
    
### MapReduce简单模型
    该模型只有Mapper过程，由Mapper产生的数据直接写入HDFS。
### MapReduce复杂模型
    对于大部分的任务来说，都是需要Reduce过程，并且由于任务繁重，会启动多个Reducer来汇总。
    
## MapReduce数据流
    从MapReduce的编程模型中可以发现，数据以不同的形式在不同节点之间流动。
    MapReduce处理的是<key,value>形式的数据，即不能直接处理文件流。而其数据源和多个Mapper产生的数据分配给多个Reducer的操作都是由Hadoop提供的基本API（InputFormat、Partioner、OutputFormat）实现的。
### 1.分片、格式化数据源（InputFormat）
    InputFormat主要由两个任务，一是对源文件进行分片，并确定Mapper的数量；二是对各分片进行格式化，处理成<key,value>形式的数据流并传给Mapper。
### 2.Map过程
    Mapper接收<key,value>形式的数据，并处理成<key,value>形式的数据。
### 3.Combiner过程
    每一个map()都可能会产生大量的本地输出，Combiner()的作用就是对map()端的输出先做一次合并，以减少在Map和Reduce结点之间的数据传输量，提高网络I/O性能，是MapReduce的一种优化手段之一。
### 4.Shuffle过程
    Shuffle过程是指从Mapper产生的直接输出结果，经过一系列的处理，成为最终的Reducer直接输入数据为止的整个过程，这一过程也是MapReduce的核心过程。
### 5.Reduce过程
    Reducer接收<key,{value,list}>形式的数据流，形成<key,value>形式的数据输出，输出数据直接写入HDFS。
    
## MapReduce任务运行流程
### 1.MRv2基本组成
    1）客户端
    2）MRAPPMaster
    3）Map Task和Reduce Task
    
## Yarn基本组成
    Yarn是一个资源管理平台，它监控和调整集群资源，并负责管理集群所有任务的运行和任务资源的分配，其基本组成有：
### 1）Resource Manager（RM）    
    为整个集群的资源调度器，主要包括Resource Schedule（资源调度器）和Application Manager（应用程序管理器）两个组件。
### 2）NodeManager
    运行于DataNode，监控并管理单个结点的计算资源，害负责对container进行创建、运行状态的监控及最终销毁。
### 3）ApplicationMaster（AM）
    负责对一个任务流程的调度、管理，包括任务注册、资源申请、以及和NodeManager通信以开启和杀掉任务等。
### 4）container
    Yarn架构下对运算资源的一种描述。
    
## 任务流程
    1）client向ResourceManager提交任务。
    
    2）ResourceManager分配该任务的第一个container,并通知相应的NodeManager启动MRAPPMaster。
    
    3）NodeManger接收命令后，开辟一个container资源空间，并在container中启动相应的MRAppMaster.

    4) MRAppMaster启动之后，第一步会向RourceManager注册，这样用户可以直接通过MRAppMaster监控任务的运行状态:之后则直接由MRAppMaster调度任务运行，重复5)~8),直到任务结束。

    5) MRAppMaster以轮询的方式向ResourceManager申请任务运行所需的资源。

    6)一旦ResourceManager配给了资源，MRAppMaster便会与相应的NodeManager通信，让它划分Container并启动相应的任务(MapTask 或 ReduceTask)。

    7) NodeManager 准备好运行环境，启动任务。

    8)各任务运行，并定时通过RPC协议向MRAppMaster汇报自己的运行状态和进度。MRAppMaster也会实时地监控任务的运行，当发现某个Task假死或失败时，便杀死它重新启动任务。
    
    9)任务完成，MRAppMaster向ResourcerManager通信，注销并关闭自己。





