---
title: 串口学习笔记
date: 2023-6-28 15:54:58
categories:
  - 经验随笔
tags:
  - Serial
  - Port
---

### 简介

串口学习笔记

<!-- more -->

### 一、什么是串口

串口通信(Serial Communication)，是一种用于在计算机和外部设备之间传输数据的通信方式。它使用串行数据传输的方式，通过计算机的串口与外部设备进行数据交换。串口通信常用于连接各种设备，例如打印机、调制解调器、传感器等。它通过逐位传输数据，可以实现可靠的数据传输和通信。

### 二、常用的串口通信方式主要包括以下几种

- RS-232：
  > RS-232是一种常见的串口通信标准，用于在计算机和外部设备之间传输数据。它使用DB-9或DB-25连接器，支持较短距离的数据传输。

- RS-485：
  > RS-485是一种多点通信协议，可以实现多个设备之间的串口通信。它支持较长距离的数据传输，通常用于工业控制系统和远程监控等领域。

- USB串口：
  > USB串口是通过USB接口进行串口通信的方式。它广泛应用于各种外部设备，如打印机、调制解调器、数码相机等。

- TTL串口：
  > TTL串口是一种基于TTL电平的串口通信方式，常用于嵌入式系统和单片机开发中。

### 三、串口怎样通信

串口通信的基本原理是通过发送和接收数据位来实现。下面是串口通信的基本步骤：

1. 确定串口参数：首先需要确定串口的参数，包括波特率（Baud Rate）、数据位（Data Bits）、停止位（Stop Bits）和校验位（Parity）等。这些参数需要在发送和接收端保持一致。

   > 以下是一些常见的串口参数定义方式：
   >
   > - 波特率（Baud Rate）：指的是每秒传输的比特数。常见的波特率有9600、115200等。发送和接收数据的设备必须使用相同 的波特率才能正常通信。
   > - 数据位（Data Bits）：指的是每个数据字节中所使用的位数。常见的数据位有5、6、7、8位。
   > - 停止位（Stop Bits）：指的是每个数据字节之后的停止位数。常见的停止位有1位和2位。
   > - 校验位（Parity Bit）：用于数据传输的错误检测。常见的校验位有无校验位、奇校验位和偶校验位。
   >
   > 串口参数的定义可以根据具体的应用需求进行配置，以确保设备之间的串口通信正常进行。

2. 打开串口：在计算机或设备上，需要打开串口以进行通信。这可以通过编程语言或串口通信工具实现。

   在C#中，你可以使用 `System.IO.Ports` 命名空间提供的 `SerialPort` 类来打开和操作串口。下面是使用C#打开232和485串口的示例代码：

   - 打开232串口：

     ```csharp
      using System.IO.Ports;
      // 创建SerialPort对象
      SerialPort serialPort = new SerialPort("COM1", 9600, Parity.None, 8, StopBits.One);
      // 打开串口
      serialPort.Open();
      // 串口打开后，可以进行数据的读取和写入操作
      // 关闭串口
      serialPort.Close();
     ```

   - 打开485串口：

     ```csharp
     using System.IO.Ports;
     // 创建SerialPort对象
     SerialPort serialPort = new SerialPort("COM1", 9600, Parity.None, 8, StopBits.One);
     // 设置485串口为半双工模式
     serialPort.RtsEnable = true;
     serialPort.DtrEnable = false;
     // 打开串口
     serialPort.Open();
     // 串口打开后，可以进行数据的读取和写入操作
     // 关闭串口
     serialPort.Close();
     ```

   > 请注意，上述代码中的参数值可能需要根据你的具体需求进行调整。同时，确保在使用完串口后及时关闭串口以释放资源。
   >
   > [点此查看SerialPort类说明:](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.ports.serialport?view=dotnet-plat-ext-7.0)

3. 发送数据：在发送端，将要发送的数据转化为二进制形式，并按照串口参数进行传输。通常是按照字节（8位）的形式发送数据。

   ```csharp
   // 要发送的数据
   string data = "Hello, World!";
   // 将数据转换为ASCII码
   byte[] asciiBytes = System.Text.Encoding.ASCII.GetBytes(data);
   // 发送数据
   serialPort.Write(asciiBytes, 0, asciiBytes.Length);
   ```

4. 接收数据：在接收端，通过串口接收器接收数据，并将其转化为可读形式。接收端需要按照相同的串口参数配置进行接收。

   ```csharp
   // 设置接收事件处理程序
   serialPort.DataReceived += SerialPort_DataReceived;
   private static void SerialPort_DataReceived(object sender, SerialDataReceivedEventArgs e)
   {
       SerialPort serialPort = (SerialPort)sender;
       // 读取接收缓冲区中的所有可用字节
       byte[] buffer = new byte[serialPort.BytesToRead];
       serialPort.Read(buffer, 0, buffer.Length);
       // 将接收到的字节数组转换为字符串
       string receivedData = System.Text.Encoding.ASCII.GetString(buffer);
   }
   ```

5. 数据校验：为了确保数据的完整性和准确性，可以使用校验位进行数据校验。常见的校验方式包括奇偶校验和循环冗余校验（CRC）等。

   ```csharp
   private static void SerialPort_DataReceived(object sender, SerialDataReceivedEventArgs e)
   {
       SerialPort serialPort = (SerialPort)sender;

       // 读取接收缓冲区中的所有可用字节
       byte[] buffer = new byte[serialPort.BytesToRead];
       serialPort.Read(buffer, 0, buffer.Length);

       // 按照串口参数规则与8位校验校验数据
       if (buffer.Length == serialPort.DataBits / 8 + 1)
       {
           // 校验停止位
           if (serialPort.StopBits == StopBits.One && buffer[buffer.Length - 1] != 0x00)
           {
               Console.WriteLine("停止位校验失败");
               return;
           }
           else if (serialPort.StopBits == StopBits.Two && buffer[buffer.Length - 1] != 0xFF)
           {
               Console.WriteLine("停止位校验失败");
               return;
           }

           // 校验奇偶校验位
           if (serialPort.Parity == Parity.None)
           {
               // 无奇偶校验位，直接输出数据
               Console.WriteLine("数据校验通过：" + BitConverter.ToString(buffer));
           }
           else if (serialPort.Parity == Parity.Even)
           {
               // 偶校验
               int parityBit = buffer[buffer.Length - 1];
               int dataBits = serialPort.DataBits;
               bool isEvenParity = IsEvenParity(buffer, dataBits);
               if (isEvenParity && parityBit == 0x00)
               {
                   Console.WriteLine("数据校验通过：" + BitConverter.ToString(buffer));
               }
               else
               {
                   Console.WriteLine("奇偶校验位校验失败");
               }
           }
           else if (serialPort.Parity == Parity.Odd)
           {
               // 奇校验
               int parityBit = buffer[buffer.Length - 1];
               int dataBits = serialPort.DataBits;
               bool isOddParity = IsOddParity(buffer, dataBits);
               if (isOddParity && parityBit == 0x00)
               {
                   Console.WriteLine("数据校验通过：" + BitConverter.ToString(buffer));
               }
               else
               {
                   Console.WriteLine("奇偶校验位校验失败");
               }
           }
       }
       else
       {
           Console.WriteLine("数据长度校验失败");
       }
   }

   private static bool IsEvenParity(byte[] data, int dataBits)
   {
       int count = 0;
       for (int i = 0; i < data.Length - 1; i++)
       {
           byte b = data[i];
           while (b != 0)
           {
               count++;
               b &= (byte)(b - 1);
           }
       }

       return count % 2 == 0;
   }

   private static bool IsOddParity(byte[] data, int dataBits)
   {
       return !IsEvenParity(data, dataBits);
   }
   ```

6. 数据处理：接收到数据后，可以根据需求进行相应的数据处理，如解析数据、执行相应的操作等。

   ```csharp
   void com_DataReceived(object sender, SerialDataReceivedEventArgs e)
   {
       // 使用二进制或字符串技术（但不能同时使用两者）
       // 缓冲和处理二进制数据
       while (com.BytesToRead > 0)
           bBuffer.Add((byte)com.ReadByte());
       ProcessBuffer(bBuffer);

       // 缓冲区字符串数据
       sBuffer += com.ReadExisting();
       ProcessBuffer(sBuffer);
   }

   private void ProcessBuffer(string sBuffer)
   {
       // 在字符串中查找有用信息
       // 然后从缓冲区中删除有用的数据
   }

   private void ProcessBuffer(List<byte> bBuffer)
   {
       // 在字节数组中查找有用的信息
       // 然后从缓冲区中删除有用的数据
   }
   ```

7. 关闭串口：通信完成后，需要关闭串口以释放资源。

> 需要注意的是，串口通信需要发送端和接收端的串口参数保持一致，否则数据可能无法正确传输。同时，串口通信的稳定性和可靠性较差，对于长距离通信或高速通信，可能需要使用更可靠的通信方式。

### 参考资料

> [串口RS232和RS485](https://blog.csdn.net/weixin_44841569/article/details/128270940)
> [串口通信校验方式：奇偶校验、累加和校验](https://blog.csdn.net/sinat_16643223/article/details/118357650)
> [C#实现串口通信解析](https://blog.csdn.net/kalvin_y_liu/article/details/126885528)
> [C#实现串口通信的上位机开发](https://blog.csdn.net/weixin_41012765/article/details/125024048)
