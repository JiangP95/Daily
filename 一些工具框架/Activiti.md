# 表说明
  act_re_deployment：部署对象表
  act_re_procdef：流程定义表
  act_ge_bytearray：资源文件表
  act_ge_property：主键生成策略表
  
 # activiti与jpbm的对比
   1. avtiviti
 	采用了流程虚拟机，支持除了bpmn2.0规范之外的流程格式，提供的接口更清晰，链式API更优雅，上手比较快。
    2. JPBM
    对自身技术过于依赖，仅支持BPMN2。


对比项		 |	Activiti				|	JPBM
------------|--------------------------|--------
ORM		    | 	mybatis3				| Hibernate
事务管理	|	spring事务机制			| 基于JTA事务管理
引擎核心	|	PVM，流程虚拟机			|Drools
流程格式	|	BPMN2，XPDL，jPDL			|BPMN2
集成接口	|	SOAP，Restful,Mule		|消息通讯
设计模式	|	命令模式，观察者模式		| 



