

参考代码

```java
class KMP {
    public static int match(String s, String target) {
        int[] next = getNext(target);
        int index = 0;
        for (int i = 0; i < s.length();) {
            if (s.charAt(i) == target.charAt(index)) {
                i++;
                index++;
                if (index == target.length())
                    return i - target.length();
            } else {
                while (true) {
                    if (index == 0)
                        break;
                    index = next[index - 1];
                    if (s.charAt(i) == target.charAt(index)) {
                        index++;
                        break;
                    }
                }
                i++;
            }
        }
        return -1;
    }

    private static int[] getNext(String s) {
        int[] next = new int[s.length()];
        int index = 0;
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(index) == s.charAt(i)) {
                next[i] = ++index;
            } else {
                while (true) {
                    if (index == 0)
                        break;
                    index = next[index - 1];
                    if (s.charAt(index) == s.charAt(i)) {
                        next[i] = ++index;
                        break;
                    }
                }
            }
        }
        return next;
    }
}
```

