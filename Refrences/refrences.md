## 错误的思路  
一直以来，思路都错了 总是往研究的方法去思考，试图从中找到一个范式，一个统一的的研究方法，也就是一招鲜，但事实上，作为生态系统生态学，他是一个复合生态系统，任何一个因素的影响都会发生改变，其实这些改变在一般情况下都是正常，这些改变是反馈活动，无时无刻调整着生态系统的稳态，然而当发生一些变化时，会从这个稳态到另一个稳态，此时也是没问题的，有问题的知识，另一个状态是什么情况，反复无常呢？还是不能支撑现在系统结构，或者对于未来来说变得易受攻击。  
亿万年的演替下，大多生态系统已经稳定了，在这稳定下，她们能有效地运行下去，然而现在我们改造的能力太强了，这些反馈变成了正反馈，变得越来越不适合人类生存。  
其实当不符合其他物种生存时，人类的生存将会不适宜而来。
### (1)景观生态系统的自适应结构与功能的研究进展
  景观结构在不同尺度和不同地质环境有着不同的问题，地形地貌限制下，每个尺度的关系也是不一致的，而且在不同区域下，其主体的不同，其驱动因素是不一致的。（全球变化背景下景观生态适应性特征2017）
### （2）基于多源数据的城市公共空间活力影响因素研究—以上海市黄浦江滨水区为例（2021）
活力中的密度和稳定区域   韧性中的关系 
![image](https://github.com/user-attachments/assets/6f045c1c-41d8-4a83-90d6-6357c4d67e76)

### 使用ALE（累积局部效应）分析了不同农村土地覆盖类型对城市热岛效应（UHI）的影响  
`  import pandas as pd  
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
  plt.show()  `
  ![image](https://github.com/user-attachments/assets/527c0d0b-66ec-4120-a29d-930b0f506b9a)

  `import matplotlib as mpl
# 设置 matplotlib 图的默认大小为 9x6 英寸mpl.rc("figure", figsize=(9, 6))
# 调用 ale_plot 函数绘制 Accumulated Local Effects (ALE) 图ale_plot(    rf_model,                  # 传入机器学习模型（例如训练好的回归或分类模型）    X_test,                      # 数据特征集，用于生成 ALE 图    X_test.columns[:1],          # 选择要绘制 ALE 的特征列，这里选择第一个特征列    bins=20,                # 将特征值分成 20 个区间（箱数）    monte_carlo=True,       # 启用蒙特卡罗模拟，用于增加鲁棒性    monte_carlo_rep=100,    # 设置蒙特卡罗模拟的重复次数为 100    monte_carlo_ratio=0.6,  # 设置蒙特卡罗采样比例为 60%)`![image](https://github.com/user-attachments/assets/c94ac1eb-4ef7-40b0-9889-eaaf2c4666eb)

 单个特征的 ALE 绘图  
` ale_plot(model=rf_model, train_set=X_test, features=["infC"], monte_carlo=True, save_path='infC.pdf')`
 ![image](https://github.com/user-attachments/assets/d1255eaf-8308-417a-80f6-fc6c26a966f9)
`ale_plot(model=rf_model, train_set=X_test, features=["infP"], monte_carlo=True, save_path='infP.pdf')`
 ![image](https://github.com/user-attachments/assets/b7d9409c-abfc-41bd-b4e5-023c705c29fb)

这里展示的是不同取值下的加法局部效应（ALE）值及其相应的置信区间具体的值
`ale_eff = ale(X=X_test, model=rf_model, 
              feature=["sqft_lot", "sqft_living"], grid_size=100)`
![image](https://github.com/user-attachments/assets/2fb51f2b-ed8b-47fb-8027-c230bfb623b9)

#### 计算ALE（加法局部效应）值
`ale_eff = ale(
    X=X_test,            # 输入数据集 用于计算ALE的特征数据
    model=rf_model,              # 模型 model: 用于计算ALE的已训练模型，通常是一个回归或分类模型
    feature=["grade"]      # 特征 feature: 计算ALE的特征，这里是“grade”，表示需要分析的特征
)`
![4a7473a7fd3cbc9c49d957707e6812c3](https://github.com/user-attachments/assets/2ce34bdf-7952-4a16-9e41-2a731363e695)
