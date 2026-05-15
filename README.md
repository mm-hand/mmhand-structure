# 机械设计仓库使用规范 | Mechanical Design Repo Guide

本指南适用于使用 Fusion 360 进行设计，且仓库主要托管 “f3d 工程文件” 和 “STL 打印文件” 的团队。
This guide is for teams using Fusion 360, with repositories containing F3D and STL files.

---

## 1. 准备工作 / Prerequisites

1. **GitHub 账号** / A GitHub account.
2. **Git LFS (必须 / Mandatory)**: 
   - CAD文件是二进制文件，必须启用 LFS 否则会导致仓库过大无法推送。
   - CAD files are binary; LFS is required to prevent repository bloat.
   - 安装命令 / Install: `git lfs install`

---

## 2. 文件管理规则 / File Management Rules

### 🚫 禁止在文件名中加版本号 / No Version Numbers in Filenames
- **错误 / Wrong:** `bracket_v1.f3d`, `bracket_final_v2.f3d`
- **正确 / Right:** `mmhand.f3d`
- **原因:** Git 会自动记录历史。保持文件名不变可以避免装配体链接失效，并方便追踪零件演进。
- **Reason:** Git tracks history automatically. Constant naming prevents broken assembly links and enables clear version tracking.

### ⚠️ 二进制冲突预防 / Avoiding Binary Conflicts
- **原则:** 不要多人同时修改同一个文件。
- **操作:** 修改前在群组或 Issue 中声明：“正在修改 [零件名]”。
- **Principle:** No concurrent edits on the same file.
- **Action:** Declare "Editing [Part Name]" in the group or Issue before starting.

---

## 3. 完整工作流 / Complete Workflow

### 🔄 每次开始工作前 - 拉取最新版本 / Before Work - Pull Latest

**方法一：GitHub Desktop**
1. 打开 GitHub Desktop
2. 确保左上角显示正确的仓库
3. 点击 **Fetch origin**（如果有更新，按钮会变成 **Pull origin**）
4. 再次点击 **Pull origin** 拉取最新文件

**方法二：命令行 (PowerShell / Terminal)**
```bash
git pull
```

---

### 📝 修改设计文件 / Modify Design Files

1. 在 Fusion 360 中打开仓库文件夹内的 f3d 文件
2. 完成修改后 **保存 (Ctrl+S)**
3. 如需 3D 打印，导出 STL 并覆盖仓库内的旧 STL 文件

---

### 📤 提交更改 / Commit Changes

**方法一：GitHub Desktop**
1. 打开 GitHub Desktop
2. 左侧会显示所有更改的文件
3. 在左下角 **Summary** 输入修改说明（例如: `Update: Adjust motor mounting hole diameter`）
4. 可选：在 **Description** 输入详细说明
5. 点击 **Commit to main**
6. 点击 **Push origin** 上传到远程仓库

**方法二：命令行 (PowerShell / Terminal)**
```bash
# 查看更改了哪些文件
git status

# 添加所有更改的文件
git add .

# 或者只添加特定文件
git add 文件夹/文件名.f3d

# 提交并填写修改说明
git commit -m "Update: Adjust motor mounting hole diameter"

# 推送到远程仓库
git push origin
```

---

### ⚠️ 如果推送被拒绝（远程有新版本）/ If Push Rejected

**方法一：GitHub Desktop**
1. 点击 **Pull origin** 拉取远程更新
2. 如果有冲突，GitHub Desktop 会提示
3. 解决冲突后再次 Commit 和 Push

**方法二：命令行**
```bash
# 拉取远程更新并合并
git pull

# 如果有冲突，解决后执行
git add .
git commit -m "Resolve merge conflict"
git push
```

---

## 4. 常见问题 / Common Issues

### ❌ 问：推送到一半失败了 / Push fails halfway
**答：** 通常是文件太大。确保已经运行过 `git lfs install`，并且仓库根目录有正确的 `.gitattributes` 文件。

### ❌ 问：GitHub Desktop 提示 "Large file detected"
**答：** 说明 LFS 没有正确配置。联系仓库管理员检查 `.gitattributes` 文件。

### ❌ 问：我不知道其他人改了什么
**答：** 运行 `git log`（命令行）或在 GitHub Desktop 中点击 **History** 查看所有提交记录。

### ❌ 问：不小心改了别人的文件怎么办
**答：** 
```bash
# 丢弃某个文件的所有本地修改
git checkout -- 文件路径/文件名.f3d
```

---

## 5. 配置信息 (仅限仓库管理员) / Configuration (Repo Admin Only)

仓库根目录必须包含以下 `.gitattributes` 文件：

```text
*.f3d filter=lfs diff=lfs merge=lfs -text
*.f3z filter=lfs diff=lfs merge=lfs -text
*.stl filter=lfs diff=lfs merge=lfs -text
```

新人无需手动配置，克隆仓库时会自动应用这些规则。

---

## 6. 关于 f3d 文件 / About f3d Files

- f3d 是 Fusion 360 的原生格式，**在线版本不上传设计历史**（草图、特征、装配关系等）
- 团队统一使用 f3d 作为唯一的工程存档格式
- f3d 是二进制文件，必须通过 Git LFS 管理
- f3d is Fusion 360's native format, **containing full design history**
- The team uses f3d as the **only** engineering archive format
- f3d files are binary and must be managed via Git LFS

---

## 7. 快速参考卡片 / Quick Reference

| 操作 | GitHub Desktop | 命令行 |
|------|----------------|--------|
| 拉取最新 | Fetch origin → Pull origin | `git pull` |
| 查看更改 | 左侧 Changes 面板 | `git status` |
| 提交 | Summary → Commit | `git add .` → `git commit -m "说明"` |
| 上传 | Push origin | `git push` |
| 查看历史 | History 标签页 | `git log` |
