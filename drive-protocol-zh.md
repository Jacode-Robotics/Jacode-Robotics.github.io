## [指令](#instruction)

| 数值 |                指令                 |                           描述                           |
| :--: | :---------------------------------: | :------------------------------------------------------: |
| 0x01 |          [Ping](#ins-ping)          |      检查数据包是否已到达与数据包ID相同的设备的指令      |
| 0x02 |          [Read](#ins-read)          |                  从设备中读取数据的指令                  |
| 0x03 |         [Write](#ins-write)         |                  在设备上写入数据的指令                  |
| 0x06 |     [Factory Reset](#ins-reset)     |          将控制表重置为其初始出厂默认设置的指令          |
| 0x08 |        [Reboot](#ins-reboot)        |                    重新启动设备的指令                    |
| 0x10 |         [Clear](#ins-clear)         |                    重置特定信息的指令                    |
| 0x20 | [Control Table Backup](#ins-backup) | 将当前控制表状态数据存储到备份区域或恢复EEPROM数据的指令 |
| 0x55 |    [Status(Return)](#ins-status)    |                  返回指令数据包的数据包                  |
| 0x8A |  [Fast Sync Read](#ins-sync-read)   |   对于多个设备，一次从相同地址读取相同长度的数据的指令   |
| 0x8B | [Fast Sync Write](#ins-sync-write)  |   对于多个设备，一次在相同地址写入相同长度的数据的指令   |

## [控制表](#control-table)

| 地址 | 字节长度 |                    数据名称                    | 权限 | 初始值 |                              范围                             |          单位         |
|:-------:|:----------:|:-----------------------------------------------:|:------:|:------------------:|:----------------------------------------------------------------:|:-----------------------:|
|    0    |     2      |          [Model Number](#model-number)          |   读   |       2,020        |                                -                                 |            -            |
|    6    |     1      |      [Firmware Version](#firmware-version)      |   读   |         -          |                                -                                 |            -            |
|    7    |     1      |                    [ID](#id)                    |   读写   |         1          | 0 ~ 252  |            -            |
|    8    |     1      |             [Baud Rate](#baud-rate)             |   读写   |         1          |                              0 ~ 9                               |            -            |
|   10    |     1      |            [Drive Mode](#drive-mode)            |   读写   |         0          |                              0 ~ 13                              |            -            |
|   11    |     1      |        [Operating Mode](#operating-mode)        |   读写   |         3          |                          0, 1, 3, 4, 16                          |            -            |
|   13    |     1      |        [Protocol Type](#protocol-type13)        |   读写   |         2          |                              2, 10                               |            -            |
|   20    |     4      |         [Homing Offset](#homing-offset)         |   读写   |         0          |                -2,147,483,648 ~<br> 2,147,483,647                |        1 [pulse]        |
|   31    |     1      |     [Temperature Limit](#temperature-limit)     |   读写   |         80         |                             0 ~ 100                              |       1 [&deg;C]        |
|   36    |     2      |             [PWM Limit](#pwm-limit)             |   读写   |       2,009        |                            0 ~ 2,009                             |       0.0498 [%]        |
|   44    |     4      |        [Velocity Limit](#velocity-limit)        |   读写   |       2,900        |                            0 ~ 2,900                             |     0.01 [rev/min]      |
|   48    |     4      |    [Max Position Limit](#max-position-limit)    |   读写   |      501,433       |                      -501,923 ~<br> 501,923                      |        1 [pulse]        |
|   52    |     4      |    [Min Position Limit](#min-position-limit)    |   读写   |      -501,433      |                      -501,923 ~<br> 501,923                      |        1 [pulse]        |
|   63    |     1      |              [Shutdown](#shutdown)              |   读写   |         52         |                              0 ~ 63                              |            -            |
|   512   |     1      |          [Torque Enable](#torque-enable)          |   读写   |         0          |                        0 ~ 1                        |            -            |
|   518   |     1      |  [Hardware Error Status](#hardware-error-status)  |   读   |         0          |                          -                          |            -            |
|   522   |     2      |       [Movement Duration](#movement-duration)     |   读写   |        2000        |                     0 ~ 65,536                      |            1 [msec]         |
|   524   |     2      |       [Velocity I Gain](#velocity-pi-gain)        |   读写   |        2209        |                     0 ~ 32,767                      |            -            |
|   526   |     2      |       [Velocity P Gain](#velocity-pi-gain)        |   读写   |        6153        |                     0 ~ 32,767                      |            -            |
|   528   |     2      |       [Position D Gain](#position-pid-gain)       |   读写   |         0          |                     0 ~ 32,767                      |            -            |
|   530   |     2      |       [Position I Gain](#position-pid-gain)       |   读写   |         0          |                     0 ~ 32,767                      |            -            |
|   532   |     2      |       [Position P Gain](#position-pid-gain)       |   读写   |        314         |                     0 ~ 32,767                      |            -            |
|   536   |     2      |   [Feedforward 2nd Gain](#feedforward-2nd-gain)   |   读写   |         0          |                     0 ~ 32,767                      |            -            |
|   538   |     2      |   [Feedforward 1st Gain](#feedforward-1st-gain)   |   读写   |         0          |                     0 ~ 32,767                      |            -            |
|   564   |     4      |          [Goal Position](#goal-position)          |   读写   |         -          | 最小位置限制（52）~最大位置限制（48） |        1[pulse]         |
|   568   |     2      |          [Realtime Tick](#realtime-tick)          |   读   |         -          |                     0 ~ 32,767                      |        1 [msec]         |
|   571   |     1      |          [Moving Status](#moving-status)          |   读   |         -          |                          -                          |            -            |
|   572   |     2      |            [Present PWM](#present-pwm)            |   读   |         -          |                          -                          |       0.0498 [%]        |
|   576   |     4      |       [Present Velocity](#present-velocity)       |   读   |         -          |                          -                          |     0.01 [rev/min]      |
|   580   |     4      |       [Present Position](#present-position)       |   读   |         -          |                          -                          |        1 [pulse]        |
|   584   |     4      |    [Velocity Trajectory](#velocity-trajectory)    |   读   |         -          |                          -                          |     0.01 [rev/min]      |
|   588   |     4      |    [Position Trajectory](#position-trajectory)    |   读   |         -          |                          -                          |        1 [pulse]        |
|   594   |     1      |    [Present Temperature](#present-temperature)    |   读   |         -          |                          -                          |       1 [&deg;C]        |
|   878   |     1      |           [Backup Ready](#backup-ready)           |   读   |         -          |                        0 ~ 1                        |            -            |