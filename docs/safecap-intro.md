# SafeCap：开源联锁/信号形式化验证工具简析（助力事故预防与复盘）

本文简要介绍 SafeCap——一个面向铁路联锁/信号系统的开源形式化验证工具。它可用于在工程阶段发现并避免信号逻辑缺陷，为事故预防、事故成因分析与整改验证提供技术支撑。

## SafeCap 是什么
SafeCap 是一套面向铁路信号与联锁（尤其是 SSI 等）的建模、仿真与形式化验证工具。与传统"基于场景的测试"不同，形式化验证可对关键安全属性进行"穷尽式"逻辑证明，从而更系统地发现潜在风险。

- 开源仓库：ennessione/safecap  
  https://github.com/ennessione/safecap

## 为什么与"事故处理/预防"相关
- 事故预防：在投运前对联锁/信号逻辑进行形式化验证，提前排除可能导致冲突/追尾/误分路等的逻辑缺陷。
- 事故复盘：通过对既有布局与逻辑的建模与自动检查，辅助定位事故/事件的根因，并验证修复方案的有效性。
- 工程可用：强调工程师可读的诊断与规模化工程数据导入，降低"形式化方法"在实际项目中的门槛。

## 核心架构要点（概览）
- 模型与布局建模：站场/线路布局建模、信号与道岔等对象的关系建模。
- 信号逻辑解析与约束：将信号/联锁逻辑转化为可被求解器处理的约束。
- 形式化验证引擎：利用定理证明/求解器自动检验关键安全性质（如互斥、防冲突、道岔位移安全等）。
- 工程数据导入与诊断输出：支持将大规模工程数据导入，并生成工程师可读的诊断/报告。

## 快速上手（方向性指引）
1. 克隆仓库并阅读 README/示例工程：  
   https://github.com/ennessione/safecap
2. 按示例导入站场/线路与信号逻辑数据，运行验证。
3. 结合工程规则定义关键安全属性（如区段占用互斥、道岔转换保护等），查看诊断输出并迭代修正。
4. 将工具纳入联锁方案评审与回归验证流程，形成"变更—验证—发布—复盘"的闭环。

## 典型应用场景
- 新建/改扩建站场的联锁方案校核与发布前把关
- 轨道电路/道岔异常相关的逻辑一致性检查
- 事故复盘与整改方案的快速验证
- 面向合规与安全评估的证据链补强

## 延伸阅读与参考
- GitHub 仓库（源码与说明）
  - https://github.com/ennessione/safecap
- 工程与媒体介绍
  - SafeCap Automated Verification of Railway Signalling（Rail Engineer）  
    https://www.railengineer.co.uk/safecap-automated-verification-of-railway-signalling/
- 学术资料（示例）
  - Formal verification of railway interlocking and its safety case  
    https://www.researchgate.net/profile/Alexander-Romanovsky/publication/364958857_Formal_verification_of_railway_interlocking_and_its_safety_case/links/636117d86e0d367d91e7b5b0/Formal-verification-of-railway-interlocking-and-its-safety-case.pdf
  - Practical Verification of Railway Signalling Programs（含 SafeCap 实践）  
    https://eprints.ncl.ac.uk/file_store/production/276164/21DFDC5C-1D7D-4F81-B849-BDF2B4819C98.pdf

> 说明：以上为基于公开资料的简要整理，旨在提供入门级概览。实际部署与合规要求需结合具体线路、规范与审查流程执行。  
> 整理：@yangyudtx
