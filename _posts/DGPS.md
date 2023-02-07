# DGPS专题

## 初步使用

1. [用户手册](https://wiki.blicube.com/grtk/zh/GRTK%E7%94%A8%E6%88%B7%E6%89%8B%E5%86%8C/#11)

   注意基站天线为ANT1，检查PWR与FIX灯是否亮起

   测试时连移动站杜邦接口，检查PWR、FIX、RTK是否亮起

2. 数传模块配置
   
   ![2023年2月7日修改](https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/20230207152726.png)

   修改空中速率为115200，波特率为57600

   修改基站端发射功率为7（最高）

3. [DRTK配置](https://wiki.blicube.com/grtk/zh/%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE%E5%91%BD%E4%BB%A4/#mode)
   
   修改DRTK基站与移动站配置

   ```
   FRESET
   GPGGA COM2 0.1  
   GPRMC COM2 0.1  
   GPHDT COM2 0.1  
   KSXT COM2 0.1
   SAVECONFIG
   (PS：此处需要使用CR&LF换行)
   ```

   ```
   CONFIG COM1 57600 8 n 0   
   saveconfig
   ```

   ```
   CONFIG RTK TIMEOUT 60
    ```

4. 测试时注意两天线要有一定高度差？

## 数据处理

1. [GPS定位精度](https://blog.csdn.net/weixin_36553855/article/details/86997798)

    水平精度以圆概率误差(CEP) 意味着 50% 的结果在给出的圆直径内，50%的结果在圆外。

    RMS是1 sigma或1倍标准差，如果结果是无偏的，概率为67%。

    2D RMS是2 sigma或2倍标准差，概率为95%。
    
    CEP 乘以1.2能转换为RMS，CEP 乘以2.4能转换为2D RMS。

    2.5M CEP -> 3M RMS -> 6M 2D RMS

2. [格式详解](https://bbs.huaweicloud.com/blogs/313486)

    GPS NMEA采集到的经度和纬度格式分别是：
    纬度ddmm.mmmmm（度分）
    经度dddmm.mmmmm（度分）

    转换后维度 = dd + (mm.mmmmm)/60
    转换后精度 = ddd + (mm.mmmmm)/60

3. [gps_common](https://codeleading.com/article/94243225958/)

    [wiki](https://github.com/swri-robotics/gps_umd/tree/ros2-devel)

4. 注意事项

    1. The master antenna, aka "ANT1", represents the location GRTK is positioning.

    2. 中国地图偏移

        [谷歌earth](https://earth.google.com/web/search/114.35779067E+30.54709515N/@30.54729759,114.35855255,23.60485661a,535.66431302d,35y,-0.00797182h,44.99663574t,0r/data=Cl4aNBIuGW_MV24OjD5AIQfU9grmllxAKhoxMTQuMzU3NzkwNjdFIDMwLjU0NzA5NTE1ThgCIAEiJgokCeEvcxfDjD5AEXUkxQjViz5AGTWwExIjl1xAIVSPf02kllxA)
    
        ![](https://pictures-kiana.oss-cn-beijing.aliyuncs.com/img/202209261123847.png)

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
