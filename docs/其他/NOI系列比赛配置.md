# NOI 系列比赛配置

## 能否使用某写法

1. `bits/stdc++.h`：可以使用，但注意标识符冲突问题，如 `y1` `next` 等。
2. `__int128`：不建议使用，不能使用 `cin` `scanf` 读入，不能使用 `cout` `printf` 输出。
3. `#define int long long`：不建议使用，对关键词进行宏定义是未定义的操作。
4. `ios::sync_with_stdio(false); cin.tie(0);`：可以使用，但需要：仅使用 C++ 风格 IO；且在最后刷新缓冲区。
5. `fclose(file.in);`：不建议使用，若要使用一定要提前刷新缓冲区。
6. `__gcd()`：可以使用；`gcd()`：C++ 14 不可以使用。
7. `#pragma GCC optimize(2)`：CCF 明确规定不可以使用。
8. `exit(0)`：可以使用，与 `return 0;` 效果一致，且可以在非主函数中退出运行，并且符合 CCF 规定“主函数返回值为 $0$”。
9. `register` `inline`：不建议使用，在 C++ 14 中没有实际效果。`static` 可以使用。
10. C++ 14 及之前的新特性：可以使用，评测系统为 C++ 14，见下文章节 [#编译选项](#编译选项)。

## 编译选项

CCF 在 NOIP 评测机的编译选项：`g++ a.cpp -o a -lm`  
建议的编译选项：`-std=c++14 -Wall -Wextra -Wl,--stack=268435456 -O`

1. `-std=c++14`，使用 C++ 14 新特性（NOI 系列评测系统为 C++ 14）。  
   如果编译器版本过低，会导致此命令报错，可以尝试使用 `-std=c++11`，也包含了大部分实用的特性，如 `auto` `for(int x : d)` 等，或安装 [此版本](../noixlbspz/dev-cpp_5.11_tdm-gcc_4.9.2_setup.exe) 的Dev-C++。
2. `-Wl,--stack=268435456`，将栈空间确定为 256MB。
3. `-Wall` `-Wextra`，开启更多警告信息，如 `long long` 变量使用 `%lld` 读入和输出、未使用的变量、逻辑运算符优先级、多个 `if` 与 `else` 配对问题、语句 `if(a=1)` 等。
4. `-Ox`，开启 Ox 优化，NOIP 评测机为 `-O`，NOI 评测机为 `-O2`。
5. `-DXXX`，自定义标识符，与 `#define XXX` 效果类似。
6. `-g`，要**手动**调试需要打开。

注：`-fsanitize=undefined`，检测未定义的行为，但 NOI 系列比赛并不支持，不过许多未定义的行为在 `-Wall` `-Wextra` 中已经包含了。

??? note "测试以上编译选项"

    配置好编译选项之后，复制以下代码并编译运行：
    
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    vector<int> d = {0, 1, 2, 3, 4};
    
    bool f(int x) {
        if(x == 1) return true;
    }
    
    int main() {
        int a;
        if(a == 1) printf("1\n");
        a = 0;
        if(a = 1) printf("2\n");
        if(a == 2) printf("3\n");
        else printf("4\n");
        for(auto x : d) printf("%d ", x);
        bool b = f(1);
        return 0;
    }
    ```
    
    如图，若支持 C++ 11 的新特性，并且相关警告也都正确提示，则配置正常。
    
    ![测试](../noixlbspz/test.webp)

## 关于 Dev-C++

1. Dev-C++ 几乎没有代码补全，因此特别需要注意经常依赖代码补全的函数名，如不同 STL 的头尾元素和放入取出操作的函数名，以及很长的函数名，如 `next_permutation` `setprecision` 等。
2. 如果不能调试，~~建议不调试~~，检查编译选项是否为 `debug` 而不是 `release`，生成代码性能分析是否为 `Yes`，产生调试信息是否为 `Yes`。
3. 建议开启自动保存，且手动备份文件到其他文件夹，防止类似编译命令打错导致源码被覆盖等问题。特别地，大段没用的代码建议暂存到其他文件，而不是直接删除。

![Dev-C++设置图片](../noixlbspz/settings.webp)

---

参考资料和推荐阅读：

1. [命令行 - OI wiki](https://oi-wiki.org/tools/cmd/)
2. [OIer 必知的编程技巧 - 洛谷文章(StudyingFather)](https://www.luogu.com.cn/article/uuh1asja)
3. [OI选手常见作死错误列表 - 洛谷文章(StudyingFather)](https://www.luogu.com/article/t8d90ewp)
4. [NOI 系列赛常见技术问题整理 - 洛谷文章(StudyingFather)](https://www.luogu.com/article/kgofa37e)
5. [关于 C++ 未定义行为的一些事 - 洛谷文章(StudyingFather)](https://www.luogu.com.cn/article/ue93zsuy)
6. [gcc/g++ 编译选项详解 - CSDN(moneymyone)](https://blog.csdn.net/moneymyone/article/details/132265876)
