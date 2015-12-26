title: Java正则表达式(二)
date: 2015-12-25 22:26:27
tags: regex
---

**Java正则表达式工具类**

<!-- more -->

```java
/**
 * 正则表达式
 * @author 小破孩
 * 2015-12-22
 */
public class Matching {
    
    //匹配中文字符
    public static boolean getMatchingChina(String str){
        boolean flag = false;
        String pattern = "[\\u4e00-\\u9fa5]+";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }

    //匹配英文字符
    public static boolean getMatchingEnglish(String str){
        boolean flag = false;
        String pattern = "[A-Za-z]+";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配英文字符大写
    public static boolean getMatchingEnglishAZ(String str){
        boolean flag = false;
        String pattern = "[A-Z]+";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配英文字符小写
    public static boolean getMatchingEnglishaz(String str){
        boolean flag = false;
        String pattern = "[a-z]+";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配输入数字
    public static boolean getMatchingNumber(String str){
        boolean flag = false;
        String pattern = "\\d+";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配输入数字位数介于a->b
    public static boolean getMatchingNumber(String str,int a, int b){
        boolean flag = false;
        String pattern = "\\d{"+a+","+b+"}";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配最多输入n位数字
    public static boolean getMatchingNumberMaxLength(String str, int n){
        boolean flag = false;
        String pattern = "\\d{"+n+"}";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配至少输入n位数字
    public static boolean getMatchingNumberMinLength(String str, int n){
        boolean flag = false;
        String pattern = "\\d{"+n+",}";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配正整数或正小数(保留到小数点后6位)
    public static boolean getMatchingPositiveNumber(String str){
        boolean flag = false;
        String pattern = "([1-9]\\d*|0)\\.{0,1}[0-9]{1,6}";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配正整数
    public static boolean getMatchingPositiveInteger(String str){
        boolean flag = false;
        String pattern = "[1-9]\\d*";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配负整数
    public static boolean getMatchingNegtiveInteger(String str){
        boolean flag = false;
        String pattern = "-[1-9]\\d*";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配数字、英文字母、_组成的字符序列
    public static boolean getMatchingPassWord(String str){
        boolean flag = false;
        String pattern = "\\w+";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配以英文字母开头,由英文字母、数字、_组成的字符序列,并且指定输入长度
    public static boolean getMatchingPassWord(String str, int a, int b){
        boolean flag = false;
        String pattern = "[a-zA-Z]\\w{"+a+","+b+"}";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配邮箱
    public static boolean getMatchingEmail(String str){
        boolean flag = false;
        String pattern = "\\w[-\\w.+]*@([A-Za-z0-9][-A-Za-z0-9]+\\.)+[A-Za-z]{2,14}";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配时分秒(hh:mm:ss)
    public static boolean getMatchingDataHHMMSS(String str){
        boolean flag = false;
        String pattern = "([01]?\\d|2[0-3]):[0-5]?\\d:[0-5]?\\d";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配年月日(yyyy-MM-dd)
    public static boolean getMatchingDataYYYYMMDD(String str){
        boolean flag = false;
        String pattern = "((((1[6-9]|[2-9]\\d)\\d{2})-(0?[13578]|1[02])-(0?[1-9]|[12]\\d|3[01]))|(((1[6-9]|[2-9]\\d)\\d{2})-(0?[13456789]|1[012])-(0?[1-9]|[12]\\d|30))|(((1[6-9]|[2-9]\\d)\\d{2})-0?2-(0?[1-9]|1\\d|2[0-8]))|(((1[6-9]|[2-9]\\d)(0[48]|[2468][048]|[13579][26])|((16|[2468][048]|[3579][26])00))-0?2-29-))";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配身份证号(18位)
    public static boolean getMatchingCardId(String str){
        boolean flag = false;
        String pattern = "\\d{15}|\\d{17}[0-9Xx]";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配手机号
    //移动：134、135、136、137、138、139、150、151、157(TD)、158、159、183、187、188 
    //联通：130、131、132、152、155、156、185、186 
    //电信：133、153、180、189、（1349卫通） 
    public static boolean getMatchingPhoneNumber(String str){
        boolean flag = false;
        String pattern = "((13[0-9])|(15[^4,\\d])|(18[0,3,5-9]))\\d{8}";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配IP
    public static boolean getMatchingIP(String str){
        boolean flag = false;
        String pattern = "(1\\d{2}|2[0-4]\\d|25[0-5]|[1-9]\\d|[1-9])\\."
                            +"(00?\\d|1\\d{2}|2[0-4]\\d|25[0-5]|[1-9]\\d|\\d)\\."  
                            +"(00?\\d|1\\d{2}|2[0-4]\\d|25[0-5]|[1-9]\\d|\\d)\\."  
                            +"(00?\\d|1\\d{2}|2[0-4]\\d|25[0-5]|[1-9]\\d|\\d)";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
    
    //匹配HTTP
    public static boolean getMatchingHTTP(String str){
        boolean flag = false;
        String pattern = "http://([\\w-]+\\.)+[\\w-]+(/[\\w-./?%&=]*)?";
        Pattern r = Pattern.compile(pattern);
        Matcher m = r.matcher(str);
        flag = m.matches();
        return flag;
    }
}
```