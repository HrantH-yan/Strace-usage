# Strace-usage

Stracing IO.cpp(executable file(IO)) using   strace -o IO.txt ./IO 
gives large amount of code, so I compared it with another "straced" blank
file(containes only main function) to see differences with IO.txt file

In IO.txt file in line 10 libstdc++.so.6 was opened; 
same thing in lines [31;46](some libs opened) libm.so.6 , libgcc_s.so.1
libm.so.6 was opened in Blank.txt too.

in the [67;70] lines is the actual write(std::cout<<) and read(std::cin>>) from my code

write has 3 parameters
-- first parameter is 1 which is the device(monitor) interrupt number
-- second is message pointer ("The begining\n" or "The end\n") 
-- third is the size of message (in bytes) (max 1024)


read has 3 parameters 0 is the device(keyboard) num
-- first parameter is 0 which is the device(keyboard) interrupt number
-- second is message pointer ("<Input>") 
-- third is the size of input (in bytes) (max 1024)

also if given input or message is too big(>1024) then read and write syscalls will split,
and message pointers will be like this

for example: if message is "absdefjhij" x 110

read(0, "abcdefghijabcdefghijabcdefghijab"..., 1024) = 1024
read(0, "efghijabcdefghijabcdefghijabcdef"..., 1024) = 77

write(1, "abcdefghijabcdefghijabcdefghijab"..., 1024) = 1024
write(1, "efghijabcdefghijabcdefghijabcdef"..., 84) = 77

 