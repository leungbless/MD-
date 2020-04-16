# BuuCTF--WriteUp

## Reverse-easyre

通过IDA Pro的反编译，我们找到Main函数

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int b; // [rsp+28h] [rbp-8h]
  int a; // [rsp+2Ch] [rbp-4h]

  _main();
  scanf("%d%d", &a, &b);
  if ( a == b )
    printf("flag{this_Is_a_EaSyRe}");
  else
    printf("sorry,you can't get flag");
  return 0;
}
```

所以，flag{this_Is_a_EaSyRe}





## Reverse-reverse1



通过在字符串中查找`Input the flag`，我们找到Main函数

```C
 for ( j = 0; j < strlen(Str2); ++j )
  {
    if ( Str2[j] == 111 ) // o
    {
        Str2[j] = 48; //  0
    }    
  }
  printf("input the flag:");  //puts()
  scanf("%20s", &Str1);
  v3 = strlen(Str2);
  if ( !strncmp(&Str1, Str2, v3) )
    printf("this is the right flag!\n");
  else
    printf("wrong flag\n");
  return 0;
}
```

所以结果是：flag{hell0_w0rld}





## Reverse-新年快乐



```C
{
  strcpy(&Str2, "HappyNewYear!");
  printf("please input the true flag:");
  scanf("%s", Str1);
  if ( !strncmp(Str1, &Str2, strlen(&Str2)) )
    puts("this is true flag!");
  else
    puts("wrong!");
  return 0;
}
```

flag{HappyNewYear!}