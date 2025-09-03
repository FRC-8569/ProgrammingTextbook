KOP底盤教學
===============

一開始就不要太難，讓KOP底盤動起來吧！

.. admonition:: 任務獲得
    :class: important

    讓KOP底盤動起來

.. note::
    因為KOP底盤(Differential Chassis, 差速底盤)比較簡單，所以就用Timed Based來寫

事前準備
-----------

✅ 會動的NEO KOP底盤

範例程式
-----------

.. code-block:: java
    :caption: Robot.java

    package frc.robot;

    import com.revrobotics.RelativeEncoder;
    import com.revrobotics.spark.SparkMax;
    import com.revrobotics.spark.SparkBase.PersistMode;
    import com.revrobotics.spark.SparkBase.ResetMode;
    import com.revrobotics.spark.SparkLowLevel.MotorType;
    import com.revrobotics.spark.config.SparkMaxConfig;
    import com.revrobotics.spark.config.SparkBaseConfig.IdleMode;

    import edu.wpi.first.wpilibj.Joystick;
    import edu.wpi.first.wpilibj.TimedRobot;
    import edu.wpi.first.wpilibj2.command.CommandScheduler;
    import frc.robot.drivetrain.Constants;

    public class Robot extends TimedRobot{
        public SparkMax LeftMotor, RightMotor;
        public RelativeEncoder LeftEncoder, RightEncoder;
        private SparkMaxConfig LeftConfig, RightConfig;
        public Joystick joystick;

        public Robot(){
            LeftMotor = new SparkMax(Constants.LeftDrive, MotorType.kBrushless);
            RightMotor = new SparkMax(Constants.RightDrive, MotorType.kBrushless);
            LeftEncoder = LeftMotor.getEncoder();
            RightEncoder = RightMotor.getEncoder();
            joystick = new Joystick(0);

            LeftConfig = new SparkMaxConfig();
            RightConfig = new SparkMaxConfig();

            LeftConfig
                .idleMode(IdleMode.kBrake)
                .inverted(false);
            RightConfig
                .idleMode(IdleMode.kBrake)
                .inverted(true);

            LeftMotor.configure(LeftConfig, ResetMode.kResetSafeParameters, PersistMode.kPersistParameters);
            RightMotor.configure(RightConfig, ResetMode.kResetSafeParameters, PersistMode.kPersistParameters);
        }

        @Override
        public void robotPeriodic() {}

        @Override
        public void disabledInit() {}

        @Override
        public void disabledPeriodic() {}

        @Override
        public void disabledExit() {}

        @Override
        public void autonomousInit() {}

        @Override
        public void autonomousPeriodic() {}

        @Override
        public void autonomousExit() {}

        @Override
        public void teleopInit() {
            
        }

        @Override
        public void teleopPeriodic() {
            double vy = joystick.getRawAxis(1);
            double omega = joystick.getRawAxis(0);

            LeftMotor.set(vy+omega);
            RightMotor.set(vy-omega);
        }

        @Override
        public void teleopExit() {}

        @Override
        public void testInit() {}

        @Override
        public void testPeriodic() {}

        @Override
        public void testExit() {}
    }

.. attention:: 
    這邊因為vy±omega永遠會在[-1,1]裡面跳，所以可以直接用，如果會超過(例如不是在同一個蘑菇頭裡面)就要用相對進階的方式去把它縮放回來，在這邊讓各位自己去研究(絕對不是因為我太懶)