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
齿轮箱是旋转机械的重要部件，其故障会导致严重的机械失效。振动信号是诊断齿轮箱故障的主要依据，但这些信号通常受到强背景噪声和其他设备的脉冲干扰，导致传统方法（时间同步平均，TSA）提取的周期信号不够准确。\
- TAS的局限

