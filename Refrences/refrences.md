## 错误的思路  
一直以来，思路都错了 总是往研究的方法去思考，试图从中找到一个范式，一个统一的的研究方法，也就是一招鲜，但事实上，作为生态系统生态学，他是一个复合生态系统，任何一个因素的影响都会发生改变，其实这些改变在一般情况下都是正常，这些改变是反馈活动，无时无刻调整着生态系统的稳态，然而当发生一些变化时，会从这个稳态到另一个稳态，此时也是没问题的，有问题的知识，另一个状态是什么情况，反复无常呢？还是不能支撑现在系统结构，或者对于未来来说变得易受攻击。  
亿万年的演替下，大多生态系统已经稳定了，在这稳定下，她们能有效地运行下去，然而现在我们改造的能力太强了，这些反馈变成了正反馈，变得越来越不适合人类生存。  
其实当不符合其他物种生存时，人类的生存将会不适宜而来。
### (1)景观生态系统的自适应结构与功能的研究进展
  景观结构在不同尺度和不同地质环境有着不同的问题，地形地貌限制下，每个尺度的关系也是不一致的，而且在不同区域下，其主体的不同，其驱动因素是不一致的。（全球变化背景下景观生态适应性特征2017）
### （2）基于多源数据的城市公共空间活力影响因素研究—以上海市黄浦江滨水区为例（2021）
活力中的密度和稳定区域   韧性中的关系 
![image](https://github.com/user-attachments/assets/6f045c1c-41d8-4a83-90d6-6357c4d67e76)

### （3）使用ALE（累积局部效应）分析了不同农村土地覆盖类型对城市热岛效应（UHI）的影响  
采用 SHapley 加法解释 （SHAP） 对农村土地覆盖的<*关键景观参数进行排名>，包括景观级参数 （LLP） 和景观类参数 （LCP）。最后，使用累积局部效应 （ALE） 图来揭示各个关键景观参数对UHI强度( UHII)的影响。内部已经研究的很多了，需要与外部一同考虑了。
```python
import pandas as pd
import numpy as np  
  import matplotlib.pyplot as pltplt.rcParams['font.family'] = 'Times New Roman'    
  plt.rcParams['axes.unicode_minus'] = Falseimport warnings# 忽略所有警告warnings.filterwarnings("ignore")    
  path = r"I.xlsx"    
  df = pd.read_excel(path)    
  from sklearn.model_selection import train_test_split# 划分特征和目标变量    
   X = df.drop(['y'], axis=1)   
   y = df['y']  # 划分训练集和测试集  
   X_train, X_test, y_train, y_test = train_test_split(    X,      y,     test_size=0.3,  random_state=42)   
   from sklearn.ensemble import RandomForestRegressor # 这里只是为ALE模型实现提供一个基础模型没有经过调参等# 初始化随机森林回归模型  
  rf_model = RandomForestRegressor(random_state=42)# 训练模型     
  rf_model.fit(X_train, y_train)    
  from alepython import ale_plot# 对单个特征绘制ALE图  
  ale_plot(rf_model, X_test, "infP", bins=50)  # 对特征 infP 进行解释   
  plt.show()
```
  ![image](https://github.com/user-attachments/assets/527c0d0b-66ec-4120-a29d-930b0f506b9a)

```python
import matplotlib as mpl
# 设置 matplotlib 图的默认大小为 9x6 英寸
mpl.rc("figure", figsize=(9, 6))
# 调用 ale_plot 函数绘制 Accumulated Local Effects (ALE) 图
ale_plot(    rf_model,                  # 传入机器学习模型（例如训练好的回归或分类模型）
X_test,                      # 数据特征集，用于生成 ALE 图
X_test.columns[:1],          # 选择要绘制 ALE 的特征列，这里选择第一个特征列
bins=20,                # 将特征值分成 20 个区间（箱数）
monte_carlo=True,       # 启用蒙特卡罗模拟，用于增加鲁棒性
monte_carlo_rep=100,    # 设置蒙特卡罗模拟的重复次数为 100
monte_carlo_ratio=0.6,  # 设置蒙特卡罗采样比例为 60%)
```
![image](https://github.com/user-attachments/assets/c94ac1eb-4ef7-40b0-9889-eaaf2c4666eb)

 单个特征的 ALE 绘图  
```
ale_plot(model=rf_model, train_set=X_test, features=["infC"], monte_carlo=True, save_path='infC.pdf')
```
 ![image](https://github.com/user-attachments/assets/d1255eaf-8308-417a-80f6-fc6c26a966f9)
```
ale_plot(model=rf_model, train_set=X_test, features=["infP"], monte_carlo=True, save_path='infP.pdf')
```
 ![image](https://github.com/user-attachments/assets/b7d9409c-abfc-41bd-b4e5-023c705c29fb)

这里展示的是不同取值下的加法局部效应（ALE）值及其相应的置信区间具体的值
```ale_eff = ale(X=X_test, model=rf_model, 
              feature=["sqft_lot", "sqft_living"], grid_size=100)```
![image](https://github.com/user-attachments/assets/2fb51f2b-ed8b-47fb-8027-c230bfb623b9)

#### 计算ALE（加法局部效应）值
```
ale_eff = ale(
    X=X_test,            # 输入数据集 用于计算ALE的特征数据
    model=rf_model,              # 模型 model: 用于计算ALE的已训练模型，通常是一个回归或分类模型
    feature=["grade"]      # 特征 feature: 计算ALE的特征，这里是“grade”，表示需要分析的特征
)
```
![4a7473a7fd3cbc9c49d957707e6812c3](https://github.com/user-attachments/assets/2ce34bdf-7952-4a16-9e41-2a731363e695)
在训练模型时，我们将特征矩阵转换为 numpy 数组。这是为了避免在创建 ALE 时出现警告消息。
```python
model = RFR()  
model.fit(X.to_numpy(), y)  
ale = ALE(model.predict , feature_names=X.columns, target_names=['Rings'])  
exp = ale.explain(X.to_numpy())   
ale = ALE(model.predict , feature_names=X.columns, target_names=['Rings'])
exp = ale.explain(X.to_numpy())
plot_ale(exp, features=[0,1,2], fig_kw={'figwidth':15, 'figheight': 5})
```
![image](https://github.com/user-attachments/assets/128142bf-0627-40e9-b44d-bdc0e68054fe)

### （4）过度为了连通性反而没多大用，增加斑块面积和质量 保障繁殖和生存可能更加有效 
https://link.springer.com/article/10.1007/s13280-025-02149-1?utm_medium=external_display&utm_source=stork&utm_content=email&utm_term=null&utm_campaign=CONR_JRNLS_AWA1_CN_CNPL_0034V_STKRE#citeas
Green infrastructure has weak conceptual links with efficient biodiversity conservation  
###  （5）特有性分布格局的解释高度依赖于观察尺度体系的选择
尺度选择中所需要考虑的区域大小(粒度)、总面积(空间范围)以及物种分类标准(分类处理)都会影响到最终呈现的结果。例如，在局地尺度某个地区可能被视为特有性热点地区，但在全球视角下可能不显著。因此，保护策略制定需建立多尺度分析框架：粗粒度用于大区域保护规划，中粒度统筹跨境物种保护，细粒度则支撑特有种栖息地的精准保护。未来研究需进一步整合多类群、多维度指标，发展尺度稳健的生物多样性评估体系。

原文链接：Daru, B. H., Farooq, H., Antonelli, A., & Faurby, S. (2020). Endemism patterns are scale dependent. Nature Communications, 11(1), 2115. https://doi.org/10.1038/s41467-020-15921-6
### （6） Nature论文解读：南极臭氧恢复的指纹识别研究 Fingerprinting the Recovery of Antarctic Ozone
首次应用气候变化研究中的指纹识别方法评估南极臭氧恢复  
量化了ODS减少对臭氧恢复的贡献，与温室气体影响进行区分  
揭示了ODS强迫如何影响臭氧的内部变异性，进而影响恢复信号的检测   
   区分自然变化与人类恢复 全球绿化 

### (7) Framework for risk assessment of economic loss from structures damaged by rainfall-induced landslides using machine learning
极端降雨事件的频率增加，强降雨诱发滑坡的灾前防范措施对提高灾害韧性至关重要。本研究提出了一种基于机器学习的降雨诱发滑坡损害建筑结构的经济风险评估方法。采用随机森林和LightGBM算法构建基于机器学习的滑坡预测模型，并综合考虑滑坡状态因子及其触发因子的空间分布。在研究中，通过降雨指数考虑降雨的时间变化，并将其与降雨强度一起作为特征变量。利用广义极值分布统计估计降雨指数与其年超越概率的关系，从而构建降雨灾害曲线。随后，利用基于机器学习的滑坡预测模型和降雨灾害曲线评估降雨诱发滑坡的易发性。最后，基于滑坡易发性评估结果和建筑物分布图，估算了降雨诱发滑坡对建筑物造成的经济损失风险曲线。研究结果表明，在评估降雨诱发滑坡易发性方面，LightGBM的预测性能优于随机森林。所采用的案例分析展示了该方法可用于制定基于风险的灾害减缓策略。

### (8) Global Change Biology | 干旱引起温度对生态系统碳吸收控制作用的减弱(20225)
 随着北方地区的气候变暖，生态系统的碳吸收量逐渐增加。然而，温暖气候条件下温度对碳吸收的正向效应是否会持续，仍然存在不确定性。  
 特别是，水分胁迫如何影响温度对生态系统碳吸收的控制作用，也未被充分研究。  
 
本文使用标准化的多重回归方法，基于遥感卫星和地面观测数据，系统地探讨了1983年至2018年间温度对北方生态系统初级生产力（GPP）的控制作用变化（SGPP-TAS）。研究表明，自2000年以来，SGPP-TAS在空间和时间上发生了显著的转变，温度对碳吸收的控制作用从正向转变为负向。土壤水分的变化被认为是主要的驱动因素，特别是在草地和针叶林地区，水分的变化加速了这一转变。这一发现对未来全球变暖和干旱加剧条件下北方生态系统碳吸收潜力的预测具有重要意义。
Wu, H., Fu, C., Yu, K., et al. Drought-Induced Weakening of Temperature Control on Ecosystem Carbon Uptake Across Northern Lands. Global Change Biology, 31:e70032 (2025). https://doi.org/10.1111/gcb.70032.

### (9) 结合水动力模型和机器学习技术，加强城市地区洪水风险评估
https://mp.weixin.qq.com/s/0xhzd2ZggRQvaxQgcs8MXwhttps://mp.weixin.qq.com/s/0xhzd2ZggRQvaxQgcs8MXw
城市洪水风险由于气候变化和密集的基础设施建设而加剧，需要创新的评估方法。本研究旨在通过整合先进的水动力模型和机器学习（ML）技术来提高城市洪水预测和危险性分析的准确性。通过精确参数校准的1D和2D水动力模型，展示了对洪水动态的卓越预测能力。研究使用高分辨率Lidar图像和复杂的建模技术模拟了伦敦西部泰晤士河的洪水情景，并结合社会经济数据绘制了易受影响区域的精确地图。研究结果表明，2D水动力模型虽然计算量大，但在预测洪水动态方面具有更高的准确性，而1D模型在预测城市相邻区域的淹没深度和范围方面可能更准确。先进的机器学习模型，如ET-PCA（Extra Trees-主成分分析）模型，进一步增强了研究结果的可靠性，显示出近乎完美的预测可靠性（R²为0.999）。这种模型提供了近乎完美的预测，并实现了精确的洪水风险分区。结合社会经济数据进一步突出了某些城市区域的脆弱性，并强调了制定针对性洪水缓解策略的重要性。除了具体案例研究外，本研究还展示了将水动力模拟与机器学习相结合的潜力，以增强全球城市洪水韧性。这一框架有助于城市规划者和政策制定者制定有效的洪水缓解策略，并提高基础设施对未来洪水事件的韧性。

### (10) NC 气候和人口变化在全球洪水暴露和脆弱性中的作用
The role ofclimate and population change in globalflood exposure and vulnerability 
洪水是一种普遍存在的自然灾害，对社会的影响深远广泛。通过使用一个高分辨率的全球洪水模型，涵盖海岸、河流和降水等多重灾害因素，我们阐明了气候变化与人口增长在改变洪水暴露中的作用。预计到2100年，面临1%年风险（100年一遇）洪水灾害的人口将从2020年的16亿增长至19亿。在这一变化中，我们将21.1%的变化归因于气候变化，76.8%归因于人口变化，2.1%则归因于气候和人口共同变化。影响洪水暴露不确定性的最大驱动力是人口变化，而气候变化依然是一个较小但仍然重要的因素。2020至2100年间，全球洪水暴露的增加主要由低GDP地区推动，到2100年，低GDP地区将在整体和城市地区占据63%的暴露份额。城市地区几乎在所有全球区域中都尤为脆弱，尤其是对极端事件敏感的城市地区，预计其人口暴露将增加33%。本研究突显了洪水暴露中的巨大不平等，未来的研究应将资源和策略集中于这些地区，以实现可持续的风险减缓.
 那沿海的风险呢？ 有一篇就是自然遗产对海平面上升的威胁程度

 
 ### （11） 通过网络分析探究黄土高原生态系统服务与社会生态因素的相互依赖关系
 生态系统服务产生于社会生态系统的内部相互作用，并受其影响。虽然网络分析方法在概念化和管理生态系统服务方面具有潜力，但其实际应用仍有待深入探索。该文介绍偏相关网络方法在生态系统服务研究中的新应用，以中国的黄土高原为案例，通过网络框架分析生态系统服务和社会生态因子。研究结果表明，从2000年到2020年，关键生态系统服务包括土壤保持、固碳和食物供给都有显著改善，而水供给呈现非显著下降趋势。网络分析揭示了生态恢复活动、景观格局变化、社会生态过程与黄土高原生态系统服务之间复杂的相互依存关系，识别出网络中的关键节点和连接。植物生产力趋势显示出最高的节点强度，表明其在驱动整个网络配置的重大变化中发挥关键作用。该文强调了偏相关网络方法在全面理解生态系统服务与社会生态因素相互依存关系方面的潜力，为生态系统服务管理和政策制定提供了新见解。
 
偏相关网络方法在生态系统服务评估中的新颖应用。这种方法超越了传统的生态系统服务时空分析，为黄土高原生态系统服务和社会生态驱动因素之间的相互依赖性和潜在反馈模式提供了更深入的见解。未来的研究可以将这种方法扩展到不同的社会生态系统，纳入长期监测，并包括捕获人类环境反馈的其他变量。这些进步将增强这种方法为应对全球环境挑战的适应性管理战略和政策干预提供信息的能力。该文中网络节点（变量）的选择和权重以生态系统服务框架和数据可用性为指导，并且所选指标反映了关键的社会生态过程和服务，但这些变量的代表性和权重仍有改进的空间，并且该文中未包括文化维度、社会关系和制度影响，这些因素可以在塑造生态系统及其提供的服务的动态方面发挥关键作用，未来的研究可以扩展网络框架以纳入这些因素，从而更全面地了解生态系统和社会系统之间的相互作用。
Wang, Z., Fu, B., Wu, X., Wang, S., Li, Y., Zhang, L., ... & Wu, X. (2025). Exploring the interdependencies of ecosystem services and social-ecological factors on the Loess Plateau through network analysis. Science of The Total Environment, 960, 178362.

## 生态系统服务 生态系统稳定性 生态系统因子 生态系统结构 社会
### （12）量化三峡水库对极端水文事件的调节能力及其对气候变化下流量状态的影响——《Water Resources Research》  灰色基础设施的生态系统调节能力
三峡水库（TGR）是世界上最大的水电工程之一，在长江水资源管理中起着重要作用。为了防灾减灾和流域管理，了解TGR对极端水文事件的调节能力及其在气候变化背景下对流量状态的影响至关重要。本研究获取了1961-2019年的TGR历史入流量，并使用分布式水文模型模拟2021-2070年的未来入流量。这些数据被用于驱动基于机器学习的TGR运行模型，以获得TGR运行下的模拟出流量，然后与TGR不运行的自然流量进行比较，以评估TGR的影响。结果表明，在历史时期，TGR的运行可以使平均洪峰和总洪水天数分别减少29.2%和53.4%。干旱指标（包括持续时间和强度）的相对下降一般少于10%。面对未来更严重的极端水文事件，TGR仍有望缓解洪水和干旱，但不能将其降低到历史水平。TGR运行对流量状态的影响也将在气候变化中演变，可能改变河流生态系统的栖息地。本研究提出了模拟大型水库运行和量化流量状态影响的可行方法，并为长江上游流域的综合水资源管理提供了见解。
Cheng, H., Wang, T., & Yang, D. (2024). Quantifying the regulation capacity of the Three Gorges Reservoir on extreme hydrological events and its impact on flow regime in a changing climate. Water Resources Research, 60(6), e2023WR036329. https://doi.org/10.1029/2023WR036329

### （13） 【Sustainable Cities and Society】城市人口与环境协调发展及影响因素分析——以中国35个大都市为例
人口与环境 与系统服务 与环境 绿地 
Analysis of the coordinated development and influencing factors betweenurban population and environment: A case study of 35 metropolisesin China
平衡人口与环境之间的关系对于当今城市的可持续发展至关重要。从城市规模角度研究人口与环境协调发展的研究相对较少。本研究构建了人口和环境系统的综合评价系统。应用相对发展指数、修正耦合协调度 （CCD） 模型、障碍度模型和双向固定效应模型，探讨了 2006—2022 年中国 35 个大城市人口-环境 （P-E） 复杂系统的发展趋势、耦合协调关系和影响因素。结果表明：（1）人口和环境发展水平普遍提高，不同规模城市间人口发展水平存在显著差异;（2） 种群发展滞后于环境发展，尽管它们逐渐变得同步;（3）P-E 复杂系统的 CCD 随城市规模的增加而增加;（4） 影响 P-E 协调的主要因素包括种群繁殖、环境投资、老龄化和迁移。特大城市等大城市应优先减缓人口老龄化、缩小城乡收入差距、加强污染治理和平衡性别结构。规模较小的城市可以专注于优化工业和就业结构。研究结果为促进城市协调、高质量和可持续发展提供了建议。
### （14）内蒙古和新疆生态系统质量变化与人类活动贡献的综合评价框架 Land Use Policy 
A comprehensive framework for evaluating ecosystem quality changes and human activity contributions in Inner Mongolia and Xinjiang, China
人类活动越来越多地塑造生态系统，引发了关于可持续性以及生态系统质量和生态系统服务之间权衡的关键问题。本研究开发了一个综合框架，以评估中国两个具有重要生态意义的地区内蒙古和新疆由土地利用土地覆盖 （LULC） 变化和人类活动 （HA） 驱动的生态系统质量变化 （KEQI） 变化，以及它们对生态系统服务价值 （ESV） 的影响。该框架整合了五种分析方法：（i） 使用生态可持续性土地变化建模器 （LCMES） 和热点分析分析 LULC 动态，以确定发生重大变化的区域;（ii） 使用遥感数据评估 LULC 对生态系统服务价值的影响;（iii） 通过改进的残差趋势分析和基于像素的偏相关系数，将人类活动和气候变化的影响分开;（iv） 通过保留率和生态质量变化率评估 KEQI 的变化;（v） 使用曲线拟合线性回归探索 KEQI、LULC、HA 和 ESV 之间的关系。该研究揭示了显著的土地利用变化，农田、草原和城市地区的扩张以牺牲裸露土地为代价，主要由人类活动驱动，内蒙古占 93%，新疆占 89%。以人类活动为主的地区观察到 KEQI 的显著改善，数值增加了 13.5–16.6，尤其是在内蒙古。然而，这些改善往往与 ESV 下降同时发生，尤其是内蒙古的草原 （-4.92%） 和新疆的农田。一些地区显示 ESV 适度增加 （+0.88 %）。本研究强调了 HA、KEQI 和 ESV 之间错综复杂的平衡，为中国的可持续土地管理提供了重要见解。政策制定者可以利用这些发现更好地管理土地利用过渡，使人类活动与保护目标保持一致，并在不断升级的土地使用压力中促进生态系统的复原力。

### 基于多智能体模型的生态脆弱区农村居民点空间优化——来自不同类型城镇的证据 Environmental Impact Assessment Review 
Spatial optimization of rural settlements in ecologically fragile regions based on a multi-agent model: Evidence from different types of towns

JCR分区：1区
