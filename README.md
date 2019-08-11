* 1,命令行查看系统目录:```dir```
* 2,解决找不到javaw的问题:
  * 可以拷贝一个jar到kettle的安装目录下，然后将jre重命名为java,
  * 注：我只是拷贝javaw到tools目录下，然后命令行激活```HardwareSimulator```
* 3,不能将```nand2tetris```文件夹放置在C盘（准确的说不能放在Program File (x86)）下，否则，可能存在写保护，无法运行程序
* 4,程序运行步骤
  * 工具分为tools和project两个文件夹，打开tools文件夹，打开hardwaresimulator.bat，
  * 在project文件夹中，编辑hdl文件，写入代码，保存后在硬件仿真器中load chip,
  * 并load script(导入测试脚本)，运行查看结果是否正确。
  * (可以在view下选择compare，导入.cmp文件，单步执行参看是否正确)
  * 注：最后一步我没成功（选择compare，不会导入.cmp），
