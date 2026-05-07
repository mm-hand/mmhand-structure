# 机械设计仓库使用规范 | Mechanical Design Repo Guide

本指南适用于使用 Fusion 360 进行设计，且仓库主要托管 “STEP 工程文件” 和 “STL 打印文件” 的团队。
This guide is for teams using Fusion 360, with repositories containing STEP and STL files.

---

## 1. 准备工作 / Prerequisites

1. **GitHub 账号** / A GitHub account.
2. **GitHub Desktop** (推荐 / Recommended).
3. **Git LFS (必须 / Mandatory)**: 
   - CAD文件是二进制文件，必须启用 LFS 否则会导致仓库过大无法推送。
   - CAD files are binary; LFS is required to prevent repository bloat.
   - 安装命令 / Install: `git lfs install`

---

## 2. 文件管理规则 / File Management Rules

### 🚫 禁止在文件名中加版本号 / No Version Numbers in Filenames
- **错误 / Wrong:** `bracket_v1.step`, `bracket_final_v2.step`
- **正确 / Right:** `mmhand.step`
- **原因:** Git 会自动记录历史。保持文件名不变可以避免装配体链接失效，并方便追踪零件演进。
- **Reason:** Git tracks history automatically. Constant naming prevents broken assembly links and enables clear version tracking.

### ⚠️ 二进制冲突预防 / Avoiding Binary Conflicts
- **原则:** 不要多人同时修改同一个文件。
- **操作:** 修改前在办公室，群组或 Issue 中声明：“正在修改 [零件名]”。
- **Principle:** No concurrent edits on the same file. 
- **Action:** Declare "Editing [Part Name]" in the group or Issue before starting.

---

## 3. 工作流 / Workflow

### A. 导出与保存 / Export & Save
1. 在 Fusion 360 中选择 **导出 (Export)**。
2. 格式选择 **STEP** (工程存档) 或 **STL** (3D打印)。
3. 将你修改好后的文件覆盖保存至本地仓库文件夹内的原文件。

### B. 提交与推送 / Commit & Push
1. 打开 **GitHub Desktop**。
2. 在 **Summary** 填写修改了什么（例如: `Update: Adjust motor mounting hole diameter`）。
3. 点击 **Commit to main**。
4. 点击 **Push origin**。

### C. 同步更新 / Sync
- 开始工作前，务必先执行 **Pull origin**，确保你拿到的是团队最新的设计。
- Always **Pull origin** before starting work to ensure you have the latest design.

---

## 4. 配置信息 (技术维护) / Configuration

仓库根目录必须包含以下两个文件以确保正常运行：

**[.gitattributes]** (用于启用 LFS):
```text
*.stp filter=lfs diff=lfs merge=lfs -text
*.step filter=lfs diff=lfs merge=lfs -text
*.stl filter=lfs diff=lfs merge=lfs -text
*.f3d filter=lfs diff=lfs merge=lfs -text
*.f3z filter=lfs diff=lfs merge=lfs -text
