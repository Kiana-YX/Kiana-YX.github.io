# DRTK专题

## 理论基础

1. [四颗卫星确定GPS](https://blog.csdn.net/wjydym/article/details/88545359)

   不考虑钟差时，仅用3个卫星即可。考虑钟差（地面接收机与标准时间差值，标准原子钟太贵），增加一个参数。

   导航目的：PVT **定位、测速、授时**（Position、Velocity、Time）

   GNSS: 全球导航系统（GPS/BDS/GAL/GLO） + 区域导航系统（QZSS/IRNSS） + 增强系统（WAAS/MSAS/EGNOS/GAGAN）

      可实现高精度授时（纳秒量级），无法获取高精度的绝对的定位结果（分米量级，信号不好地方更差）

   RTK: 实时相对定位技术，得到两个设备之间的相对位置信息，一般在十公里以内

   网络RTK：通过云端算法实现虚拟基站。受限于网络

   精密单点定位技术：通过卫星播发更精确的卫星轨道信息，卫星钟差信息以及其他的一些偏差信息来实现高精度的绝对定位

2. GPS/INS组合定位

   浅组合模式是指直接利用 GPS 接收机输出的定位信息与 MINS 组合，位置、速度组合的捷联卡尔曼滤波方式是典型的代表。它一般用 GPS 和 MINS 输出的位置和速度信息的差值作为量测值，经过卡尔曼滤波，估计 MINS 的误差，然后对 MINS 系统输出进行校正。
   
   紧组合模式是指利用 GPS 的原始信息来和 MINS组合，伪距、伪距率组合的扩展卡尔曼滤波方式是典型的代表。它利用 MINS 输出的位置和速度信息来估计 GPS 的伪距和伪距率，并与 GPS 输出的原始测量数据一一伪距和伪距率进行比较，将差值作为综合滤波器的状态，经过滤波器的最优估计，分别给出校正 MINS 和 GPS 的信息。在这种情况下，滤波器的状态方程是线性的，而量测方程是非线性的。因此，对位置、速度等 MINS 误差量的估计是个非线性滤波的过程。
   
   通常情况卜，深组合模式的导航精度要高于浅组合模式，我国目前在该方面的实验应用仍以浅组合模式为主

## 初步使用

1. [用户手册](https://wiki.blicube.com/grtk/zh/GRTK%E7%94%A8%E6%88%B7%E6%89%8B%E5%86%8C/#11)

   注意基站天线为ANT1，检查PWR与FIX灯是否亮起

   测试时连移动站杜邦接口，检查PWR、FIX、RTK是否亮起

2. 数传模块配置
   
   ![型号参数](https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/8638892AA48984CE858D4FDFF301F992.jpg )

   ![2023年2月7日修改](https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/20230207152726.png)

   修改空中速率为115200，波特率为57600，此时空中延时36ms

   一般而言，**大数据传输优先选 GFSK 模块**，小数据并且要求远距离或低功耗的优先选 LORA 模块。

   传输距离的差异主要取决于**发射功率**。对单向通信的应用场景可选大功率做发射小功率做接收。双向通信的应用则需要两边相同发射功率，否则信号强度不对等，则传输距离不对称。修改基站端发射功率为7（最高）

   天线的增益越高，水平方向传输距离越远，条件允许时尽量采用外置天线，垂直于地面安装并且高度在**2米以上**有助于提升通讯效果（地面吸收无线电波），带磁性底座的天线吸附在铁皮物体上效果更佳。
   
   切忌将天线安装在全封闭的金属壳体内。非金属壳体也会因结构差异产生不同通讯效果。

3. [DRTK配置](https://wiki.blicube.com/grtk/zh/%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE%E5%91%BD%E4%BB%A4/#mode)
   
   <div align=center>     <img src="https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/20231019122321.png" width = "400" alt="20231019122321"/>    </div>  
   
   修改DRTK基站与移动站配置

   ```
   FRESET
   GPGGA COM2 0.1  
   GPRMC COM2 0.1  
   GPHDT COM2 0.1  
   KSXT COM2 0.1
   (PS：此处需要使用CR&LF换行)
   CONFIG COM1 57600 8 n 0   //不要忘了
   saveconfig
   ```

   ```
   CONFIG RTK TIMEOUT 60
    ```

4. ~~测试时注意两天线要有一定高度差？~~

   应该是地面吸收电磁波，不能俩都放地面

## 数据处理

1. [调试软件RTKLIB](https://wiki.blicube.com/grtk/zh/RTK-lib/)

   `rtkplot.exe`
   
2. [GPS定位精度](https://blog.csdn.net/weixin_36553855/article/details/86997798)

    水平精度以圆概率误差(CEP) 意味着 50% 的结果在给出的圆直径内，50%的结果在圆外。

    RMS是1 sigma或1倍标准差，如果结果是无偏的，概率为67%。

    2D RMS是2 sigma或2倍标准差，概率为95%。
    
    CEP 乘以1.2能转换为RMS，CEP 乘以2.4能转换为2D RMS。

    2.5M CEP -> 3M RMS -> 6M 2D RMS

3. [格式详解](https://bbs.huaweicloud.com/blogs/313486)

    GPS NMEA采集到的经度和纬度格式分别是：
    纬度ddmm.mmmmm（度分）
    经度dddmm.mmmmm（度分）

    转换后维度 = dd + (mm.mmmmm)/60
    转换后精度 = ddd + (mm.mmmmm)/60

    <div align=center>     <img src="https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/20230409165841.png" width = "600" alt="20230409165841"/>    </div>

   ```c
   //转换后维度 = dd + (mm.mmmmm)/60
   double convertDDMMToDegree(char *coordinate, int size)
   {
      char temp[4];
      strncpy(temp, coordinate, size);
      double preValue = atof(temp);               // 整数部分
      double lastValue = atof(coordinate + size); // 小数部分
      return preValue + lastValue / 60;
   }    
   ```

4. [gps_common](https://codeleading.com/article/94243225958/)

    [wiki](https://github.com/swri-robotics/gps_umd/tree/ros2-devel)

5. 注意事项

    1. The master antenna, aka "ANT1", represents the location GRTK is positioning.

    2. 中国地图偏移

        [谷歌earth](https://earth.google.com/web/search/114.35779067E+30.54709515N/@30.54729759,114.35855255,23.60485661a,535.66431302d,35y,-0.00797182h,44.99663574t,0r/data=Cl4aNBIuGW_MV24OjD5AIQfU9grmllxAKhoxMTQuMzU3NzkwNjdFIDMwLjU0NzA5NTE1ThgCIAEiJgokCeEvcxfDjD5AEXUkxQjViz5AGTWwExIjl1xAIVSPf02kllxA)
    
        ![](https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/202209261123847.png)

6. GPS数据处理
   
   1. [读取与写入](https://blog.csdn.net/y18771025420/article/details/105884911)

    ```c++
    #include <iostream>
    #include <fstream>

    //using namespace std;
    using std::ofstream;

    int main()
    {
        ofstream res("./gps.txt", std::ios::app);
        int i = 100;

        while (i--) {
            
            res << 3 << " " << 2 << " " << 1 << std::endl;
        }

        res.close();
    }
    ```
   2. 数据分析
    ```matlab
    clc;clear;
    gps_array = load('F:\Administrator\Desktop\gps.txt');  % 这里的load()参数是txt文件的地址，test_array就是所读取的数据
    x = gps_array(:,1);
    y = gps_array(:,2);

    sz = 4;
    c = linspace(1,10,length(x));   % 绘制颜色
    
    scatter(x, y ,sz ,c ,'filled');  

    axis("equal")   % 坐标轴等比例
    ```

    下载曲线拟合器 curve fitting

    命令行输入cftool

### 卡尔曼滤波

### 原理

1. 3种坐标系

    建议参考《GPS导航原理与应用》王慧南 p28
    
    [参考一](https://blog.csdn.net/xiaojinger_123/article/details/122112843)

    [椭球参数](https://baike.baidu.com/item/%E5%8F%82%E8%80%83%E6%A4%AD%E7%90%83/5153782)

    [参考代码](https://blog.csdn.net/taiyang1987912/article/details/112982150)
    
    <center>
         <img style="border-radius: 0.3125em;
         box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
         src="https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/202209301140951.png" width = "65%" alt=""/>
         <br>
         <div style="color:orange; border-bottom: 1px solid #d9d9d9;
         display: inline-block;
         color: #999;
         padding: 2px;">
            椭球参数
         </div>
    </center>
    <center>
         <img style="border-radius: 0.3125em;
         box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
         src="https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/202209301052075.jpg" width = "65%" alt=""/>
         <br>
         <div style="color:orange; border-bottom: 1px solid #d9d9d9;
         display: inline-block;
         color: #999;
         padding: 2px;">
            ECEF(Earth-Centered, Earth-Fixed)，地心地固直角坐标系
         </div>
    </center>

    <center>
         <img style="border-radius: 0.3125em;
         box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
         src="https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/202209301054951.png" width = "65%" alt=""/>
         <br>
         <div style="color:orange; border-bottom: 1px solid #d9d9d9;
         display: inline-block;
         color: #999;
         padding: 2px;">
            WGS-84坐标，LLA坐标(Longitude、Latitude、Altitude)
         </div>
    </center>

    <center>
         <img style="border-radius: 0.3125em;
         box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
         src="https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/202209301058992.png" width = "65%" alt=""/>
         <br>
         <div style="color:orange; border-bottom: 1px solid #d9d9d9;
         display: inline-block;
         color: #999;
         padding: 2px;">
            东北天坐标系(站心坐标系)
         </div>
    </center><center>
         <img style="border-radius: 0.3125em;
         box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
         src="https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/202209301112907.png" width = "65%" alt=""/>
         <br>
         <div style="color:orange; border-bottom: 1px solid #d9d9d9;
         display: inline-block;
         color: #999;
         padding: 2px;">
            东北天坐标系(站心坐标系)
         </div>
    </center>

    ![](https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/202209301722404.png)

    注意：地球表面坐标 (x,y,z)，和($\phi$, $\lambda$, $h$)

    其中，$\phi$维度，$\lambda$经度


## 代码实操

1. 数据转换

   