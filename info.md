
忆襄 (2016-08-15 10:47:00) 
你可以看成是零部件的加工，在加工前会对材料进行一些处理，比如把几个小的材料组合在一块，然后开始零件的加工过程，在加工过程需要把处理好的材料切割成需要的小零件
2016-08-15 10:51:54
忆襄  
工艺参数里涉及到原材料的选择（品种），加工过程使用的切割材料，一些配方比例，一些距离的调整等参数



cuberoot  
时序状态表，为什么有不少id的数据是完全一样的？这不科学吧？
cuberoot  
例如测试集里的8971和14885
2016-08-15 15:34:36
cuberoot (2016-08-15 15:34:36) @
还有7865和18094，这样完全相同的还有不少



忆襄 (2016-08-16 14:07:30) @
@cuberoot 确实如@badoun96 所言，一条生产线上同时加工好几个小批次，即product_no，可以把这几组product_no看成一个大批次，对于一个大批次，传感器采集的数据是一样的