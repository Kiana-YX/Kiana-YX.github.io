## ���������

### gps

1. ����˵��

    ʹ��2��gps��gps_truth��������Ϊ��ʵֵ��gps_meas���������Ϊ����ֵ��

    ```c++
    GPS {
    SFString type             "satellite"   # {"satellite", "laser"}
    SFFloat  accuracy         0             # [0, inf) ��׼��
    SFFloat  noiseCorrelation 0             # [0, 1] ��һ������������Ӱ�죬0������Ӱ��
    SFFloat  resolution       -1            # {-1, [0, inf)} �ֱ��ʣ�-1��ʾ����
    SFFloat  speedNoise       0             # [0, inf)
    SFFloat  speedResolution  -1            # {-1, [0, inf)}
    }    
    ```

    �������
    ```
    std::cout << " Right Sensor Value:" << ds[0]->getValue() << "  Left Sensor Value:" << ds[1]->getValue() << std::endl;

    std::cout << "GPS Value X: " << gps_meas->getValues()[0]
        << " Y: " << gps_meas->getValues()[1] << " Z: " << gps_meas->getValues()[2] << std::endl;
    std::cout << "Truth X: " << gps_truth->getValues()[0]
        << " Y: " << gps_truth->getValues()[1] << " Z: " << gps_truth->getValues()[2] << std::endl;

    std::cout << " leftSpeed:" << leftSpeed << "  rightSpeed:" << rightSpeed << std::endl;
    ```