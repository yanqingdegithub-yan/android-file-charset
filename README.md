# android-file-charset
Android 获取文件的编码

注：本文属于转载
原文链接：[https://blog.csdn.net/u013144287/article/details/74455817](https://blog.csdn.net/u013144287/article/details/74455817)  遵循[ CC 4.0 BY-SA ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议

最后的ExchangeUtil代码经简单测试可以直接使用，支持的编码格式很多，我试过GB2313、GBK、UTF-8、BIG5等格式能正确获取到

使用方式
```
public String getJavaEncode(File file) {
     ExchangeUtils s = new ExchangeUtils();
     return ExchangeUtils.javaname[s.detectEncoding(file)];
}
```
个人使用时对源代码有个小修改，如下
```
private static final int FILE_BYTES_MAX = 1024 * 1024;

public int detectEncoding(File testfile) {
        FileInputStream chinesefile;
        byte[] rawtext;
        //做个最大值限制，防止内存溢出，但是不知道是否会影响准确性
        int length = (int)testfile.length();
        if(length > FILE_BYTES_MAX) {
            length = FILE_BYTES_MAX;
        }
        rawtext = new byte[length];
        try {
            chinesefile = new FileInputStream(testfile);
            chinesefile.read(rawtext);
            chinesefile.close();
        } catch(Exception e) {
            System.err.println("Error: " + e);
        }
        return detectEncoding(rawtext);
    }
```
源代码是读取文件的全部字节，这个考虑到文件特别大会有内存问题，所有做了一个最大读取字节的限制。（不知道会不会影响获取编码的准确度，我自己测试的没有影响）

这里的ExchangeUtil.java文件是未修改过的源文件
