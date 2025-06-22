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

### (9) 
 
