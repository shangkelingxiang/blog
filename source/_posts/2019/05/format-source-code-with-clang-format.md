---
layout: post
title: 使用 clang-format 對程式碼進行排版
date: 2019-05-27 10:14:31
updated: 2019-05-27 10:14:31
categories:
tags: [C/C++, clang-format]
---

# 介紹

[`clang-format`](https://clang.llvm.org/docs/ClangFormat.html) 是基於 `clang` 的程式碼格式化（format，與硬碟的格式化不同，指針對程式碼風格進行排版）工具，支持許多種 Coding Style 如：

<!--more-->

- [Google Style Guides](http://google.github.io/styleguide/)
- [LLVM Coding Standards](https://llvm.org/docs/CodingStandards.html)
- [Webkit Code Style Guidelines](https://webkit.org/code-style-guidelines/)
- [Mozilla Coding style](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Coding_Style)
- ...

# 安裝

## Microsoft Windows 10

在 Windows 作業系統下，必須先安裝好 Python 環境並將其添加至環境變數中，接著在 [LLVM Snapshot Builds](http://llvm.org/builds/) 頁面選擇 Windows 版本進行安裝，注意在安裝過程中要勾選 `Add LLVM to the system PATH for all users` 或 `Add LLVM to the system PATH for current user` 其中一項。安裝完後可以在終端機中檢查當前版本：

```bash
$ clang-format -version
clang-format version 9.0.0 (trunk)
```

## macOS

在 macOS 作業系統下，使用套件管理工具 [Homebrew](https://brew.sh/) 進行安裝：

```bash
$ brew install clang-format
```

# 使用

## 主要用法

```bash
# 以 LLVM 風格格式化 main.cpp，並將結果輸出到 stdout
$ clang-format -style=LLVM main.cpp

# 以 LLVM 風格格式化 main.cpp，直接將結果對 main.cpp 進行覆寫
$ clang-format -style=LLVM -i main.cpp

# 以 LLVM 風格格式化當前目錄下所有 *.cpp *.h，直接將結果進行覆寫（won't recurse into subdirectories）
$ clang-format -style=LLVM -i *.cpp *.h

# 對指定行數內容進行格式化，格式化 main.cpp 的第 1-5 行
$ clang-format -style=LLVM -lines=1:5 main.cpp
```

## 配合 `git` 使用

```bash
# 對 git 設定全局風格檔案路徑與 clang-format 版本
$ git config --global clangFormat.binary clang-format-9.0
$ git config --global clangFormat.style file
$ git config --global --path clangFormat.stylePath /PATH/.clang-format

# 格式化所有已 stage 的文件
$ git clang-format -style=LLVM

# 格式化整個 repo 並創建一個提交
$ git clang-format -style=LLVM --commit `git hash-object -t tree /dev/null`
```

## 常用命令

```bash
# 配合 find 與正則表達式，遞迴地對資料夾與子資料夾中文件進行格式化
$ find . -regex '.*\.\(cpp\|hpp\|cc\|cxx\)' -exec clang-format -style=LLVM -i {} \;
```

# 格式配置：`.clang-format` 的使用

通常根據不同專案，會在某些細節部分的排版進行微調。我們可以透過在專案的根目錄下加入 `.clang-format` 文件，來自定義該專案的格式配置，可以透過以下命令生成一個基於指定樣式的 `.clang-format` 文件：

```bash
$ clang-format -style=llvm -dump-config > .clang-format
```

以下是一份加上了註解的 `.clang-format`，或者是可以使用 [clang-format Configurator](https://zed0.co.uk/clang-format-configurator/) 進行具體修改：

```
# 語言: None, Cpp, Java, JavaScript, ObjC, Proto, TableGen, TextProto
Language: Cpp
BasedOnStyle: LLVM

# 訪問說明符（public、private…）的偏移
AccessModifierOffset: -4

# 開括號後的對齊: Align, DontAlign, AlwaysBreak（總是在開括號後換行）
AlignAfterOpenBracket: Align

# 連續賦值時，對齊所有等號
AlignConsecutiveAssignments: true

# 連續宣告時，對齊所有宣告的變數名稱
AlignConsecutiveDeclarations: true

# 左對齊跳脫換行（使用反斜杠換行）的反斜杠
AlignEscapedNewlinesLeft: true

# 水平對齊二元和三元表達式的操作數
AlignOperands: true

# 對齊連續的尾隨的註釋
AlignTrailingComments: true

# 允許函數聲明的所有參數在放在下一行
AllowAllParametersOfDeclarationOnNextLine: true

# 允許短的塊放在同一行
AllowShortBlocksOnASingleLine:	false

# 允許短的 case 標簽放在同一行
AllowShortCaseLabelsOnASingleLine:	false

# 允許短的函數放在同一行: None, InlineOnly, Empty, Inline, All
AllowShortFunctionsOnASingleLine: Empty

# 允許短的 if 語句保持在同一行
AllowShortIfStatementsOnASingleLine:	false

# 允許短的循環保持在同一行
AllowShortLoopsOnASingleLine:	false

# 總是在定義返回類型後換行（deprecated）
AlwaysBreakAfterDefinitionReturnType:	None

# 總是在返回類型後換行: None, All, TopLevel(頂級函數，不包括在類中的函數), 
#   AllDefinitions(所有的定義，不包括聲明), TopLevelDefinitions(所有的頂級函數的定義)
AlwaysBreakAfterReturnType:	None

# 總是在多行 string 字面量前換行
AlwaysBreakBeforeMultilineStrings:	false

# 總是在 template 宣告後換行
AlwaysBreakTemplateDeclarations: true

# false 表示函數實參要麽都在同一行，要麽都各自一行
BinPackArguments:	true

# false 表示所有形參要麽都在同一行，要麽都各自一行
BinPackParameters:	true

# 大括號換行，只有當 BreakBeforeBraces 設置為 Custom 時才有效
BraceWrapping: 
  # class定義後面
  AfterClass:	false
  # 控制語句後面
  AfterControlStatement:	false
  # enum定義後面
  AfterEnum:	false
  # 函數定義後面
  AfterFunction:	false
  # 命名空間定義後面
  AfterNamespace:	false
  # ObjC定義後面
  AfterObjCDeclaration:	false
  # struct定義後面
  AfterStruct:	false
  # union定義後面
  AfterUnion:	false
  # catch之前
  BeforeCatch:	true
  # else之前
  BeforeElse:	true
  # 縮進大括號
  IndentBraces:	false

# 在二元運算符前換行: None, NonAssignment(在非賦值的操作符前換行), All(在操作符前換行)
BreakBeforeBinaryOperators:	NonAssignment
# 在大括號前換行: Attach(始終將大括號附加到周圍的上下文), Linux(除函數、命名空間和類定義，與Attach類似), 
#   Mozilla(除枚舉、函數、記錄定義，與Attach類似), Stroustrup(除函數定義、catch、else，與Attach類似), 
#   Allman(總是在大括號前換行), GNU(總是在大括號前換行，並對於控制語句的大括號增加額外的縮進), WebKit(在函數前換行), Custom
#   註：這裏認為語句塊也屬於函數
BreakBeforeBraces:	Custom
# 在三元運算符前換行
BreakBeforeTernaryOperators:	true
# 在構造函數的初始化列表的逗號前換行
BreakConstructorInitializersBeforeComma:	false
# 每行字符的限制，0表示沒有限制
ColumnLimit:	200
# 描述具有特殊意義的註釋的正則表達式，它不應該被分割為多行或以其它方式改變
CommentPragmas:	'^ IWYU pragma:'
# 構造函數的初始化列表要麽都在同一行，要麽都各自一行
ConstructorInitializerAllOnOneLineOrOnePerLine:	false
# 構造函數的初始化列表的縮進寬度
ConstructorInitializerIndentWidth:	4
# 延續的行的縮進寬度
ContinuationIndentWidth:	4
# 去除C++11的列表初始化的大括號{後和}前的空格
Cpp11BracedListStyle:	false
# 繼承最常用的指針和引用的對齊方式
DerivePointerAlignment:	false
# 關閉格式化
DisableFormat:	false
# 自動檢測函數的調用和定義是否被格式為每行一個參數(Experimental)
ExperimentalAutoDetectBinPacking:	false
# 需要被解讀為foreach循環而不是函數調用的宏
ForEachMacros:	[ foreach, Q_FOREACH, BOOST_FOREACH ]
# 對#include進行排序，匹配了某正則表達式的#include擁有對應的優先級，匹配不到的則默認優先級為INT_MAX(優先級越小排序越靠前)，
#   可以定義負數優先級從而保證某些#include永遠在最前面
IncludeCategories: 
  - Regex:	'^"(llvm|llvm-c|clang|clang-c)/'
    Priority:	2
  - Regex:	'^(<|"(gtest|isl|json)/)'
    Priority:	3
  - Regex:	'.*'
    Priority:	1
# 縮進case標簽
IndentCaseLabels:	false
# 縮進寬度
IndentWidth:	4
# 函數返回類型換行時，縮進函數聲明或函數定義的函數名
IndentWrappedFunctionNames:	false
# 保留在塊開始處的空行
KeepEmptyLinesAtTheStartOfBlocks:	true
# 開始一個塊的宏的正則表達式
MacroBlockBegin:	''
# 結束一個塊的宏的正則表達式
MacroBlockEnd:	''
# 連續空行的最大數量
MaxEmptyLinesToKeep:	1
# 命名空間的縮進: None, Inner(縮進嵌套的命名空間中的內容), All
NamespaceIndentation:	Inner
# 使用ObjC塊時縮進寬度
ObjCBlockIndentWidth:	4
# 在ObjC的@property後添加一個空格
ObjCSpaceAfterProperty:	false
# 在ObjC的protocol列表前添加一個空格
ObjCSpaceBeforeProtocolList:	true
# 在call(後對函數調用換行的penalty
PenaltyBreakBeforeFirstCallParameter:	19
# 在一個註釋中引入換行的penalty
PenaltyBreakComment:	300
# 第一次在<<前換行的penalty
PenaltyBreakFirstLessLess:	120
# 在一個字符串字面量中引入換行的penalty
PenaltyBreakString:	1000
# 對於每個在行字符數限制之外的字符的penalty
PenaltyExcessCharacter:	1000000
# 將函數的返回類型放到它自己的行的penalty
PenaltyReturnTypeOnItsOwnLine:	60
# 指針和引用的對齊: Left, Right, Middle
PointerAlignment:	Left
# 允許重新排版註釋
ReflowComments:	true
# 允許排序#include
SortIncludes:	true
# 在C風格類型轉換後添加空格
SpaceAfterCStyleCast:	false
# 在賦值運算符之前添加空格
SpaceBeforeAssignmentOperators:	true
# 開圓括號之前添加一個空格: Never, ControlStatements, Always
SpaceBeforeParens:	ControlStatements
# 在空的圓括號中添加空格
SpaceInEmptyParentheses:	false
# 在尾隨的評論前添加的空格數(只適用於//)
SpacesBeforeTrailingComments:	2
# 在尖括號的<後和>前添加空格
SpacesInAngles:	true
# 在容器(ObjC和JavaScript的數組和字典等)字面量中添加空格
SpacesInContainerLiterals:	true
# 在C風格類型轉換的括號中添加空格
SpacesInCStyleCastParentheses:	true
# 在圓括號的(後和)前添加空格
SpacesInParentheses:	true
# 在方括號的[後和]前添加空格，lamda表達式和未指明大小的數組的聲明不受影響
SpacesInSquareBrackets:	true

# 標準: Cpp03, Cpp11, Auto
Standard: Cpp11

# tab 寬度
TabWidth: 4

# 使用 tab 字元: Never, ForIndentation, ForContinuationAndIndentation, Always
UseTab:	Never
```

# 與編輯器和 IDE 集成使用

除此之外還可以將 `clang-format` 與常用的編輯器進行集成：

- [Atom](https://atom.io/packages/clang-format)
- [CLion](http://clang.llvm.org/docs/ClangFormat.html#clion-integration)
- [Emacs](http://clang.llvm.org/docs/ClangFormat.html#emacs-integration)
- [Vim](http://clang.llvm.org/docs/ClangFormat.html#vim-integration)
- [Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
- [Visual Studio](https://docs.microsoft.com/en/visualstudio/ide/reference/options-text-editor-c-cpp-formatting?view=vs-2019)

# 參考資料

- [GitHub | eklitzke/clang-format-all](https://github.com/eklitzke/clang-format-all)
- [子豐的博客 | Clang-Format 格式化選項介绍](https://blog.csdn.net/softimite_zifeng/article/details/78357898)