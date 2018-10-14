>文章链接：[https://mp.weixin.qq.com/s/yiFOmljhyalE8ssAgwo6Jw](https://mp.weixin.qq.com/s/yiFOmljhyalE8ssAgwo6Jw)

关于python图片转字符画，相信大家都不陌生，经常出现在 n个超有趣的python项目中。  
今天我也来实践这个有趣的项目，更进一步的是把这个功能做成一个在线的网站，直接上传图片生成字符画，在线预览可以让更多的人来体验。      
体验网址：[https://www.manjiexiang.cn/blog/post_img](https://www.manjiexiang.cn/blog/post_img)  
举个栗子，就是这是一张图片   
![](https://user-gold-cdn.xitu.io/2018/10/8/166542172d8f837d?w=300&h=310&f=png&s=51556)   
经过转换成的字符画是这样的，这个txt的文件  
![](https://user-gold-cdn.xitu.io/2018/10/9/16656e9df316de5b?w=300&h=406&f=png&s=69307)
  
代码部分：  
使用PIL处理图片，resize方法转成指定宽高
```
from PIL import Image
im = Image.open("qq.png")
im = im.resize((width, height), Image.NEAREST)
```
像素转字符方法，将r，b，g转化为灰度值，然后根据灰度值的大小确定所选字符在ascii_char中的位置。
```
def get_char(r, g, b, alpha=256):
    ascii_char = list("$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/\|()1{}[]?-_+~<>i!lI;:,\"^`'. ")
    if alpha == 0:
        return ' '
    length = len(ascii_char)
    gray = int(0.2126 * r + 0.7152 * g + 0.0722 * b)

    unit = (256.0 + 1) / length
    return ascii_char[int(gray / unit)]
```
处理图片，遍历图片中的每一个像素，依次调用get_char方法即可得到每一个像素对应的字符，然后将这些字符组合起来即可得到所需的字符画了，输出到txt文件。
```
def draw():
    im = Image.open("qq.png")
    im = im.resize((width, height), Image.NEAREST)
    txt = ""
    for i in range(height):
        for j in range(width):
            txt += get_char(*im.getpixel((j, i)))
        txt += '\n'
    with open("qq.txt", 'w') as f:
        f.write(txt)
```
至此，生成字符画的脚本完成了。   
接下来就是运用到网站项目中，区别是图片是上传的，不是本地的路径，这里采用的是django的项目，图片上传到项目里的路径
```
media_root = os.path.join(settings.BASE_DIR, 'upload/')
```
原本想将生成的txt文件下载下来的，发现下载的txt文件里面字符画错乱了，索性就重定向进行浏览，效果一样。  
![](https://user-gold-cdn.xitu.io/2018/10/10/1665be3301d71c26?w=440&h=196&f=png&s=8615)   
设置的宽高可以修改生成字符画的大小，即上面的width、height   
网站地址：  
[https://www.manjiexiang.cn/blog/post_img](https://www.manjiexiang.cn/blog/post_img)   

欢迎大家使用   

脚本github地址：[https://github.com/taixiang/py_draw](https://github.com/taixiang/py_draw)

欢迎关注我的个人博客：[https://www.manjiexiang.cn/](https://www.manjiexiang.cn/)  

更多精彩欢迎关注微信号：春风十里不如认识你  
一起学习，一起进步，欢迎上车，有问题随时联系，一起解决！！！

![](https://user-gold-cdn.xitu.io/2018/8/12/1652cd77eaebeb98?w=900&h=540&f=jpeg&s=64949)    
