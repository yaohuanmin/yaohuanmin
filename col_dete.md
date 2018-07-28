                                                   基于Python的图片颜色检测2.0
                                                       
一.大致思路

1.首先建立一个颜色字典用来定义11种基本的颜色，用于后续查找使用

2.在颜色检测函数中利用循环对字典里面的每一种颜色进行检测，并计算每一种颜色所占比例

3.通过对图片比例的判断，确定图中是否含有此种颜色

4.由此判断图片中含有的颜色并进行了标记保存

二.程序代码

import  cv2
import numpy as np
import collections


def getColorList():
    dict = collections.defaultdict(list)
 
    lower_black = np.array([0, 0, 0])
    upper_black = np.array([180, 255, 46])
    color_list = []
    color_list.append(lower_black)
    color_list.append(upper_black)
    dict['black'] = color_list

    lower_gray = np.array([0, 0, 46])
    upper_gray = np.array([180, 43, 220])
    color_list = []
    color_list.append(lower_gray)
    color_list.append(upper_gray)
    dict['gray']=color_list

    lower_white = np.array([0, 0, 221])
    upper_white = np.array([180, 30, 255])
    color_list = []
    color_list.append(lower_white)
    color_list.append(upper_white)
    dict['white'] = color_list
 
   
    lower_red = np.array([156, 43, 46])
    upper_red = np.array([180, 255, 255])
    color_list = []
    color_list.append(lower_red)
    color_list.append(upper_red)
    dict['red']=color_list

    lower_red = np.array([0, 43, 46])
    upper_red = np.array([10, 255, 255])
    color_list = []
    color_list.append(lower_red)
    color_list.append(upper_red)
    dict['red2'] = color_list
 
  
    lower_orange = np.array([11, 43, 46])
    upper_orange = np.array([25, 255, 255])
    color_list = []
    color_list.append(lower_orange)
    color_list.append(upper_orange)
    dict['orange'] = color_list

    lower_yellow = np.array([26, 43, 46])
    upper_yellow = np.array([34, 255, 255])
    color_list = []
    color_list.append(lower_yellow)
    color_list.append(upper_yellow)
    dict['yellow'] = color_list
 
    
    lower_green = np.array([35, 43, 46])
    upper_green = np.array([77, 255, 255])
    color_list = []
    color_list.append(lower_green)
    color_list.append(upper_green)
    dict['green'] = color_list
 
   
    lower_cyan = np.array([78, 43, 46])
    upper_cyan = np.array([99, 255, 255])
    color_list = []
    color_list.append(lower_cyan)
    color_list.append(upper_cyan)
    dict['cyan'] = color_list
 
  
    lower_blue = np.array([100, 43, 46])
    upper_blue = np.array([124, 255, 255])
    color_list = []
    color_list.append(lower_blue)
    color_list.append(upper_blue)
    dict['blue'] = color_list
 
 
    lower_purple = np.array([125, 43, 46])
    upper_purple = np.array([155, 255, 255])
    color_list = []
    color_list.append(lower_purple)
    color_list.append(upper_purple)
    dict['purple'] = color_list
 
    return dict
 

def get_color():
    colour=[]
    count=0
    color_dict = getColorList()
    for d in color_dict:
        frame = cv2.imread('2017.jpg',-1)
        sp=frame.shape
        hsv = cv2.cvtColor(frame,cv2.COLOR_BGR2HSV)
        kernel_2 = np.ones((2,2),np.uint8)
        kernel_3 = np.ones((3,3),np.uint8)
        kernel_4 = np.ones((4,4),np.uint8)
        mask = cv2.inRange(hsv,color_dict[d][0],color_dict[d][1])
        
        erosion = cv2.erode(mask,kernel_4,iterations = 1)
        erosion = cv2.erode(erosion,kernel_4,iterations = 1)
        dilation = cv2.dilate(erosion,kernel_4,iterations = 1)
        dilation = cv2.dilate(dilation,kernel_4,iterations = 1)
   
        target = cv2.bitwise_and(frame, frame, mask=dilation)
   
        ret, binary = cv2.threshold(dilation,127,255,cv2.THRESH_BINARY) 
        opt,contours, hierarchy = cv2.findContours(binary,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)

        area = 0
        for i in range(len(contours)):
            area += cv2.contourArea(contours[i])

        rate=area/(sp[0]*sp[1])  

        if (rate>0.01):
          colour.append(d)
          count =count+1

    if(count>=2):
      print("parti-colour")
      print("the colours are:")
      print(colour)
    elif(count==1):
      print("pure-colour")
      print("the colour is:")
      print(colour)

    return count  

 
if __name__ == '__main__':
    print(get_color())


    
