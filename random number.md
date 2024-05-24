```
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(){
    srand(time(0));
    int ans = (rand() % 100)+1;
}
```