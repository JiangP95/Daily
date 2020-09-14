# 表说明
 1、结构设计

1.1、  逻辑结构设计

> Activiti使用到的表都是ACT_开头的。
>
> ACT_RE_*:
>
> ’RE’表示repository(存储)，RepositoryService接口所操作的表。带此前缀的表包含的是静态信息，如，流程定义，流程的资源（图片，规则等）。
>
> ACT_RU_*:
>
> ‘RU’表示runtime，运行时表-RuntimeService。这是运行时的表存储着流程变量，用户任务，变量，职责（job）等运行时的数据。Activiti只存储实例执行期间的运行时数据，当流程实例结束时，将删除这些记录。这就保证了这些运行时的表小且快。
>
> ACT_ID_*:
>
> ’ID’表示identity (组织机构)，IdentityService接口所操作的表。用户记录，流程中使用到的用户和组。这些表包含标识的信息，如用户，用户组，等等。
>
> ACT_HI_*:
>
> ’HI’表示history，历史数据表，HistoryService。就是这些表包含着流程执行的历史相关数据，如结束的流程实例，变量，任务，等等
>
> ACT_GE_*:
>
> 全局通用数据及设置(general)，各种情况都使用的数据。

1.2、  所有表的含义

| 序号 | 表名                | 说明                                                         |
| ---- | ------------------- | ------------------------------------------------------------ |
| 1    | act_ge_bytearray    | 二进制数据表                                                 |
| 2    | act_ge_property     | 属性数据表存储整个流程引擎级别的数据,初始化表结构时，会默认插入三条记录， |
| 3    | act_hi_actinst      | 历史节点表                                                   |
| 4    | act_hi_attachment   | 历史附件表                                                   |
| 5    | act_hi_comment      | 历史意见表                                                   |
| 6    | act_hi_identitylink | 历史流程人员表                                               |
| 7    | act_hi_detail       | 历史详情表，提供历史变量的查询                               |
| 8    | act_hi_procinst     | 历史流程实例表                                               |
| 9    | act_hi_taskinst     | 历史任务实例表                                               |
| 10   | act_hi_varinst      | 历史变量表                                                   |
| 11   | act_id_group        | 用户组信息表                                                 |
| 12   | act_id_info         | 用户扩展信息表                                               |
| 13   | act_id_membership   | 用户与用户组对应信息表                                       |
| 14   | act_id_user         | 用户信息表                                                   |
| 15.  | act_re_deployment   | 部署信息表                                                   |
| 16.  | act_re_model        | 流程设计模型部署表                                           |
| 17   | act_re_procdef      | 流程定义数据表                                               |
| 18   | act_ru_event_subscr | throwEvent、catchEvent时间监听信息表                         |
| 19   | act_ru_execution    | 运行时流程执行实例表                                         |
| 20   | act_ru_identitylink | 运行时流程人员表，主要存储任务节点与参与者的相关信息         |
| 21   | act_ru_job          | 运行时定时任务数据表                                         |
| 22   | act_ru_task         | 运行时任务节点表                                             |
| 23   | act_ru_variable     | 运行时流程变量数据表                                         |

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



