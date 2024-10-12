> [!NOTE]  
> 以下代码片段只适用于Unix/Linux  
```C++
#include<termios.h>
#include<unistd.h>

void hideInput(bool hide) {
    struct termios tty;
    tcgetattr(STDIN_FILENO, &tty);
    if (hide) {
        tty.c_lflag &= ~ECHO;
    } else {
        tty.c_lflag |= ECHO;
    }
    tcsetattr(STDIN_FILENO, TCSANOW, &tty);
}
```