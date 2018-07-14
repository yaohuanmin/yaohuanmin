                                                 基于Python的图片颜色检测
一.大致思路

1.首先建立一个颜色字典用来定义11种基本的颜色，用于后续查找使用

2.在颜色检测函数中利用循环对字典里面的每一种颜色进行检测，并计算每一种颜色所占面积

3.通过对面积大小的判断，确定是否保存处理后的图片

4.由此计算出了读入图片中含有的颜色并进行了标记保存

二.程序代码

    import  cv2
    import numpy as np
    import collections
    
    def getColorList():
        dict = collections.defaultdict(list)

    #黑色
    lower_black = np.array([0, 0, 0])      
    upper_black = np.array([180, 255, 46])
    color_list = []
    color_list.append(lower_black)
    color_list.append(upper_black)
    dict['black'] = color_list
    
    #灰色
    lower_gray = np.array([0, 0, 46])
    upper_gray = np.array([180, 43, 220])
    color_list = []
    color_list.append(lower_gray)
    color_list.append(upper_gray)
    dict['gray']=color_list
    
    #白色
    lower_white = np.array([0, 0, 221])
    upper_white = np.array([180, 30, 255])
    color_list = []
    color_list.append(lower_white)
    color_list.append(upper_white)
    dict['white'] = color_list
 
    #红色  
    lower_red = np.array([156, 43, 46])
    upper_red = np.array([180, 255, 255])
    color_list = []
    color_list.append(lower_red)
    color_list.append(upper_red)
    dict['red']=color_list

    #红色2
    lower_red = np.array([0, 43, 46])
    upper_red = np.array([10, 255, 255])
    color_list = []
    color_list.append(lower_red)
    color_list.append(upper_red)
    dict['red2'] = color_list
 
    #橙色
    lower_orange = np.array([11, 43, 46])
    upper_orange = np.array([25, 255, 255])
    color_list = []
    color_list.append(lower_orange)
    color_list.append(upper_orange)
    dict['orange'] = color_list


    #黄色
    lower_yellow = np.array([26, 43, 46])
    upper_yellow = np.array([34, 255, 255])
    color_list = []
    color_list.append(lower_yellow)
    color_list.append(upper_yellow)
    dict['yellow'] = color_list
 
    
    #绿色 
    lower_green = np.array([35, 43, 46])
    upper_green = np.array([77, 255, 255])
    color_list = []
    color_list.append(lower_green)
    color_list.append(upper_green)
    dict['green'] = color_list
 
    #青色
    lower_cyan = np.array([78, 43, 46])
    upper_cyan = np.array([99, 255, 255])
    color_list = []
    color_list.append(lower_cyan)
    color_list.append(upper_cyan)
    dict['cyan'] = color_list
 
    #蓝色
    lower_blue = np.array([100, 43, 46])
    upper_blue = np.array([124, 255, 255])
    color_list = []
    color_list.append(lower_blue)
    color_list.append(upper_blue)
    dict['blue'] = color_list
 
    #紫色
    lower_purple = np.array([125, 43, 46])
    upper_purple = np.array([155, 255, 255])
    color_list = []
    color_list.append(lower_purple)
    color_list.append(upper_purple)
    dict['purple'] = color_list
 
    return dict
 

    #颜色检测函数
    def get_color():
      count=0   #记录图片中颜色种类
      color_dict = getColorList()
      for d in color_dict:          #判断颜色
        frame = cv2.imread('888.jpg',-1)  #导入图片
        hsv = cv2.cvtColor(frame,cv2.COLOR_BGR2HSV)   #把BGR图像转换为HSV格式
        
        kernel_2 = np.ones((2,2),np.uint8)    #2x2的卷积核
        kernel_3 = np.ones((3,3),np.uint8)    #3x3的卷积核
        kernel_4 = np.ones((4,4),np.uint8)    #4x4的卷积核
        mask = cv2.inRange(hsv,color_dict[d][0],color_dict[d][1])           #把HSV图片中在颜色范围内的区域变成白色，其他区域变成黑色
        
        #用卷积进行滤波
        erosion = cv2.erode(mask,kernel_4,iterations = 1)
        erosion = cv2.erode(erosion,kernel_4,iterations = 1)
        dilation = cv2.dilate(erosion,kernel_4,iterations = 1)
        dilation = cv2.dilate(dilation,kernel_4,iterations = 1)
   
        #把原图中的非目标颜色区域去掉剩下的图像
        target = cv2.bitwise_and(frame, frame, mask=dilation)
   
        #将滤波后的图像变成二值图像放在binary中
        ret, binary = cv2.threshold(dilation,127,255,cv2.THRESH_BINARY) 
        #在binary中发现轮廓，轮廓按照面积从小到大排列
        opt,contours, hierarchy = cv2.findContours(binary,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)
       
        #计算颜色所占面积
        area = 0
        for i in range(len(contours)):
            area += cv2.contourArea(contours[i])
        
        #证明颜色存在
        if area>0.5:
          count =count+1
          cv2.drawContours(frame,contours,-1,(0,0,255),3)    #画出图形轮廓
          cv2.imwrite(d+'.jpg', frame)                    #存储图片
          print(d) 

    return count  
    if __name__ == '__main__':
      print("the detected colours:")
      print(get_color())
 
 
三.实验结果
1.读入的图片

![image](https://github.com/yaohuanmin/yaohuanmin/blob/master/888.jpg)

2.运行代码后的结果

控制台运行
![image](https://github.com/yaohuanmin/yaohuanmin/blob/master/1.png)

检测到的蓝色
![image](https://github.com/yaohuanmin/yaohuanmin/blob/master/blue.jpg)

检测到的绿色
![image](https://github.com/yaohuanmin/yaohuanmin/blob/master/green.jpg)

检测到的红色
![image](https://github.com/yaohuanmin/yaohuanmin/blob/master/red.jpg)

检测到的红色2
![image](https://github.com/yaohuanmin/yaohuanmin/blob/master/red2.jpg)

检测到的黄色
![image](https://github.com/yaohuanmin/yaohuanmin/blob/master/yellow.jpg)




    
