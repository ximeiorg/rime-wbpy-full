# 曦码 · 五笔拼音方案库（rime-wbpy-full）

一套面向 Rime 与曦码（Xime）的多方案合集：五笔 86/98、简体拼音、四种双拼，以及把它们压到
14 / 17 / 18 键的「合并键」版本。所有方案共用同一份码表与词库，切换方案不会重复下载，输入习惯
跨平台一致。

> 本仓库 fork 自 [KyleBing/rime-wubi86-jidian](https://github.com/KyleBing/rime-wubi86-jidian)，
> 码表沿用极点五笔 86，输入习惯接近老极点五笔；原方案说明见下文「一、前言」及之后各节。

## 包含哪些方案

### 五笔

| schema_id | 名称 | 键数 | 说明 |
| --- | --- | --- | --- |
| `wubi86` | 五笔86 | 26 | 默认方案，四码唯一即上屏 |
| `wubi86_pinyin` | 五笔拼音 | 26 | 五笔为主、拼音反查兜底 |
| `wubi86_trad` | 简入繁出 | 26 | 打简体出繁体 |
| `wubi86_trad_pinyin` | 拼音混输繁体 | 26 | 繁体的五笔拼音混输 |
| `wubi86_14jian` / `_17jian` / `_18jian` | 五笔合并键 | 14 / 17 / 18 | 相邻两字母并键，见「八」 |
| `wubi98` | 五笔98 | 26 | 98 版码表，未列入默认方案表 |

### 拼音与双拼

| schema_id | 名称 | 键数 | 说明 |
| --- | --- | --- | --- |
| `pinyin_simp` | 简体拼音 | 26 | 全拼 |
| `pinyin_14jian` / `_17jian` / `_18jian` | 拼音合并键 | 14 / 17 / 18 | 见「七」 |
| `double_pinyin` | 自然码双拼 | 26 | 雾凇同源键位 |
| `double_pinyin_flypy` | 小鹤双拼 | 26 | |
| `double_pinyin_mspy` | 微软双拼 | 26 | `ing` 在分号键；Xime 无分号键，额外支持 `ing→n` |
| `double_pinyin_sogou` | 搜狗双拼 | 26 | 同上 |
| `double_pinyin_{14,17,18}jian` | 自然码合并键 | 14 / 17 / 18 | 见「九」 |
| `double_pinyin_flypy_{14,17,18}jian` | 小鹤合并键 | 14 / 17 / 18 | 见「九」 |
| `t9_pinyin` | 拼音九键 | 9 | 需宿主内置 librime-t9，未列入默认方案表 |

### 其他

| schema_id | 名称 | 说明 |
| --- | --- | --- |
| `english` | English | 纯英文输入，未列入默认方案表 |
| `numbers` | 大写数字 | 壹贰叁肆…，见「五、8」，未列入默认方案表 |
| `handwriting` | 手写输入 | 依赖宿主提供手写面板，未列入默认方案表 |

> 标注「未列入默认方案表」的方案不在 `default.custom.yaml` 的 `schema_list` 里，用到时把
> `- schema: <id>` 加进该列表再重新部署即可。其余方案装好就能在方案菜单（`F4` / `` Ctrl+` ``）里选。

## 快速开始

1. 从 [Releases](../../releases) 下载 `rime-wbpy-full-<版本>.zip`，同目录还有 `.sha256` 可校验；
2. 解压后，把 **`rime-wbpy-full-<版本>/` 里面的所有文件**复制到 Rime 用户目录：
   - macOS：`~/Library/Rime`（鼠须管）
   - Windows：`%APPDATA%\Rime`（小狼毫）
   - Linux：`~/.config/ibus/rime`（ibus-rime）或 `~/.local/share/fcitx5/rime`（fcitx5）
3. 重新部署（鼠须管菜单「重新部署」；小狼毫右键托盘图标 →「重新部署」）；
4. `F4` 或 `` Ctrl+` `` 切方案。各平台细节见「四、安装」。

**曦码 Xime（Android）**：设置 → 输入方案 → 导入方案包，选中这个 zip 即可；部署后在方案列表里勾选
要用的方案。合并键与双拼方案会自动套用对应的键盘布局（绑定写在仓库自带的 `xime.custom.yaml` 里），
不需要额外配置。

## 相对上游新增了什么

- **合并键方案**：把横向相邻两键并成一键（`qw`/`er`/`ty`…），键更大、误触更少。拼音、五笔、双拼
  三系都有 14 / 17 / 18 键版本，共用词库、各自独立 prism（见「七」「八」「九」）。
- **四套双拼**：自然码、小鹤、微软、搜狗，键位与 [rime-double-pinyin](https://github.com/rime/rime-double-pinyin) 同源。
- **九键与英文**：`t9_pinyin`（配合 librime-t9，含日期/农历候选）、`english` 纯英文方案。
- **曦码键盘适配**：`xime.custom.yaml` 把合并键布局绑到方案上，并补齐 App 旧版本缺失的四行布局
  （`shift` / `delete` 与空格、回车、中英切换）。

## 目录

- 一、前言　二、外观　三、文件说明　四、安装　五、使用说明　六、常见自定义功能
- 七、合并键方案（14 / 17 / 18 键）　八、五笔 14 / 17 / 18 键　九、双拼

---

## 一、前言

Rime 是个输入法集合，涵盖常用的三个平台 Windows、macOS、Linux，三个平台对应的输入法如上图。

该输入法具有高度可定制化的特性，输入法通过读取不同的配置文件，来实现不同输入方式：全拼、双拼、五笔、仓颉，设置不同的输入习惯：二三候选、回车清码、z键拼音反查等等。

正因为如此高的可自定义性，拉高了它的使用门槛。为了让更多的五笔用户更省心地使用 Rime 输入五笔，就有了这个86五笔输入方案，它的输入习惯比较接近于之前的极点五笔，码表也是使用的极点五笔的码表。

由于三个平台使用的码表配置文件是一样的，所以能够在多个平台间保持同一种输入习惯，打起字来也会比较舒服。

该码表词条不是很多，很多专业性的词语可能会没有，需要自己添加，可以使用配套工具「五笔词条工具」来完成词条的添加。


## 二、不同平台的输入法外观


__macOS__

__Windows__

皮肤可以通过修改 `weasel.custom.yaml` 文件内的 color_scheme 实现修改，对应正文的颜色方案

```yaml
patch:
  style:
    color_scheme: WhiteAqua  # 匹配正文的颜色方案，对应正文的颜色方案名
```

__Linux: Ubuntu__



## 三、文件说明

```bash
.
├── .github/workflows/release.yml            # CI - Release 发布时自动打包上传
├── README.md                               # 当前说明文档
├── default.yaml                            # 配置文件 - 默认配置
├── default.custom.yaml                     # 配置文件 - 自定义一些输入法的功能：标点，二三候选等
├── squirrel.custom.yaml                     # 配置文件 - 鼠须管（for macOS）输入法候选词界面
├── trime.custom.yaml                       # 配置文件 - 同文（for Android）输入法候选词界面
├── xime.custom.yaml                        # 配置文件 - Xime 合并键布局（四行 rows + 功能键）与方案绑定
├── english.schema.yaml                     # 输入方案 - 纯英文
├── english.dict.yaml                       # 词库文件 - 英文词库
├── numbers.schema.yaml                     # 输入方案 - 大写数字
├── pinyin_simp.dict.yaml                   # 词库文件 - 简体拼音主码表
├── pinyin_simp_ext.dict.yaml               # 词库文件 - 拼音扩展词库（被主码表 import_tables 引入）
├── pinyin_simp.schema.yaml                 # 输入方案 - 简体拼音
├── pinyin_14jian.schema.yaml               # 输入方案 - 拼音14键（合并键）
├── pinyin_17jian.schema.yaml               # 输入方案 - 拼音17键（合并键）
├── pinyin_18jian.schema.yaml               # 输入方案 - 拼音18键（合并键）
├── double_pinyin.schema.yaml               # 输入方案 - 自然码双拼（26键）
├── double_pinyin_flypy.schema.yaml         # 输入方案 - 小鹤双拼（26键）
├── double_pinyin_mspy.schema.yaml          # 输入方案 - 微软双拼（26键）
├── double_pinyin_sogou.schema.yaml         # 输入方案 - 搜狗双拼（26键）
├── double_pinyin_14jian.schema.yaml        # 输入方案 - 自然码双拼14键（合并键）
├── double_pinyin_17jian.schema.yaml        # 输入方案 - 自然码双拼17键（合并键）
├── double_pinyin_18jian.schema.yaml        # 输入方案 - 自然码双拼18键（合并键）
├── double_pinyin_flypy_14jian.schema.yaml  # 输入方案 - 小鹤双拼14键（合并键）
├── double_pinyin_flypy_17jian.schema.yaml  # 输入方案 - 小鹤双拼17键（合并键）
├── double_pinyin_flypy_18jian.schema.yaml  # 输入方案 - 小鹤双拼18键（合并键）
├── symbols.yaml                            # 配置文件 - 特殊符号
├── wubi86.schema.yaml                      # 输入方案 - 五笔86
├── wubi86.dict.yaml                        # 词库文件 - 五笔86主码表
├── wubi86_pinyin.schema.yaml               # 输入方案 - 五笔拼音混输
├── wubi86_14jian.schema.yaml               # 输入方案 - 五笔86 14键（合并键）
├── wubi86_17jian.schema.yaml               # 输入方案 - 五笔86 17键（合并键）
├── wubi86_18jian.schema.yaml               # 输入方案 - 五笔86 18键（合并键）
├── wubi86_trad.schema.yaml                 # 输入方案 - 五笔简入繁出
├── wubi86_trad_pinyin.schema.yaml          # 输入方案 - 拼音混输繁体
├── wubi86_user.dict.yaml                   # 词库文件 - 用户私人词库
├── wubi86_extra.dict.yaml                  # 词库文件 - 扩展词库
├── wubi98.schema.yaml                      # 输入方案 - 五笔98
├── wubi98.dict.yaml                        # 词库文件 - 五笔98码表
├── handwriting.schema.yaml                 # 输入方案 - 手写输入
├── t9_pinyin.schema.yaml                   # 输入方案 - 拼音九键（依赖 librime-t9 插件）
├── lua/
│   ├── README.md                           # 脚本来源说明
│   ├── convert_ar_num_to_zh.lua            # 脚本 - 阿拉伯数字转中文（被 date_translator 引用）
│   ├── date_translator.lua                 # 脚本 - 日期 / 时间 / 星期 / 时间戳
│   ├── english_spacing.lua                 # 脚本 - 英文方案词间自动空格
│   └── uuid.lua                            # 脚本 - 输出 UUID
└── macOS-双击复制文件内容.command           # 脚本 - macOS 一键复制配置到 Rime 目录
```

## 四、安装

### 1. 鼠须管（macOS）
去 Rime 官网下载鼠须管，按步骤安装即可

1. 下载本仓库的五笔配置文件
2. macOS 上的 鼠须管 配置文件存放目录是 `~/Library/Rime`，把下载后的`rime-wubi`内的所有文件移到 `Rime` 目录中，
3. 点击状态栏上的输入法图标，下拉菜单中选择 <kbd>部署</kbd> (英文是<kbd>Deploy</kbd>），或者可以直接使用快捷键 <kbd>control</kbd> + <kbd>option</kbd> + <kbd>~</kbd>

> **注意：** `Rime` 目录下的 `Build` 目录是程序生成的，不要把配置文件放在那里面，无视它即可。

放的时候目录结构是这样的：
```bash
~/Library/
└── Rime
    ├── 该项目中的文件
    ├── 该项目中的文件
    ├── 该项目中的文件
    ├── ...
    ├── ...
```

> **注意**：对于不熟悉命令行操作的朋友， `~` 代表的是当前用户的主目录，比如我的用户名是 `kyle`, `~` 就代表 `/Users/kyle/` 这个绝对路径。
> 需要将你下载的文件放入 `/Users/你用户名/Library/Rime` 这个目录下，了然否？


### 2. 小狼毫（Windows）

Windows 中的配置方法：
1. 右击状态栏中的小狼毫输入法图标，选择 <kbd>用户文件夹</kbd>
2. 把该项目中的文件复制到里面
3. 右击状态栏中的小狼毫输入法图标，选择 <kbd>重新部署</kbd> 即可


### 3. ibus-rime (Ubuntu)

执行下面指令安装 `ibus-rime` 输入法

```bash
sudo apt-get install ibus-rime
```

ubuntu 的配置文件目录在 `~/.config/ibus/rime/`

### 4. 小企鹅输入法 (Android)
插件： 需要到 fcitx5-android 的发布页面下载 rime 支持插件
配置方法：
1. 使用 adb 把文件 push 到`/sdcard/Android/data/org.fcitx.fcitx5.android/files/data/rime`
2. 打开输入法输入界面的工具菜单 -> 重载配置

### 5. Xime 输入法 (Android)
Xime 输入法（仓库地址：[https://github.com/ximeiorg/Xime](https://github.com/ximeiorg/Xime)）已内置本仓库的方案，无需下载或手动配置，安装 Xime 后即可直接使用。


## 五、使用说明

### 1. 选项菜单
在输入状态时，<kbd>control</kbd> + <kbd>0</kbd> 或者 <kbd>shift</kbd> + <kbd>control</kbd> + <kbd>0</kbd> 弹出菜单

### 2. 菜单内容
弹出的菜单中，处于第一位的是当前使用的输入法方案，其后跟着是该方案中的输入法菜单，有【半角 - 全角】【简 - 繁】等常见功能菜单，再后面是其它可选的输入法方案，对应 `default.custom.yaml` 中 `schema_list` 字段内容

### 3. 默认二三候选
默认的二三候选是 <kbd>;</kbd> <kbd>'</kbd> 两个键

### 4. 候选翻页
方向 <kbd>上</kbd><kbd>下</kbd>、<kbd>-</kbd> <kbd>=</kbd>、<kbd>[</kbd> <kbd>]</kbd>

### 5. 临时拼音输入
在忘了某字的五笔编码时，<kbd>z</kbd>键可以进入临时拼音输入模式

### 6. 支持 简入繁出
是以切换输入方案的形式实现的，使用时，调出菜单，选择 `简入繁出` 方案即可
简繁转换的功能能实现：
- 转繁体
- 转香港繁体
- 转台湾繁体
具体可以看这个文件内的说明： [wubi86_trad.schema.yaml](wubi86_trad.schema.yaml)
> 以不切换文字的形式使用只是暂时转繁，换个程序就会恢复简体了。如果你想一直使用简入繁出就选择 「简入繁出」这个方案

### 7. 系统 `时间`、`日期` 和 `星期`
由 `lua/date_translator.lua` 提供（在 `wubi98`、`简入繁出` 两套繁体方案中启用；触发词可在方案里改）：
- `rq` 输出日期：`2019-06-19`、`2019/06/19`、`2019年6月19日` 等
- `sj` 输出时间：`10:00`、`10:00:00`、`上午 10:00`
- `xq` 输出星期：`星期四`、`礼拜四`、`周四`
- `dt` 输出 ISO 日期时间，`ts` 输出时间戳，`rqzh` / `rqen` 输出中文 / 英文日期

> 五笔 86 系方案（`wubi86`、`wubi86_pinyin` 等）没有挂这个脚本；九键 `t9_pinyin` 的日期功能由
> librime-t9 插件提供（`77`→日期、`75`→时间），见「六、5」的说明。

### 8. 支持大写数字输入：壹贰叁肆伍陆
本库中包含一个可以输入大写数字的方案，名叫 `大写数字`，呼出菜单选择该方案即可。
在这个模式下：具体可以看源文件 `numbers.schema.yaml`


| 键           | 对应值             | | 键 (按住 shift) | 对应值            |
|-------------|--------------------|---|-----------|-------------------|
| 1234567890  | 壹贰叁肆伍陆柒捌玖零  | | 1234567890 | 一二三四五六七八九〇  |
| wqbsjfd.    | 万仟佰拾角分第点     | | wqbsjfd.   | 万千百十角分点       |
| z           | 整之               | | z          | 整之               |
| y           | 元月亿             | | y          | 元月亿             |

### 9. 特殊字符快捷输入
默认是关闭的，具体可以查看 wiki 中「如何启用 `/fh` 这种特殊符号输入」。


## 六、常见自定义功能
所有配置说明都在配置文件中说明了，如果有其它问题可以在 `issue` 中提出。

### 1. 回车清码
默认是开启的
想要关闭，打开 `default.custom.yaml` 文件，找到下面这行，在前面添加 `#` 即可，如下

```yaml
      # 回车清码
      - { when: composing, accept: Return, send: Escape }
```

### 2. 空码时自动清码
默认配置是不会自动清的，想要自动清码，修改文件  `wubi86.schema.yaml` 中 `speller` 这一栏，将前面的 `#` 号删除即可，如下
```yaml
  auto_clear: max_length                # 空码时自动清空
```

### 3. 编码提示
默认是关闭的，
想要开启，打开 `wubi86.schema.yaml` 编辑 `translator` -> `comment_format` 改成如下即可

```yaml
  comment_format: 
#    - xform/.+//                       # 注释掉该行，即可显示词条编码
```

### 4. 关于手动造词功能
一种方式是，手动往词库中添加词组，并重新部署
> 这个操作要注意的是词组与编码之间的符号是`tab`，写错了这个词是不会被识别的

另一种是使用工具。Rime 默认是没有词条管理工具的，所以就写了一个工具，基本满足日常需求，如果有什么其它需要的功能，可以跟我说。

### 5. Lua 脚本扩展
本项目将 Lua 脚本统一放在 `lua/` 目录，并在对应的 `*.schema.yaml` 中引用：
- `uuid.lua`：输入 `*uuid` 输出 UUID（`lua_translator@*uuid`，多数方案引用）；
- `date_translator.lua`：输出日期 / 时间 / 星期 / ISO 日期时间 / 时间戳 / 中英文日期，
  触发词默认 `rq` `sj` `xq` `dt` `ts` `rqzh` `rqen`（可在方案里配置 `date_translator:` 段覆盖），
  在 `wubi98`、`wubi86_trad`、`wubi86_trad_pinyin` 中引用；
- `convert_ar_num_to_zh.lua`：阿拉伯数字转中文，被 `date_translator.lua` 内部 `require`；
- `english_spacing.lua`：英文方案词间自动加空格（`lua_filter@*english_spacing`，见 `english.schema.yaml`，
  由方案开关 `english_spacing` 控制，默认开）。

> 注：九键方案（`t9_pinyin.schema.yaml`）的日期提示不再使用 Lua 脚本，改由 librime-t9 插件的 `t9_date_translator`（C++）提供，`lua/date_hint.lua` 已移除。支持 `rq/sj/xq/dt/ts/nl` 系统触发词（如 `77`→rq、`75`→sj，对应日期/时间/星期/日期时间/时间戳/农历）与「今天/今日/明天/明日/昨天/昨日」中文触发词（如 `518`（j't 简拼）或 `5468426`（jintian 全拼），输出「今天(1月15日)」形式的复合日期候选），并内置公历转农历（覆盖 1899–2100 年）；数字 preedit 的拼音显示由 `t9_filter` 完成。该方案需 librime-t9 插件支持（如 Xime 输入法内置）。

### 6. 开启自动造词

> **注意**: 这样会关闭自动上屏，顶字上屏的功能。

需要修改 `wubi86.schema.yaml` 下面几个内容

```bash
speller:
  # max_code_length: 4                 # 四码上屏
  auto_select: false                   # 自动上屏

translator:
  enable_sentence: true                # 句子输入模式
  enable_user_dict: true               # 是否开启用户词典（用户词典记录动态字词频，用户词）
  enable_encoder: true

```

## 七、合并键方案（14 / 17 / 18 键）

`pinyin_14jian` / `pinyin_17jian` / `pinyin_18jian` 是三套**合并键**拼音方案：
把横向相邻的两个字母并成一个键（例如 `q` 和 `w` 并成 `qw`），键更少、键面更大、误触更少。
三套方案与 `pinyin_simp` 共用同一份词典（`pinyin_simp.dict.yaml`），不额外需要词库。

| 方案 | 分组 | 键数 |
| --- | --- | --- |
| `pinyin_14jian` | `qw/er/ty/ui/op`、`as/df/gh/jk/l`、`zx/cv/bn/m` | 14 |
| `pinyin_17jian` | `q/we/rt/yu/i/op`、`a/sd/fg/h/jk/l`、`z/xc/v/bn/m` | 17 |
| `pinyin_18jian` | `q/we/rt/y/u/io/p`、`a/sd/fg/h/jk/l`、`z/xc/v/bn/m` | 18 |

**怎么做到「少键还能打出正确候选」**：前端点击合并键时只上送组内**第一个字母**（代表字母），
所以输入侧只有那几个字母。方案侧再用两行拼写运算，把词库里的拼音码折成「按键序列码」：

```yaml
speller:
  algebra:
    # ... 与 pinyin_simp 相同的简拼 / 容错规则 ...
    - derive/(.*)/\U$1/                                        # ① 复制一份大写码
    - xlit/QWERTYUIOPASDFGHJKLZXCVBNM/qqeettuuooaaddggjjlzzccbbm/   # ② 逐位折成代表字母（14 键）
```

① `derive` 造出的大写副本只用于折叠，原始小写码保留；② `xlit` 只映射大写字母，所以不会动到原码。
以 14 键为例：`ni` → `NI` → `BI` → `bu`，而 `bi` 也折成 `bu`，两个音节在按键序列上重合，
最后由 `pinyin_simp` 的词频在候选层分出先后。这正是九键（`t9_pinyin`）把字母映射成数字的同一套思路。

**与前端配合**：本仓库自带 `xime.custom.yaml`，把合并键布局绑到方案上——
`qwerty_14 → pinyin_14jian + wubi86_14jian + 双拼 14 键`、
`qwerty_17 → pinyin_17jian + wubi86_17jian + 双拼 17 键`、
`qwerty_18 → pinyin_18jian + wubi86_18jian + 双拼 18 键`。

**四行布局在这里写全，不依赖 App 内置那份**。Xime 2.5.0～3.5.0 内置的 `qwerty_14/17/18` 只有
三行字母键：没有 `space` / `enter` / `mode_change`，`shift` 与 `delete` 只靠「整行一个功能键都没有
就自动补」的兜底塞进**第二行两端**——等于没有空格键，键盘也没法正常用。
（App 3.5.1 起内置布局已补全；本文件仍写全，是为了让旧版本装上方案包也能用。）
所以 `xime.custom.yaml` 里把 `layout.rows` 与五个功能键的手势都补全了：

| 行 | 14 键 | 17 / 18 键 |
| --- | --- | --- |
| 1 | `qw er ty ui op` | `q we rt yu i op` / `q we rt y u io p` |
| 2 | `as df gh jk l` | `a sd fg h jk l` |
| 3 | `shift zx cv bn m delete` | `shift z xc v bn m delete` |
| 4 | `mode_change comma space earth enter` | 同左 |

`keys` 是**按键 id 逐个覆盖**的（不是整段替换），所以字母键与合并键仍沿用内置定义，
这里只补 `shift` / `delete` / `mode_change` / `space` / `enter`。功能键与字母键同宽（`width: 1`），
因为行 3 的键数接近行 1，同宽时列对齐最好看。

改分组时**必须同时改三处**：方案里的 `xlit` 目标串、App 内置 `xime.yaml` 的 `layout.rows`、
以及本仓库 `xime.custom.yaml` 的 `layout.rows`（`xlit` 按 QWERTY 顺序逐位给出 26 个字母的代表
字母，`rows` 的子数组决定哪些字母并在一起，三边一个字都不能错）。

**反查**：本仓库没有笔画词库（`stroke`），所以三套方案用 `` ` `` 前缀反查**五笔码**
（`` `wq `` → 码为 `wq` 的字，注释给出拼音），桌面端可用；手机端没有 `` ` `` 键，用不到也不影响输入。

## 八、五笔 14 / 17 / 18 键（合并键）

三套方案把同一套合并键机制套在五笔上，分组与同名拼音方案完全一致，共用 `wubi86` 码表；
布局沿用 `qwerty_14/17/18`，绑定写在仓库的 `xime.custom.yaml` 里。

| 方案 | 分组 | 键数 |
| --- | --- | --- |
| `wubi86_14jian` | `qw/er/ty/ui/op`、`as/df/gh/jk/l`、`zx/cv/bn/m` | 14 |
| `wubi86_17jian` | `q/we/rt/yu/i/op`、`a/sd/fg/h/jk/l`、`z/xc/v/bn/m` | 17 |
| `wubi86_18jian` | `q/we/rt/y/u/io/p`、`a/sd/fg/h/jk/l`、`z/xc/v/bn/m` | 18 |

拼音合并后还有“音节”这层冗余兜底，五笔没有——码就是任意字母序列，一合并真撞。
按 `wubi86` + `wubi86_extra` 共 9.1 万条，实测/统计如下：

| 指标 | 26 键（`wubi86`） | 14 键 | 17 键 | 18 键 |
| --- | --- | --- | --- | --- |
| 25 个一级简码的首选变化 | — | **1 个**（`u` 产→不） | **4 个**（`s` 要→在、`w` 人→有、`x` 经→以、`y` 主→产） | **3 个**（`s`、`w`、`x`） |
| 唯一码占比（四码自动上屏） | 87.5% | 32.7% | 45.0% | 50.9% |
| 原码首选仍排第一（按码数，静态估算） | 100% | 35.5% | 47.6% | 54.8% |
| 前 3 位内命中（同上） | 100% | 69.9% | 81.2% | 86.7% |

> 一级简码那行是 `rime_console` 逐键实测；后三行是拿码表按码数统计的估算值（词频并列时
> 实际顺序由词典条目顺序决定，会有出入）。

有个反直觉的地方：**14 键唯一码最低，一级简码却几乎不乱**。因为一级简码本身就是最高频的字，
合并后撞在一起时通常还是它权重最高；掉档主要发生在三码、四码的常用字与词组上：

| 字/词 | 原码 | 26 键 | 14 键 | 17 / 18 键 |
| --- | --- | --- | --- | --- |
| 你好 | `wqvb` | 首选 | `qqcb` → 首选变「铯」 | `wqvb` → 首选（与原码相同） |
| 中国 | `khlg` | 首选 | `jglg` → 第 2（「最惠国」在前） | `jhlf` → 第 2（「跳转」在前） |
| 我 | `trnt` | 首选 | `tebt` → 第 2（「秀发」在前） | `rrbr` → 第 3（「白发」「播发」在前） |
| 好 | `vbg` | 首选 | `cbg` → 掉出前三（生僻字占位） | `vbf` → 首选 |

结论：**17 键是五笔这边的甜点**（少 8 个键，一级简码只错 4 个、前 3 位仍有八成），
14 键要能接受更多翻页，适合只想在超大键面下慢慢打的人。

两处刻意的取舍（三套一致）：

1. **放开单键的用户词典**。`wubi86.schema.yaml` 里有 `disable_user_dict_for_patterns: ^[a-z]$`，
   一级简码完全不参与学习；这三套方案去掉这条，被顶掉的简码打几次就能学回来。
2. **键面没有字根提示**。合并键横跨两个字母，`qwerty_14/17/18` 的键面只标 `qw`/`we`/`io`
   这样的组合名。需要字根提示的话，得另做一套带 `swipe_down` 字根的布局段。

## 九、双拼

四套 26 键双拼 + 六套合并键双拼，全部与 `pinyin_simp` 共用同一份词库（各用独立 prism，
不复制码表）。键位映射表与 [rime-double-pinyin](https://github.com/rime/rime-double-pinyin) 同源
（雾凇拼音用的也是这份），本仓库只替换了词典和引擎外壳。

| 方案 | schema_id | 键数 |
| --- | --- | --- |
| 自然码双拼 | `double_pinyin` | 26 |
| 小鹤双拼 | `double_pinyin_flypy` | 26 |
| 微软双拼 | `double_pinyin_mspy` | 26 |
| 搜狗双拼 | `double_pinyin_sogou` | 26 |
| 自然码 14 / 17 / 18 键 | `double_pinyin_14jian` 等 | 14 / 17 / 18 |
| 小鹤 14 / 17 / 18 键 | `double_pinyin_flypy_14jian` 等 | 14 / 17 / 18 |

**分号问题（微软 / 搜狗）**：这两种键位里 `ing` 落在分号 `;` 上。Xime 的 26 键布局没有分号键，
而且 `default.custom.yaml` 里 `;` 在候选状态下是「选第 2 个候选」的快捷键，所以这两个方案额外
派生了一条 `derive/;/n/`，键盘上打 `n` 也能输入 -ing 音节。代价是 `xn` 会先出「新」再出「行」；
桌面端打 `;` 仍然是标准的 `ing`。

**合并键双拼的代价**（`pinyin_simp.dict.yaml` 10.07 万条按词频分组估算，词频并列时实际顺序可能略有出入）：

| 方案 | 首选仍排第一 | 前 3 位内命中 |
| --- | --- | --- |
| 自然码 26 键 | 100% | 100% |
| 自然码 18 键 | 82.1% | 95.8% |
| 自然码 17 键 | 78.3% | 94.1% |
| 自然码 14 键 | 67.9% | 86.9% |
| 小鹤 26 键 | 100% | 100% |
| 小鹤 18 键 | 82.4% | 96.0% |
| 小鹤 17 键 | 79.7% | 95.1% |
| 小鹤 14 键 | 67.1% | 86.8% |

本机 `rime_deployer` + `rime_console` 实测：

```
自然码 26   nihk → 你好        ui → 是
小鹤   26   nihc → 你好
微软   26   nihk → 你好        x; → 行        xn → 新、行
自然码 18   bihj → 你好
自然码 17   bihj → 你好
自然码 14   bugj → 不好、你好、不敢     ← 14 键下「不好」与「你好」同码
小鹤   18   bihx → 你好
小鹤   17   bihx → 你好
小鹤   14   bugc → 不会、不好、你好
```

为什么要谨慎用 14 键：它把 `i` 并进 `u`、`d` 并进 `s`，双拼里 `bi`(比) 与 `bu`(不) 都成了 `bu`，
`di`(地) 与 `duo`(多) 都成了 `si`。想少键又少翻页，18 键是甜点（键少 8 个，首选只掉 18 个百分点）。

两点实现说明：

1. **合并键双拼没有 `preedit_format`**。输入框显示的是折叠后的按键码（与键面标的组合名一致），
   不再还原成全拼——字母已经不一一对应，强行还原只会显示出错的拼音。
2. **别把双拼挂进纠错 / 模糊拼音**。`xime.yaml` 的 `correction.schemas` 与 `fuzzy_pinyin.schemas`
   只认「按键字母 = 编码」的全拼方案，双拼的编码经过拼写运算变换，加进去只会污染编码
   （配置文件的注释里也写了这条）。

> **始于** 2019年02月28日
