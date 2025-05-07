关键功能说明
  ​​依赖图构建​​：
  使用AssetDatabase获取所有AB包及其直接依赖，
  递归收集所有传递依赖形成完整依赖树，
  构建反向引用表referencedBy用于快速溯源。
  
​​拓扑标记算法​​：
  从根节点（包含场景/预制体的包）出发进行广度优先搜索，
  标记所有可达节点，剩余节点即为可卸载孤立包。
  
​​Hash去重机制​​：
  使用MD5计算AB包文件内容的Hash值，
  对相同Hash值的包进行分组，生成共享包，
  更新原始包的依赖关系指向共享包。
  
​​编辑器集成​​：
  通过EditorWindow提供可视化操作界面，
  可扩展为自动化构建管线的一部分。
  
实际应用示例
  假设存在以下AB包结构：
  
  - scene1 (包含Main.unity场景) 依赖 character
  - character 依赖 materials
  - materials (包含metal.mat)
  - weapon (包含sword.prefab) 依赖 materials
  - materials_copy (包含metal.mat的副本)
    
执行分析后将：

  标记scene1和weapon为根节点
  发现materials_copy未被任何根节点引用，标记为可卸载
  检测到materials和materials_copy内容Hash相同，合并为shared_xxxx包
  更新character和weapon的依赖指向shared_xxxx
