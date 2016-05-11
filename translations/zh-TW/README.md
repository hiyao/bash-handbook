# bash-handbook [![CC 4.0][cc-image]][cc-url]

這份文件是為想要學習 Bash 但是又不需要專研太深的人。

> **Tip**：不妨嘗試 [**learnyoubash**](https://git.io/learnyoubash) — 一個基於本手冊的互動教學平台！

# Node 套件手稿

你可以使用 `npm` 來安裝這個手冊。只要執行：

```
$ npm install -g bash-handbook
```

你現在可以在命令執行 `bash-handbook`。它會開啟你所選的 `$PAGER`。當然，你可以繼續在這裡閱讀。

這是可用的資源：<https://github.com/denysdovhan/bash-handbook>

# 翻譯

目前翻譯這個 **bash-handbook** 的版本有：

- [Português (Brasil)](/translations/pt-BR/README.md)
- [简体中文 (中国)](/translations/zh-CN/README.md)
- [繁體中文（台灣）](/translations/zh-TW/README.md)

[**徵求其他語言版本翻譯**][tr-request]

[tr-request]: https://github.com/denysdovhan/bash-handbook/issues/new?title=Translation%20Request:%20%5BPlease%20enter%20language%20here%5D&body=I%20am%20able%20to%20translate%20this%20language%20%5Byes/no%5D

# 目錄

- [簡介](#introduction)
- [Shell 的模式](#shell-的模式)
  - [交互性模式](#交互性模式)
  - [非交互性模式](#非交互性模式)
  - [Exit codes](#exit-codes)
- [註解](#註解)
- [變數](#變數)
  - [局部變數](#局部變數)
  - [環境變數](#環境變數)
  - [位置參數](#位置參數)
- [Shell expansion](#shell-expansion)
  - [括號 expansion](#括號-expansion)
  - [命令替換](#命令替換)
  - [算術 expansion](#算術-expansion)
  - [單引號和雙引號](#單引號和雙引號)
- [陣列](#陣列)
  - [陣列宣告](#陣列宣告)
  - [陣列 expansion](#陣列-expansion)
  - [陣列 slice](#陣列-slice)
  - [在一個陣列中新增元素](#在一個陣列中新增元素)
  - [在一個陣列中刪除元素](#在一個陣列中刪除元素)
- [Streams、pipes 和命令序列](#streamspipes-和-lists)
  - [Streams](#streams)
  - [Pipes](#pipes)
  - [命令序列](#命令序列)
- [條件陳述](#條件陳述)
  - [Primary 和組合表達式](#primary-和組合表達式)
  - [使用一個 `if` 陳述](#使用一個-if-陳述)
  - [使用一個 `case` 陳述](#使用一個-case-陳述)
- [迴圈](#迴圈)
  - [`for` 迴圈](#for-迴圈)
  - [`while` 迴圈](#while-迴圈)
  - [`until` 迴圈](#until-迴圈)
  - [`select` 迴圈](#select-迴圈)
  - [迴圈控制](#迴圈控制)
- [函式](#函式)
- [Debug](#debug)
- [後記](#後記)
- [其他資源](#其他資源)
- [License](#license)

# 簡介

如果你是一個開發者，你了解時間的價值，優化你的工作流程是這份工作的最重要之一。

在通往高效和生產力的路上，我們經常不得不做出反覆的動作，像是︰

* 截圖並上傳到伺服器。
* 可能有許多形式的處理文件。
* 在不同檔案格式件進行轉換。
* 解析程式的輸出。

讓 **Bash** 來拯救我們吧！

Bash 是一個 Unix Shell，由 [Brian Fox][] 為 GNU 專案以自由軟體來取代 [Bourne shell](https://en.wikipedia.org/wiki/Bourne_shell) 所撰寫的。Unix Shell 發佈於 1989，而且作為 Linux 和 OSX 的預設 shell 已經有很長一段時間。

[Brian Fox]: https://en.wikipedia.org/wiki/Brian_Fox_(computer_programmer)
<!-- link this format, because some MD processors handle '()' in URLs poorly -->

那麼我們為什麼需要學習有著 30 年歷史的東西呢？答案很簡單：這個_東西_是當今最強大的而且是可攜的工具，為所有基於 Unix 系統撰寫高效的 script。這就是為什麼你需要學習 bash 的原因。

在這個手冊中，我會透過範例，去描述 bash 最重要的概念。我希望這篇概略性的文章對你會有幫助。

# Shell 的模式

使用者 bash shell 可以工作在兩種模式 - 交互性和非交互性。

## 交互性模式

如果你是在 Ubuntu 上工作，你有七個虛擬終端機可以讓你使用。
桌面環境有七個虛擬終端機，所以你可以使用 `Ctrl-Alt-F7` 讓你進入友善的 GUI 介面。

你可以使用 `Ctrl-Alt-F1` 來使用 shell。在那之後，熟悉的 GUI 介面將會消失，然後會顯示虛擬終端機。

如果你看到以下的東西，代表你處在交互性模式：

    user@host:~$

你可以輸入各種 Unix 的命令，像是 `ls`、`grep`、`cd`、`mkdir`、`rm`，然後你可以看到執行後的結果。

我們稱這個 shell 叫做交互性模式，因為它是直接與使用者作互動。

使用虛擬終端機實在不怎麼的方便，例如，假設你想要同時編輯文件和執行其他命令，在這樣的情況下，你最好使用虛擬終端模擬，像是：

- [GNOME Terminal](https://en.wikipedia.org/wiki/GNOME_Terminal)
- [Terminator](https://en.wikipedia.org/wiki/Terminator_(terminal_emulator))
- [iTerm2](https://en.wikipedia.org/wiki/ITerm2)
- [ConEmu](https://en.wikipedia.org/wiki/ConEmu)

## 非交互性模式

在非交互性模式，shell 從檔案或是 pipe 讀取到命令然後執行。當直譯器到達檔案的末端時，shell 程式終止 session 並返回到父程式。

使用以下的命令來執行，讓 shell 處於非交互性模式中：

    sh /path/to/script.sh
    bash /path/to/script.sh

在上面的例子中，`script.sh` 只是一個普通的檔案，而 shell 直譯器可以判斷檔案中組成的命令；`sh` 和 `bash` 是 shell 的直譯程式。你可以使用你喜歡的文字編輯器來建立 `script.sh`（例如：vim、nano、Sublime Text、Atom 等等）。

除此之外，你還可以透過 `chmod` 命令將檔案加入可執行的權限，來執行 script：


    chmod +x /path/to/script.sh

另外，在這個 script 第一行必須說明執行這個檔案應該使用哪個程式，像是：

```bash
#!/bin/bash
echo "Hello, world!"
```

或者，如果你偏好使用 `sh` 而不是 `bash`，將 `#!/bin/bash` 改成 `#!/bin/sh`。這個 `#!` 字元序列被稱為 [shebang](http://en.wikipedia.org/wiki/Shebang_%28Unix%29)。現在你可以執行這個 script 了：

    /path/to/script.sh

我們使用上面簡單方便的技巧使用 `echo` 將文字列印到終端螢幕。

另一個方式使用 sheban 來顯示：

```bash
#!/usr/bin/env bash
echo "Hello, world!"
```

使用 shebang 的好處是可以基於 `PATH` 環境變數來尋找程式（這個例子是 `bash`）。相較於上面的第一種方法，應該使用這個方法，因為本機的檔案系統的程式位置是不能假設的。這樣好處是，如果 `PATH` 變數在系統上可能被設定到其他不同版本的程式上。例如，安裝一個新版本的 `bash` 同時保留原來版本的 `bash`，然後新增新版本的 `PATH` 變數到本機。使用 `#!/bin/bash` 會導致使用到原來的 `bash`，而使用 `#!/usr/bin/env bash` 會使用到新的版本。


## Exit codes

每個命令都會回傳一個 **exit code**（**回傳狀態**或**結束狀態**）。成功的命令始終回傳 `0`（零碼），而失敗總是回傳非零值（錯誤代碼）。錯誤代碼必須介於 1 到 255 的正整數。

撰寫 script 時，我們可以使用另一個方便的 `exit` 命令。使用這個命令來終止目前的執行，並向 shell 提供一個 exit code。執行一個不帶有任何參數的 `exit` code，會終止執行目前的 script 並回傳在 `exit` 之前的最後一個命令的 exit code。

當程式終止時，shell 分配一個 **exit code** 給 `$?` 環境變數。`$?` 變數是我們測試一個 script 是否成功執行。

以同樣的方式我們可以使用 `exit` 來終止一個 script，我們可以使用 `return` 命令來離開一個 function _並_回傳一個 **exit code** 給 caller。

# 註解

Script 中可以包含_註解_。註解是一個特殊的宣告，會被 `shell` 直譯器忽略。它們以一個 `#` 符號為開頭，到最末行結束。

例如：

```bash
#!/bin/bash
# 這個 script 將會列印你的名字。
whoami
```

> **Tip**: 使用註解可以解釋你的 script 做了什麼，以及_為什麼_這麼做。

# 變數


與大部分的程式語言一樣，你可以在 bash 內建立變數。
Bash 中沒有資料的類型。變數可以只包含數字或是一個或多個字元所組成的字串。你可以建立三種類型的變數：局部變數、環境變數、以及作為_位置參數_的變數。

## 局部變數

**局部變數**是只存在於單一 script 的變數。它們是無法讓其他程式或 script 存取的。
一個變數可以使用 `=` 來宣告（根據規則，宣告**應該不能**有任何的空白字元在變數和 `=` 之間），然後可以透過 `$` 來取得這個變數。例如：

```bash
username="denysdovhan"  # 宣告變數
echo $username          # 顯示變數
unset username          # 刪除變數
```

我們也可以使用 `local` keyword 宣告一個變數到一個 function。當 function 退出時，這個變數就會消失。

```bash
local local_var="I'm a local value"
```

## 環境變數

**環境變數**執行在目前 shell session 可以讓任何程式或 script 存取的變數。建立它們與局部變數類似，但使用的是一個 `export` keyword。

```bash
export GLOBAL_VAR="I'm a global variable"
```

bash 中有_許多_全域變數。你會經常遇到這些變數，所以這些是實用的快速查閱資料表︰

| 變數         | 描述                                                          |
| :----------- | :------------------------------------------------------------ |
| `$HOME`      | 目前使用者的家目錄。                                          |
| `$PATH`      | shell 以這些冒號分隔的目錄清單尋找命令。                      |
| `$PWD`       | 目前的工作目錄。                                              |
| `$RANDOM`    | 0 到 32767 之間的亂數整數。                                   |
| `$UID`       | 數字類型，真實使用者的 ID。                                   |
| `$PS1`       | 主要的提示字串。                                              |
| `$PS2`       | 次要的提示字串。                                              |

根據這個[連結](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_02.html#sect_03_02_04)，可以查看在 Bash 內的環境變數擴充列表。

## 位置參數

**位置參數**是當 function 計算時，變數被分配到的位置。下面表格列出了當你在 function 內部時，位置參數變數和其他特殊變數它們所代表的意義。


| 參數           | 描述                                                         |
| :------------- | :----------------------------------------------------------- |
| `$0`           | Script 的名稱。                                              |
| `$1 … $9`      | 從第 1 個到第 9 個的參數列表元素。                           |
| `${10} … ${N}` | 從第 10 個到第 N 個的參數列表元素。                          |
| `$*` 或 `$@`   | 除了 `$0` 以外的所有位置參數。                               |
| `$#`           | 參數數目，不包括 `$0`。                                      |
| `$FUNCNAME`    | function 名稱（只有一個變數在 function 內部）。              |

在下面的範例，位置參數會是 `$0='./script.sh'`、`$1='foo'` 和 `$2='bar'`：

    ./script.sh foo bar

變數也可以有_預設值_。我們可以使用以下的語法來定義預設值：

```bash
 # 如果變數是空值，分配一個預設值。
: ${VAR:='default'}
: ${$1:='first'}
# 或
FOO=${FOO:-'default'}
```

# Shell expansion

_Expansion_ 被劃分為 _token_ 後，會在命令列上執行。換句話說，這些 expansions 是一種機制來計算算術運算，以儲存命令的執行結果等等。

如果你有興趣的話，你可以閱讀[更多關於 shell expansions](https://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions)。

## 括號 expansion

括號 expansion 讓我們可以產生任意字串。它類似於_檔案名稱 expansion_。例如：

```bash
echo beg{i,a,u}n # begin began begun
```

括號 expansions 也可以用在建立一個 loop iterate 的範圍。

```bash
echo {0..5} # 0 1 2 3 4 5
echo {00..8..2} # 00 02 04 06 08
```

## 命令替換

命令替換允許我們對一個命令求值，並將其值替換到另一個命令或變數賦值。當命令被封閉在 ``` `` ``` 或 `$()` 時，命令替換將會執行。例如，我們可以使用它如下所示：

```bash
now=`date +%T`
# 或
now=$(date +%T)

echo $now # 19:08:26
```

## 算術 expansion

在 bash 我們可以很自由的做任何算術運算。但是表達式必須封閉在 `$(( ))`，算術 expansions 的格式為：

```bash
result=$(( ((10 + 5*3) - 7) / 2 ))
echo $result # 9
```

在算術 expansions 內，一般使用變數不需要 `$` 前綴：

```bash
x=4
y=7
echo $(( x + y ))     # 11
echo $(( ++x + y++ )) # 12
echo $(( x + y ))     # 13
```

## 單引號和雙引號

單引號和雙引號有很重要的區別。在雙引號內的變數或是命令替換會被 expand。在單引號內則不會。例如：

```bash
echo "Your home: $HOME" # Your home: /Users/<username>
echo 'Your home: $HOME' # Your home: $HOME
```

局部變數和環境變數包含空格時，它們在引號內的 expand 要格外小心。隨便舉個例子，使用 `echo` 來列印一些使用者的輸入：

```bash
INPUT="A string  with   strange    whitespace."
echo $INPUT   # A string with strange whitespace.
echo "$INPUT" # A string  with   strange    whitespace.
```

第一個 `echo` 調用了 5 個分隔的參數 — $INPUT 被拆成獨立的單字，`echo` 在每個單字之間列印一個空白字元。在第二種情況中，`echo` 調用了一個參數（整個 $INPUT，包含空白）。

現在假設一個更嚴重的範例：

```bash
FILE="Favorite Things.txt"
cat $FILE   # 嘗試列印兩個檔案：`Favorite` 和 `Things.txt`。
cat "$FILE" # 列印一個檔案： `Favorite Things.txt`。
```

在這個範例的問題可以透過把 FILE 重新命名成 `Favorite-Things.txt` 來解決，考慮輸入進來的環境變數、位置參數，或是命令的輸出（`find`、`cat` 等等）。如果輸入*可能*包含空格，請記得使用括號括起來。

# 陣列

與大部分的程式語言一樣，在 bash 一個陣列是一個變數，讓你可以可以參考多個數值。在 Bash 中，陣列是從 0 開始，意思是陣列第一個元素的索引是 0 。

當處理陣列時，我們應該要知道一個特殊的環境變數 `IFS`。**IFS** 或 **Input Field Separator**，是陣列中的元素之間的分隔符號。預設值是一個空格 `IFS=' '`。

## 陣列宣告

在 Bash 你可以通過簡單地賦值給陣列變數的索引來建立陣列。

```bash
fruits[0]=Apple
fruits[1]=Pear
fruits[2]=Plum
```

也可以使用複合賦值建立陣列變數，像是：

```bash
fruits=(Apple Pear Plum)
```

## 陣列 expansion

獨立的陣列元素可以被 expand 成類似其他的變數：

```bash
echo ${fruits[1]} # Pear
```

整個陣列可以被 expand，透過在索引位置使用 `*` 或 `@` ：

```bash
echo ${fruits[*]} # Apple Pear Plum
echo ${fruits[@]} # Apple Pear Plum
```

以上兩行有重要的（和微妙的）區別，假設陣列元素包含空白：

```bash
fruits[0]=Apple
fruits[1]="Desert fig"
fruits[2]=Plum
```

我們想要在單獨一行列印陣列每個元素，所以我們嘗試使用 `printf` 建立：

```bash
printf "+ %s\n" ${fruits[*]}
# + Apple
# + Desert
# + fig
# + Plum
```

為什麼 `Desert` 和 `fig` 各佔了一行？讓我們嘗試使用引號：

```bash
printf "+ %s\n" "${fruits[*]}"
# + Apple Desert fig Plum
```

現在所有元素都在單獨一行了 - 這不是我們想要的！為了解決這個問題，接著由 `${fruits[@]}` 登場：

```bash
printf "+ %s\n" "${fruits[@]}"
# + Apple
# + Desert fig
# + Plum
```

在雙引號中，`${fruits[@]}` 將陣列每個元素 expand 成獨立的參數；保留了陣列元素中的空白。

## 陣列 slice

除此之外，我們可以使用 _slice_ 運算符來取出陣列中某個部份：

```bash
echo ${fruits[@]:0:2} # Apple Desert fig
```

在上面的範例，`${fruits[@]}` expands 整個陣列的內容，而 `:0:2` 取出從索引為 0，長度為 2 的元素。

## 在一個陣列中新增元素

在一個陣列中新增元素也是相單簡單的。在這種情況下複合賦值特別有用。我們可以像這樣使用︰

```bash
fruits=(Orange "${fruits[@]}" Banana Cherry)
echo ${fruits[@]} # Orange Apple Desert fig Plum Banana Cherry
```

上面的範例中，`${fruits[@]}` expands 整個陣列的內容，並替換 fruits 然後帶入複合賦值，分配新的值到 `fruits` 陣列，修改原始的值。

## 在一個陣列中刪除元素

如果要從一個陣列中刪除一個元素，使用 `unset` 命令：

```bash
unset fruits[0]
echo ${fruits[@]} # Apple Desert fig Plum Banana Cherry
```

# Streams、pipes 和 lists

Bash 擁有功能強大的工具，用於處理其他程式和它們的輸出。使用 streams 我們可以將一個程式的輸出傳送到另一個程式或檔案，從而撰寫日誌或任何我們想要的東西。

Pipes 給我們建立 conveyor 的機會和控制命令的執行。_（譯者註：converyor 為輸送的概念）_

最重要的是我們瞭解如何使用這個強大和複雜的工具。

## Streams

Bash 接收輸入並將輸出作為序列或 **streams** 的字元。這些 streams 可能會被重導到檔案或其他 streams。

這裡有三個描述：

| 代碼 | 描述符     | 描述                 |
| :--: | :--------: | :------------------- |
| `0`  | `stdin`    | 標準輸入             |
| `1`  | `stdout`   | 標準輸出             |
| `2`  | `stderr`   | 錯誤輸出             |

重導讓我們可以控制命令的輸入來自哪裡，和輸出到哪裡去。這些運算符在重導 streams 時會被用到：

| 運算符   | 描述                                         |
| :------: | :------------------------------------------- |
| `>`      | 重新導向輸出                                 |
| `&>`     | 重新導向輸出和錯誤輸出                       |
| `&>>`    | 以附加形式重新導向輸出和錯誤輸出             |
| `<`      | 重新導向輸入                                 |
| `<<`     | [Here 文件](http://tldp.org/LDP/abs/html/here-docs.html)語法 |
| `<<<`    | [Here 字串](http://www.tldp.org/LDP/abs/html/x17837.html) |

這裡有一些重新導向的範例：

```bash
# 將 ls 的輸出寫入到 list.txt。
ls -l > list.txt

# 將輸出附加到 list.txt。
ls -a >> list.txt

# 所有錯誤將會被寫入到 error.txt。
grep da * 2> errors.txt

# 從 errors.txt 讀取。
less < errors.txt
```

## Pipes

我們不只可以在檔案可以重導 streams，也可以在其他的程式中。**Pipes** 讓我們將輸出作為其他程式的輸入。

在下方的範例，`command1` 傳送輸出到 `command2`，然後輸出作為 `command3` 的輸入：

    command1 | command2 | command3

這樣的結構稱為 **pipelines**。

實際上，這可以用來在多個程式中依序處理資料。例如，`ls -l` 的輸出傳送到 `grep`，只列印出附檔名為 `.md` 的檔案，然後這個輸出最終傳到 `less`：

    ls -l | grep .md$ | less

pipeline 的退出狀態通常是 pipeline 中最後一個命令的退出狀態。shell 不會回傳狀態，直到所有在 pipeline 的命令都完成。如果任何在 pipeline 內的命令失敗，你應該設定 pipefail 選項：

    set -o pipefail

## 命令序列

**命令序列**是由 `;`、`&`、`&&` 或 `||` 運算符分隔一個或多個 pipeline 序列。

如果一個命令是透過控制運算符 `&` 終止，shell 會在子 shell 中非同步執行命令。換句話說，這個命令會在背景執行。

命令透過 `;` 隔開依序執行：一個接一個。shell 會等待每個命令完成。

```bash
# command2 會在 command1 執行完後被執行。
command1 ; command2

# 等同於這個方式。
command1
command2
```

`&&` 和 `||` 分別稱為 _AND_ 和 _OR_ 序列。

_AND 序列_ 看起來像是：

```bash
# 只有在 command1 結束成功時（回傳 exit status 為 0 ），command2 才會被執行。
command1 && command2
```

_OR 序列_ 的形式：

```bash
# 只有在 command1 未成功完成時（回傳錯誤代碼 ），command2 才會被執行。
command1 || command2
```

一個 _AND_ 或 _OR_ 序列回傳的狀態碼，是最後一個執行命令的狀態碼。

# 條件陳述

與大部分的程式語言一樣，Bash 中的條件陳述讓我們決定是否執行某些程式。結果取決於在 `[[ ]]` 內的計算運算式。

條件表達式可以包含 `&&` 和 `||` 運算符，就是 _AND_ 和 _OR_。除此之外，這裡還有許多[其他方便的表達式](#primary-and-combining-expressions)。

有兩種不同條件陳述：`if` 和 `case`。

## Primary 和組合表達式

封閉在 `[[ ]]`（或 `sh` 中的 `[ ]`）中的表達式稱為 **測試命令** 或 **primaries**。這些表達式幫助我們表明條件的結果。在下表中，我們使用 `[ ]`，因為它也可以在 `sh` 執行。這裡有關於在 bash 中，[雙引號和單引號的差別](http://serverfault.com/a/52050)的一些回答。

**處理檔案系統：**

| Primary       | 含意                                                         |
| :-----------: | :----------------------------------------------------------- |
| `[ -e FILE ]` | 如果 `FILE` 存在（**e**xists），為 Ture。                    |
| `[ -f FILE ]` | 如果 `FILE` 存在且為一個普通的文件（**f**ile），為 True。    |
| `[ -d FILE ]` | 如果 `FILE` 存在且為一個目錄（**d**irectory），為 True。     |
| `[ -s FILE ]` | 如果 `FILE` 存在且不為空（**s**ize 大於 0），為 True。       |
| `[ -r FILE ]` | 如果 `FILE` 存在且有可讀權限（**r**eadable），為　True。     |
| `[ -w FILE ]` | 如果 `FILE` 存在且有可寫入權限（**w**ritable），為 True。    |
| `[ -x FILE ]` | 如果 `FILE` 存在且有可執行權限（e**x**ecutable），為 True 。 |
| `[ -L FILE ]` | 如果 `FILE` 存在且為符號連結（**l**ink），為 True。          |
| `[ FILE1 -nt FILE2 ]` | FILE1 比 FILE2 新（**n**ewer **t**han）。            |
| `[ FILE1 -ot FILE2 ]` | FILE1 比 FILE2 舊（**o**lder **t**han）。            |

**處理字串：**

| Primary        | 含意                                                        |
| :------------: | :---------------------------------------------------------- |
| `[ -z STR ]`   | `STR` 為空（長度為 0，**z**ero）。                          |
| `[ -n STR ]`   |`STR` 不為空 (長度不為 0，**n**on-zero）。                   |
| `[ STR1 == STR2 ]` | `STR1` 和 `STR2` 相等。                                 |
| `[ STR1 != STR2 ]` | `STR1` 和 `STR2` 不相等。                               |

**二進位的算術運算符：**

| Primary             | 含意                                                     |
| :-----------------: | :------------------------------------------------------- |
| `[ ARG1 -eq ARG2 ]` | `ARG1` 和 `ARG2` 相等（**eq**ual）。                     |
| `[ ARG1 -ne ARG2 ]` | `ARG1` 和 `ARG2` 不相等（**n**ot **e**qual）。           |
| `[ ARG1 -lt ARG2 ]` | `ARG1` 小於 `ARG2`（**l**ess **t**han）。                |
| `[ ARG1 -le ARG2 ]` | `ARG1` 小於等於 `ARG2`（**l**ess than or **e**qual）。   |
| `[ ARG1 -gt ARG2 ]` | `ARG1` 大於 `ARG2`（**g**reater **t**han）。             |
| `[ ARG1 -ge ARG2 ]` | `ARG1` 大於等於 `ARG2`（**g**reater than or **e**qual）。|

條件可以和**表達式組合**使用：

| 運算符         | 效果                                                               |
| :------------: | :----------------------------------------------------------------- |
| `[ ! EXPR ]`   | 如果 `EXPR` 為 false，則為 True。                                  |
| `[ (EXPR) ]`   | 回傳 `EXPR` 的值。                                                 |
| `[ EXPR1 -a EXPR2 ]` | _AND_ 邏輯。如果 `EXPR1` **a**nd `EXPR2` 為 Ture，則為 True。|
| `[ EXPR1 -o EXPR2 ]` | _OR_ 邏輯。如果 `EXPR1` **o**r `EXPR2` 為 Ture，則為 True。  |

還有許多有用的 primariy 你可以在 [Bash man 網頁](http://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)輕鬆的找到它們。

## 使用一個 `if` 陳述

`if` 使用方式就像其他程式語言一樣。如果表達式括號內為 true，在 `then` 和 `fi` 之間的程式碼會被執行。`fi` 說明根據條件所執行的程式碼已經結束。

```bash
# 單行
if [[ 1 -eq 1 ]]; then echo "true"; fi

# 多行
if [[ 1 -eq 1 ]]; then
  echo "true"
fi
```

同樣的，我們可以使用 `if..else`，像是：

```bash
# 單行
if [[ 2 -ne 1 ]]; then echo "true"; else echo "false"; fi

# 多行
if [[ 2 -ne 1 ]]; then
  echo "true"
else
  echo "false"
fi
```

有時候 `if..else` 不能滿足我們的需要。在這個情況下，別忘了 `if..elif..else`，使用起來也是很方便。

看看下面的例子︰

```bash
if [[ `uname` == "Adam" ]]; then
  echo "Do not eat an apple!"
elif [[ `uname` == "Eva" ]]; then
  echo "Do not take an apple!"
else
  echo "Apples are delicious!"
fi
```

## 使用一個 `case` 陳述
如果你面對的是好幾個不同情況，需要採取不同的行動，那麼使用 `case` 陳述或許會比巢狀化的 `if` 陳述來的有用。下方是使用 `case` 來處理更多複雜的條件：

```bash
case "$extension" in
  "jpg"|"jpeg")
    echo "It's image with jpeg extension."
  ;;
  "png")
    echo "It's image with png extension."
  ;;
  "gif")
    echo "Oh, it's a giphy!"
  ;;
  *)
    echo "Woops! It's not image!"
  ;;
esac
```

每個情況都會對應到一個模式。`|` 符號是被用來分離多種模式，而 `)` 運算符是終止這個模式。第一個符合的命令會被執行。`*` 代表不對應以上所給定的模式。每個命令區塊之間需要透過 `;;` 隔開。

# 迴圈

這裡我們不會感到驚訝。與任何程式語言一樣，在 bash 如果控制條件為 true，就會迭代迴圈。

在 Bash 有四種類型的迴圈：`for`、`while`、`until` 和 `select`。

## `for` 迴圈

`for` 非常相似於 C 的 `for` 迴圈。它看起來像是：

```bash
for arg in elem1 elem2 ... elemN
do
  # statements
done
```

在每個迴圈中，`arg` 會從 `elem1` 到 `elemN` 依序被賦值。值可能是萬用字元或[括號 expansion](#括號-expansion)。

當然，我們也可以將 `for` 迴圈寫成一行，但在這個情況需要一個分號在 `do` 之前，像是以下：

```bash
for i in {1..5}; do echo $i; done
```

還有，如果你覺得 `for..in..do` 看起來很奇怪，你也可以撰寫像是 C 語言風格的 `for` 迴圈，像是：

```bash
for (( i = 0; i < 10; i++ )); do
  echo $i
done
```

當我們想要將目錄下每個文件做相同的操作時，`for` 是相當方便的。例如，如果我們需要移動所有的 `.bash` 檔案到 `script` 資料夾，並給予它們可執行的權限，我們的 script 可以像是這樣：

```bash
#!/bin/bash

for FILE in $HOME/*.bash; do
  mv "$FILE" "${HOME}/scripts"
  chmod +x "${HOME}/scripts/${FILE}"
done
```

## `while` 迴圈

只要該條件為 _true_，`while` 迴圈測試一個條件，並遍歷序列的命令。被檢測的條件跟 `if.. then` 中使用的 [primary](#primary-and-combining-expressions) 並沒有差別。所以 `while` 迴圈看起來會像是：

```bash
while [[ condition ]]
do
  # statements
done
```

就像 `for` 迴圈一樣，如果我們想要將 `do` 和條件寫成一行的話，我們必須在 `do` 前面加上分號。

一個處理範例如下所示：

```bash
#!/bin/bash

# 0 到 9 每個數字的平方。
x=0
while [[ $x -lt 10 ]]; do # x 小於 10。
  echo $(( x * x ))
  x=$(( x + 1 )) # x 加 1。
done
```

## `until` 迴圈

`until` 迴圈和 `while` 迴圈完全相反的。它會像 `while` 一樣檢查條件，只要狀態是 _false_ 迴圈就會繼續保持：

```bash
until [[ condition ]]; do
  #statements
done
```

## `select` 迴圈

`select` 迴圈幫助我們組織一個使用者功能表。它和 `for` 迴圈語法幾乎相同：

```bash
select answer in elem1 elem2 ... elemN
do
  # statements
done
```

`select` 會在螢幕上列印出 `elem1..elemN` 的序號，之後它會提醒使用者。通常看到的是 `$?`（`PS3` 變數）。答案將會儲存在 `answer`。如果 `answer` 是介於 `1..N` 的數字，`statements` 和 `select` 將會執行到下一個迭代— 所以我們應該使用 `break` 避免無窮迴圈。

一個處理範例如下所示：

```bash
#!/bin/bash

PS3="Choose the package manager: "
select ITEM in bower npm gem pip
do
  echo -n "Enter the package name: " && read PACKAGE
  case $ITEM in
    bower) bower install $PACKAGE ;;
    npm)   npm   install $PACKAGE ;;
    gem)   gem   install $PACKAGE ;;
    pip)   pip   install $PACKAGE ;;
  esac
  break # 避免無窮迴圈。
done
```

在這個範例中，詢問使用者想要使用哪些套件管理，然後詢問哪些我們需要套件，最後執行並安裝：

如果我們執行，我們可以得到：

```
$ ./my_script
1) bower
2) npm
3) gem
4) pip
Choose the package manager: 2
Enter the package name: bash-handbook
<installing bash-handbook>
```

## 迴圈控制

在某些情況下，我們需要在正常結束前或跳過某次迭代來結束迴圈。在這個情況下，我們可以使用 shell 內建的 `break` 和 `continue` 語句。這兩種方式都可以在迴圈中執行。

`break` 語句被用於在迴圈結束之前，退出目前迴圈。我們之前已經看過了。

`continue` 語句跳過一個迭代。我們可以像這樣使用它：

```bash
for (( i = 0; i < 10; i++ )); do
  if [[ $(( i % 2 )) -eq 0 ]]; then continue; fi
  echo $i
done
```

如果我們執行上方的範例，我們可以列印出 0 到 9 間的奇數。

# 函式

在 script 中，我們有能力定義和呼叫 function。就像任何其他程式語言一樣，在 Bash 中，function 是一個程式碼區塊，但是有些不同。

在 Bash 中，function 是一個被組織在單一名稱的命令序列，這就是 function 的 _名稱_。呼叫一個 function 就像在其他程式語言一樣，你只要撰寫 function 的名稱，function 就會被 _調用_。

我們可以用這種方式宣告我們的 function：

```bash
my_func () {
  # statements
}

my_func # 呼叫 my_func。
```

我們在調用 function 前，必須要先宣告。

Function 可以接受參數並回傳一個結果 — exit code。在 function 內，與[非交互模式](#非交互模式)下的 script 參數處理方式相同 — 使用[位置參數](#位置參數)。結果代碼可以使用 `return` 命令來 _回傳_。

以下的 function 需要一個名稱並回傳 `0`，表示成功執行。

```bash
# 帶參數的 function。
greeting () {
  if [[ -n $1 ]]; then
    echo "Hello, $1!"
  else
    echo "Hello, unknown!"
  fi
  return 0
}

greeting Denys  # Hello, Denys!
greeting        # Hello, unknown!
```

我們已經討論過 [exit codes](#退出代碼)。`return` 命令不帶任何最後一個執行命令 exit code 的回傳參數。上面的範例，`return 0` 會回傳一個成功的 exit code `0`。

## Debug

shell 提供了 debug script 的工具。如果我們想要在 debug 模式執行 script，我們在 script 的 shebang 使用一個特別的選項：

```bash
#!/bin/bash options
```

這個選項是改變 shell 行為的設定。下表是一些可能對你有幫助的選項清單：

| 簡寫  | 名稱        | 描述                                                   |
| :---: | :---------- | :----------------------------------------------------- |
| `-f`  | noglob      | 禁止檔案名 expansion（globbing）。　　　　　                 |
| `-i`  | interactive | Script 執行在_交互_模式。                              |
| `-n`  | noexec      | 讀取命令，但不執行它們（語法確認）。                   |
|       | pipefail    | 如果任何命令失敗使 pipeline 失敗，不只是最後一個命令失敗。 |
| `-t`  | —           | 在第一個命令完後退出。                                 |
| `-v`  | verbose     | 在執行前，列印每個命令到 `stderr`。                    |
| `-x`  | xtrace      | 在執行前，列印每個命令和 expansion 參數到 `stderr`。          |

例如，我們有個 script 有 `-x` 選項像是：

```bash
#!/bin/bash -x

for (( i = 0; i < 3; i++ )); do
  echo $i
done
```

這將會列印變數的值到 `stdout` 以及其他有用的資訊：

```
$ ./my_script
+ (( i = 0 ))
+ (( i < 3 ))
+ echo 0
0
+ (( i++  ))
+ (( i < 3 ))
+ echo 1
1
+ (( i++  ))
+ (( i < 3 ))
+ echo 2
2
+ (( i++  ))
+ (( i < 3 ))
```

有時候我們需要 debug script 某個部份。在這種情況下使用 `set` 命是很方便的。這命令有啟用和禁用選項。使用 `-` 啟用選項，`+` 禁用選項。

```bash
#!/bin/bash

echo "xtrace is turned off"
set -x
echo "xtrace is enabled"
set +x
echo "xtrace is turned off again"
```

# 後記

我希望這麼小手冊會是有趣並有幫助的。老實說，我撰寫這個手冊是為了幫助自己不忘記這些基本的 bash。我嘗試透過簡單但有意義的方式，希望你們會喜歡。

這本手冊講述我自己與 Bash 的經驗。它並不全面，所以如果你還需要更多資訊的話，請執行 `man bash` 並從那裡開始。

非常歡迎你的貢獻，任何指正或問題我都非常感激。這些都可以透過建立一個新 [issue](https://github.com/denysdovhan/bash-handbook/issues) 來進行。

感謝你閱讀這本手冊！

# 想要了解更多嗎？

這裡有一些涵蓋 Bash 的相關資料：

* Bash man 頁面。在許多環境中，你可以執行 Bash 中的 `man bash`，藉由系統並顯示 Bash 相關的幫助資訊。有關更多 `man` 命令的相關資訊，請到託管在 [The Linux Information Project](http://www.linfo.org/) 的 ["The man Command"](http://www.linfo.org/man.html) 網頁查看。
* ["Bourne-Again SHell manual"](https://www.gnu.org/software/bash/manual/) 有許多格式，包含 HTML、Info、TeX、PDF、和 Texinfo。託管在 <https://www.gnu.org/>。在 2016/01，包括 4.3 版本，最後更新時間 2015/02/02。

# 其他資源

* [awesome-bash](https://github.com/awesome-lists/awesome-bash) 是一個 Bash script 和其他資源的蒐集。
* [awesome-shell](https://github.com/alebcay/awesome-shell) 是另一個 Bash script 和其他資源的蒐集。
* [bash-it](https://github.com/Bash-it/bash-it) 為你日常工作使用、 開發和維護 shell script 和自訂命令提供了可靠的框架。
* [dotfiles.github.io](http://dotfiles.github.io/) 是一個很棒的資源點，各種 dotfile 蒐集和可用於 bash 或其他 shell 的 shell 框架。
* [learnyoubash](https://github.com/denysdovhan/learnyoubash) 幫助你撰寫你的第一個 bash script。
* [shellcheck](https://github.com/koalaman/shellcheck) 是一個 shell script 的靜態分析工具。你可以從在 [www.shellcheck.net](http://www.shellcheck.net/) 網頁上或從命令來執行它。安裝說明在 [koalaman/shellcheck](https://github.com/koalaman/shellcheck) github repository 頁面。

最後，Stack Overflow 有許多[標記為 bash](https://stackoverflow.com/questions/tagged/bash) 的問題，當你在卡住時，你可以從那些地方學習或是提問。

# License

[![CC 4.0][cc-image]][cc-url] © [Denys Dovhan](http://denysdovhan.com)

[cc-url]: http://creativecommons.org/licenses/by/4.0/
[cc-image]: https://i.creativecommons.org/l/by/4.0/80x15.png
