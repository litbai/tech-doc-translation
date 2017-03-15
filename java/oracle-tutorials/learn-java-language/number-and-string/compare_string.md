### 比较String和String的子串


String类提供了很多方法用来比较两个字符串或者字符串的一部分，如下：

|Method|Description|
|------|-----------|
|boolean endsWith(String suffix)|字符串是否以指定子串结尾|
|boolean startsWith(String prefix)|字符串是否以指定子串开始|
|boolean startsWith(String prefix, int offfset)|字符串从位置offset开始，是否以prefix开头|
|int compareTo(String anotherString)|根据字母表比较两个字符串， 返回负数、0、正数|
|boolean equals(Object obj)||
|boolean equalsIgnoreCase(String anotherString)|忽略大小写比较|
|boolean regionMatches(int toffset, String other, int ooffset, int len)|判断两个字符串的特定区域是否相同|
|boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)|是否忽略大小写|
|boolean matches(String regex)|字符串是否符合正则|

下面的程序，RegionMatchesDemo使用了regionMatches方法来搜寻字符串：

```
public class RegionMatchesDemo {
    public static void main(String[] args) {
        String searchMe = "Green Eggs and Ham";
        String findMe = "Eggs";
        int searchMeLength = searchMe.length();
        int findMeLength = findMe.length();
        boolean foundIt = false;
        for (int i = 0; 
             i <= (searchMeLength - findMeLength);
             i++) {
           if (searchMe.regionMatches(i, findMe, 0, findMeLength)) {
              foundIt = true;
              System.out.println(searchMe.substring(i, i + findMeLength));
              break;
           }
        }
        if (!foundIt)
            System.out.println("No match found.");
    }
}


```


























