蓝牙、超声波控制+视频自动循迹小车

平台：车辆：arduino 数据处理：python3.7 + pytorch

器材：9v干电池*1，单层两轮小车*1，arduino uno开发板*1，L298N电机驱动模块*1，BT08B蓝牙串口模块*1，HC-SR04超声波单元*1，手机*1，杜邦线若干

实验过程：

1.通过pygame模块实现电脑键盘的监控，同时根据不同的键盘输入对小车进行不同的控制。根据不同的行进方向，通过手机返回的图像信息，存储为不同组别的图片，实现数据的采集。

2.从存储的图片中载入图片信息，使用opencv块实现图片的灰度化和二值化处理，使得道路两侧的标记与道路产生足够的区分度。并使用独热码实现对不同图片的标记。

3.使用pytorch模块，自定义模型。其中此次实验使用的模型为包含两个隐藏层，隐藏层之间的激活函数采用Relu函数，损失函数采用MSELoss函数，优化方法采用SGD函数，并设定相应的学习率。最终经过多次训练得到相应的模型。

注：自定义模型如下：

class MLP(nn.Module):

    def __init__(self, input_size, hidden1_size, hidden2_size,output_size):
    
        super(MLP, self).__init__()
        
        self.fc1 = nn.Linear(input_size, hidden1_size)
        
        self.fc2 = nn.Linear(hidden1_size, hidden2_size)
        
        self.fc3 = nn.Linear(hidden2_size, output_size)
        
    def forward(self, x):
    
        x = F.relu(self.fc1(x))
        
        x = F.relu(self.fc2(x))
        
        x = self.fc3(x)
        
        return x
        
4.模型训练结束后，通过定时的采集手机返回的图像信息，进行与采集数据时相同的图像处理，使用训练完成的模型进行预测，与不同行进方向对应的标签进行比对，得到正确的预测结果，并通过python的串口模块控制小车的运行。
