***vault-door-6***
===

![alt text](image.png)

Tương tự như các bài **vault-door** trước, ta có đoạn code java:
```java
import java.util.*;

class VaultDoor6 {
    public static void main(String args[]) {
        VaultDoor6 vaultDoor = new VaultDoor6();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
        String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
        if (vaultDoor.checkPassword(input)) {
            System.out.println("Access granted.");
        } else {
            System.out.println("Access denied!");
        }
    }

    // Dr. Evil gave me a book called Applied Cryptography by Bruce Schneier,
    // and I learned this really cool encryption system. This will be the
    // strongest vault door in Dr. Evil's entire evil volcano compound for sure!
    // Well, I didn't exactly read the *whole* book, but I'm sure there's
    // nothing important in the last 750 pages.
    //
    // -Minion #3091
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
            0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
            0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
            0xa , 0x66, 0x36, 0x30, 0x67, 0x6c, 0x64, 0x6c,
        };
        for (int i=0; i<32; i++) {
            if (((passBytes[i] ^ 0x55) - myBytes[i]) != 0) {
                return false;
            }
        }
        return true;
    }
}
```

Ở trong **myBytes** có dãy số theo kiểu HEX, decode về lại kiểu DEC ta được dãy số sau:
```
59 101 33 10 56 0 54 29 
10 61 97 39 17 102 39 10 
33 29 97 59 10 45 101 39 
10 102 54 48 103 108 100 108
```
XOR các số ở trên với 85 (0x55 = 85) và chuyển lại về dạng kí tự, ta tìm được flag (chương trình C++ để tìm flag ở dưới):
```c++
#include <iostream>

using namespace std;

int a[32] = {59, 101, 33, 10, 56, 0, 54, 29,
            10, 61, 97, 39, 17, 102, 39, 10,
            33, 29, 97, 59, 10, 45, 101, 39,
            10, 102, 54, 48, 103, 108, 100, 108};
            
int main(){
    for (int i = 0; i < 32; i++){
        cout << (char)(a[i] ^ 85); // 0x55(hex) = 85(dec)
    }
    return 0;
}
```

Flag
=
```picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_3ce2919}```