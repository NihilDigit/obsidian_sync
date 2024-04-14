### 第三讲：频率、概率与基本概率分布

#### 1. 频率与概率
- **自定义函数**：在Python中，可以使用`def`关键字定义函数，如：
  ```python
  def function_name(parameters):
      expressions
  ```
  如果函数需要返回值，可以在`expressions`中使用`return`关键字。

- **计算频率示例**：以下是一个模拟抛掷硬币并计算正面朝上次数的示例：
  ```python
  import random

  def coin_trial():
      heads = 0
      for i in range(10):
          if random.random() <= 0.5:
              heads += 1
      return heads

  def simulate(n):
      trials = []
      for i in range(n):
          trials.append(coin_trial())
      return sum(trials) / n

  print(simulate(100))
  ```

#### 2. 常用库与模块简介
- **Pandas**：用于数据处理和分析，提供高级数据结构和各种分析工具。
- **Stats**：SciPy库中的统计数据分析模块，用于统计模型估计、执行统计测试等。

#### 3. 常用分布计算
- **二项分布**：
  - 累计概率：`binom.cdf(k=数值, p=概率, n=试验次数)`
  - 取某值的概率：`binom.pmf(k=数值, p=概率, n=试验次数)`
  - 示例：计算二项分布`B(100, 0.3)`的分布律：
    ```python
    from scipy.stats import binom
    import numpy as np

    x_s = np.arange(11)
    y_s = binom.pmf(k=x_s, p=0.3, n=11)
    print(y_s)
    ```

---

### 第四讲：分布律、概率密度函数、分布函数图像

#### 1. 绘图配置
- **plt.style.use()**：设置绘图背景。
- **plt.rcParams**：自定义图形的各种默认属性，如窗体大小、分辨率、线条宽度等。

#### 2. 绘图函数
- **plt.plot()**：用于绘制线性图。
- **plt.text()**：用于设置标签。
- **plt.fill_between()**：用于填充两个函数之间的区域。
- **plt.scatter()**：用于绘制散点图。

#### 3. 分布律与概率密度函数图像绘制
- **标准正态分布图像**：使用以下代码可以绘制标准正态分布的概率密度函数（PDF）和累积分布函数（CDF）图像：
  ```python
  import numpy as np
  import scipy.stats as stats
  import matplotlib.pyplot as plt
  import matplotlib.style as style

  style.use('fivethirtyeight')
  plt.rcParams["figure.figsize"] = (14, 7)
  plt.figure(dpi=100)

  plt.plot(np.linspace(-4, 4, 100), stats.norm.pdf(np.linspace(-4, 4, 100)))
  plt.fill_between(np.linspace(-4, 4, 100), stats.norm.pdf(np.linspace(-4, 4, 100)), alpha=.15)

  plt.plot(np.linspace(-4, 4, 100), stats.norm.cdf(np.linspace(-4, 4, 100)))

  plt.text(x=-1.5, y=.7, s="pdf (normed)", rotation=65, alpha=.75, weight="bold", color="#008fd5")
  plt.text(x=-.4, y=.5, s="cdf", rotation=55, alpha=.75, weight="bold", color="#fc4f30")

  plt.tick_params(axis = 'both', which = 'major', labelsize = 18)
  plt.axhline(y = 0, color = 'black', linewidth = 1.3, alpha = .7)

  plt.text(x = -5, y = 1.25, s = "Normal Distribution - Overview", fontsize = 26, weight = 'bold', alpha = .75)
