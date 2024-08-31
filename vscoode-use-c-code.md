當要執行任何程式語言時都需要有 **編輯器** + **編譯器**，其中編輯器負責 coding 的介面，讓使用者可以方便而美觀的修改程式；編譯器負責編譯程式碼，把程式碼轉譯成機器碼後執行，兩者都準備好了才能夠好好的執行程式。

以下是透過 Visual Studio Code( VS Code )作為編輯器，MinGW 作為編譯器，執行 C/C++ 的方法。


## 安裝 VS Code 編輯器

首先到 [VS Code 官網](https://code.visualstudio.com/)上安裝應用程式，基本上都是按下一步繼續安裝就可以了。

安裝完成之後需要再安裝執行 C/C++ 所需的延伸模組，在左側延伸模組搜尋`Code Runner` & `C/C++`並安裝。也推薦安 `Chinese (Traditional) Language Pack for Visual Studio Code` 繁中模組在後續的過程更方便的操作。

再來按下 `Ctrl + , ` 搜尋 code runner terminal 把 Whether yo run code in integrated Terminal. 的選項打勾，這樣程式就可以透過按下右上角的三角形符號在終端機執行了，省去都要輸入 `g++ [檔案名稱] -o [編譯後檔案名稱]  e.g. g++ test.cpp -o test` 這樣指令編譯程式的麻煩。

## 安裝 MinGW 編譯器

### 第一步. 安裝 MinGW
先一樣到 [MinGW 官網](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/)找到「MinGW-W64 Online Installer」連線下載，或是下方的「MinGW-W64 GCC-8.1.0」選擇適合自己電腦的版本離線下載，兩者的差異只是下載方式不同而已，內容都是一樣的。

安裝過程要注意別無腦連續下一步，這裡有小東西要改 XD。
如果你想要嘗試較新的選擇也可以看看這裡的額外說明。[^註腳1]
![mingw-w64-setting](https://hackmd.io/_uploads/ByT1iHg3R.png =500x)


設定說明 :
- **Version**：gcc 的版本，選擇默認選項。
- **Architecture**：選擇 CPU 的位元數，32 位元選擇 i686、64 位元選擇 x86_64。
- **Threads**：作業系統的 API 選擇，Windows 選 win32；有需要與其他作業系統合作選 posix。
- **Exception**：異常處理機制，選擇 seh，它速度、效能最好，是 Windows 系統原生支援的方式，但只能用於 64 位元。
- **Build revision**：構建修訂版本號，可以維持默認就好。

如果你安裝的過程有出現`The file has been downloaded incorrectly`問題可以參考這裡的說明[^註腳2]

### 第二步. 設定環境變數

安裝完成之後 MinGW 就會出現在你的電腦 C 槽中，找到 MinGW 裡面的 bin 資料夾路徑，裡面放的就是編譯 C/C++ 的編譯工具，把這複製下來，等下會用到。

到「控制台」→「系統」→「進階系統設定」→「進階」→「環境變數」→「系統變數」→「點擊 PATH」→「編輯」→「新增」，把剛剛的資料夾路徑加到這裡面就可以了。
P.S. 我多放的路徑是放個心安的 XD
![mingw-w64-path-setting](https://hackmd.io/_uploads/rJUZjrln0.png)


### 第三步. MinGW 測試

在 window 搜尋列裡輸入`cmd`找到命令提示字元，並輸入`g++ -v`還有`gcc -v`，如果都是出現了一大串文字的話那就表示 MinGW 安裝成功了。

而如果出現`window下g++ 不是内部或外部命令`或其他的錯誤提示那就表示安裝過程有問題，需要回到前面的步驟重新檢查看看。

## 實際測試

回到 VS Code 按下`Ctrl + Shift + P`會出現選單，選擇「C/C++: Edit Configurations (UI)」進入設置畫面，在這裡要做兩個設定 :
1. 找到「Compiler path」，輸入 g++.exe [^註腳3] }}的路徑位置，舉例來說我的是`C:\mingw64\bin\g++.exe`。
2. 找到「IntelliSense mode」，選擇 `windows-gcc-x64`。

最後提供一段簡單的 C 語言程式碼讓你測試是否可以成功的在 VS Code 上執行 C 語言程式檔。

```C
#include <stdio.h>

int main() {
    printf("Hello World!");
    return 0;
}
```

## 參考

- [用 VSCode 寫 C/C++ 教學](https://hackmd.io/@liaojason2/vscodecppwindows)
- [[C++] MinGW-w64 安裝與設定](https://alexspot.tech/jottings-windows-vscode-with-mingw-w64/)
- [Meaning of options in mingw-w64 installer](https://stackoverflow.com/questions/29947302/meaning-of-options-in-mingw-w64-installer)
- [What is difference between sjlj vs dwarf vs seh?](https://stackoverflow.com/questions/15670169/what-is-difference-between-sjlj-vs-dwarf-vs-seh/15685229#15685229)



[^註腳1]: 我在寫文的時候有看到別人[建議使用另一種方法](https://stackoverflow.com/questions/29947302/meaning-of-options-in-mingw-w64-installer)，但是我不是這麼做的，所以如果你有實驗精神的話或許可以試試看，失敗的話就回頭重新安裝一次就好XD
他的建議 : `posix - dwarf - 2`，因為 **posix** 啟用 C++11 的`<thread>,<mutex>和<future>`；**dwarf** 更快 ；**2** 是最新版的。
[^註腳2]: 這個問題我個人在安裝的時候有出現，但是其他朋友測試的時候並沒有，所以應該是少數問題，就另外在註腳寫。
我的解法參考自[wingw-w64安装时 the file has been downloaded incorrectly!](https://blog.csdn.net/kramer_1711/article/details/119416512)，估計這是網路連線產生的問題，只要改成離線下載就可以了。
[^註腳3]: g++ 和 gcc 都是GNU Compiler Collection 的 C++ 編譯器，但兩者有用途上的些微差別。
g++ 支援 C++ 編譯，同時也兼容 C 程式碼的編譯，適合處理同時包含 C 和 C++ 程式碼的專案。
gcc 主要用於編譯 C 語言程式碼，並在擴展語法上支援某些 C++ 功能，但不完全支援 C++ 。