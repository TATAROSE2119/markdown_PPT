---
marp: true
size: 16:9
paginate: true



style: |
  section {
    font-family: 'Microsoft YaHei', sans-serif;
    font-size:18px;
    line-height: 1.5;
    }
  section::before {
    content: "";
    position: absolute;
    top: 0;
    right: 0;
    left: 60;
    width: 120%;
    height: 120%;
    background: url('https://github.com/TATAROSE2119/markdown_PPT/blob/main/DiMSC/img/image.png?raw=true') no-repeat right;
    background-size: contain;
    opacity: 0.1; /* 设置背景图片透明度 */
    pointer-events: none;
    }



---
# A weighted subspace local averaging method and its applications in gearbox fault diagnosis
作者：Yifei Zhang

---

## **1.背景与研究意义**
- 研究问题
齿轮箱是旋转机械的重要部件，其故障会导致严重的机械失效。振动信号是诊断齿轮箱故障的主要依据，但这些信号通常受到强背景噪声和其他设备的脉冲干扰，导致传统方法（时间同步平均，TSA）提取的周期信号不够准确。
- TAS
将输入信号按旋转周期分割成多个等长段。
对这些段信号逐点求平均。
随着段数增加，噪声幅度减小，周期信号被增强。

- 局限
周期有限时，降噪效果受限
信号中存在周期性强脉冲干扰，TAS会保留，对只有白噪声的干扰降噪效果比较明显


![bg w:650 left:50% ](https://github.com/TATAROSE2119/markdown_PPT/blob/main2/WSLA/img/image1.png?raw=true)

---

## **2.论文核心方法WSLA**
- 1.采样信号预处理
采集振动信号和转速信号。
使用计算阶次跟踪（COT）将时域信号重采样为角域信号，确保各段信号同步。

![bg w:50 left:50% ](![alt text](image.png))
