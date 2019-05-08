# 虚拟DOM

avaon会在DOMReady对.ms-controller节点进行outerHTML操作 之前是直接用outerHTML,但后来发现outerHTML在各个浏览器下差异性太大了. IE6-7会对colgroup, dd, dt, li, options, p, td, tfoot, th, thead, tr元素自闭合 让我的htmlPaser跪掉 于是写了htmlfy,手动取每个元素nodeName, attrName, attrValue, nodeValue来构建outerHTML

第2阶段,将这个字符串进行parser,转换为虚拟DOM 这个阶段对input/textarea元素补上type属性, `ms-*`自定义元素补上ms-widget属性, 对table元素补上tbody, 在ms-for指令的元素两旁加上 `<!--ms-for-->`,`<!--ms-for-end-->`占位符, 并将它们的之间的元素放到一个数组中(表明它们是循环区域) 并去掉所有只有空白的文本节点

第3个阶段,优化,对拥有`ms-*`属性的虚拟DOM添加dynamic属性 表明它以后要保持其对应的真实节点,并对没有`ms-*`属性的元素添加skipAttrs属性,表明以后不需要遍历其属性。 如果它的子孙没有`ms-*`或插值表达式或ms-自定义元素,那么还加上skipContent，表明以后不要遍历其孩子.

这三个属性,dynamic用于节点对齐算法,skipAttrs与skipContent用于diff算法

第4个阶段, 应用节点对齐算法, 将真实DOM中无用的空白节点移除,并插入占位符, 并将需要刷新的元素保持在以应的拥有dynamic属性的虚拟DOM中

第5个阶段,放进render方法中,render方法里面再调parseView,parseView会调每个指令的parse方法 将虚拟DOM树转换为一个$render方法

第6个阶段,执行$render方法,生成新的虚拟DOM,与最早的那个虚拟DOM树diff,一边diff一边更新真实DOM.

以后VM的属性发生变动,就直接执行第6个阶段.