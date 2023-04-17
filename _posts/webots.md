## 传感器添加

### gps

1. 基本说明

    使用2个gps，gps_truth无噪声作为真实值，gps_meas添加噪声作为测量值。

    ```c++
    GPS {
    SFString type             "satellite"   # {"satellite", "laser"}
    SFFloat  accuracy         0             # [0, inf) 标准差
    SFFloat  noiseCorrelation 0             # [0, 1] 上一秒噪声对现在影响，0代表无影响
    SFFloat  resolution       -1            # {-1, [0, inf)} 分辨率，-1表示无穷
    SFFloat  speedNoise       0             # [0, inf)
    SFFloat  speedResolution  -1            # {-1, [0, inf)}
    }    
    ```

    输出数据
    ```
    std::cout << " Right Sensor Value:" << ds[0]->getValue() << "  Left Sensor Value:" << ds[1]->getValue() << std::endl;

    std::cout << "GPS Value X: " << gps_meas->getValues()[0]
        << " Y: " << gps_meas->getValues()[1] << " Z: " << gps_meas->getValues()[2] << std::endl;
    std::cout << "Truth X: " << gps_truth->getValues()[0]
        << " Y: " << gps_truth->getValues()[1] << " Z: " << gps_truth->getValues()[2] << std::endl;

    std::cout << " leftSpeed:" << leftSpeed << "  rightSpeed:" << rightSpeed << std::endl;
    ```