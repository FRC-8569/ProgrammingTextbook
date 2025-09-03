程式的種類
=============

在開始講程式的種類之前，我們要先知道FRC有三種主要的控制模式：TeleOp, Auto, Disabled分別代表手動，自動跟停機(還有另外一個Test不過基本不會用到所以在這邊不講)

每個模式都會有三個函數支撐：Init, Periodic跟Exit,分別代表初始化，中間要做的事跟收尾。(但是通常Disabled不會有動作所以disabledPeriodic都是空的)

Timed Based
-----------

Timded Based是FRC中最簡單的程式架構，基本上可以把所有的東西壓在一個檔案裡面，但是當程式比較大(例如整機程式或是Swerve的程式)的時候可讀性跟可維護性會大幅降低，所以基本上Timed Based只會在不見測試跟簡單的程式的時候會用。


.. code-block:: java
    :caption: Robot.java (Timed Based)

    package frc.robot;

    import edu.wpi.first.wpilibj.TimedRobot;

    public class Robot extends TimedRobot {

    ...

    public Robot() {} //機器的初始化

    @Override
    public void robotPeriodic() {} //機器要做的事(會無視Disabled)(通常是跑CommandScheduler或是SmartDashboard的更新)

    @Override
    public void autonomousInit() {} //自動初始化

    @Override
    public void autonomousPeriodic() {} //自動要做的事

    @Override
    public void autonomousExit(){} //自動的收尾

    @Override
    public void teleopInit() {} //手動初始化

    @Override
    public void teleopPeriodic() {} //手動要做的事

    @Override
    public void teleopExit(){} //自動的收尾

    @Override
    public void disabledInit() {} //停機初始化

    @Override
    public void disabledPeriodic() {} //停機要做的事(通常為空)

    @Override
    public void testInit() {}

    @Override
    public void testPeriodic() {}

    @Override
    public void simulationInit() {}

    @Override
    public void simulationPeriodic() {}
    }

基本上就是這樣

.. note::
    Exit類函數預設不會有要自己打

Command Based
----------------

Command Based就相對來說比較複雜（但是可塑性也比較高）通常會有這些檔案

.. code-block:: text
    :caption: CommandBased.tree
    
    .
    ├── Main.java
    ├── Robot.java
    ├── RobotContainer.java
    ├── Drivetrain/
    │   ├── SwerveModules.java
    │   ├── Drivetrain.java
    │   └── Constants.java
    ├── Elevator/
    │   ├── Constants.java
    └── └── Elevator.java
    ...

等等，而RobotContainer.java通常會像是這樣

.. code-block:: java
    :caption: RobotContainer.java

    package frc.robot;

    public class RobotContainer {
       
        //通常是所有的子系統(subsystem)跟控制(joystick)等等都會放在這邊順便就初始化了

        public RobotContainer() {
            // 其他initial的程式碼
            configureBindings();
        }

        public void configureBindings() {
           //要做的其他事(例如Trigger的按鈕等等)
        }

        //Auto的指令(用PathPlanner畫出來的)
        public Command getAutonomousCommand() {
            return Commands.println("No Fucking Auto");
        }
    }

然後Robot.java就大概不用去動它了(除非要更新SmartDashboard)不然就甭管他。
剩下RobotContainer要怎麼寫會在之後的文章提到