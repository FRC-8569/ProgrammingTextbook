API 介紹
==============

對很神奇吧，幹話完之後第一篇不是教你怎麼寫Java是直接進到API介紹，超爽(?)

.. tip::
    基本上單字(尤其是類別)不用全部打完，打開頭幾個字(或是一半)就可以直接按Tab或是Enter鍵讓他自動選(而且如果是自定義類別(eg. REVLib的東西))會自動引用(超爽)

REV系列
-----------

.. important::
    VendorDep: REVLib

SparkMax Motor Controller適用於NEO v1.1(`21-1650 <https://www.revrobotics.com/rev-21-1650/>`_)與NEO 550(`21-1651 <https://www.revrobotics.com/rev-21-1650/>`_)與所有的有刷馬達

.. code-block:: java
    
    import com.revrobotics.spark.SparkMax;  
    pulic SparkMax motor = new SparkMax(CanID);

----

SparkFlex Motor Controller適用於NEO Vortex(`21-1652 <https://www.revrobotics.com/rev-21-1652/>`_)

.. code-block:: java

    import com.revrobotics.spark.SparkFlex;  
    public SparkFlex motor = new SparkFlex(CanID);

顏色感測器(Color Sensor V3)(沒想像中好用)

.. note::
    基本上Color Sensor都插一般的I2C(因為MXP有機會要裝Gyro)

.. code-block:: java

    import edu.wpi.first.wpilibj.I2C.Port;
    public ColorSensorV3 sensor = new ColorSensorV3(Port.kOnboard);

軸編碼器(Through Bore Encoder)

.. attention::
    這個鬼東西還要搭配Adapter Board才可以用(還有分絕對跟相對的要注意看)

.. note::
    不同於CANCoder，他要跟著SparkMax才有辦法召喚出來

.. code-block:: java

    import com.revrobotics.spark.SparkMax;
    import com.revrobotics.RelativeEncoder;
    import com.revrobotics.AbsoluteEncoder;

    public SparkMax motor = new SparkMax(CanID);
    public RelativeEncoder encoder = motor.getAlternativeEncoder(); //相對模式
    public AbsoluteEncoder encoder = motor.getAbsoulteEncoder(); //絕對模式

CTRE系列
-----------

.. important::
    VendorDeps: Phoenix 5, Phoenix 6

TalonSRX 純有刷馬達控制器(Phoenix 5)

.. code-block::

    import com.ctre.phoenix.motorcontrol.can.TalonSRX;
    public TalonSRX motor = new TalonSRX(CanID);

TalonFX Kraken x44, x60, Falcon內建的控制器(Phoenix 6)

.. note::
    因為TalonFX控制器支援CANivore，所以可以選用哪個CANBus(不過預設就是RoboRIO出來的CANBus)

.. code-block::

    import com.ctre.phoenix6.hardware.TalonFX;
    public TalonFX motor = new TalonFX(CANID);
    public TalonFX motor = new TalonFX(int CANID, String CanBusName);
    public TalonFX motor= new TalonFX(int CANID, CANBus canbus); //recommend this instead 2nd one.

剩下的都差不多自己去研究

.. note::
    其實還有另外一種馬達控制器VictorSPX(VEX出的)，但是他現在在比賽場上比在三類組的女生還有日本的壓縮機還要稀少，所以就不特別細講。

馬達控制器的設定
---------------------

.. tip::
    通常馬達在弄新設定之前都先Reset Factory Default比較不容易出事

SparkMax
++++++++

.. code-block:: java

    public SparkMax motor = new SparkMax(0, MotorType.kBrushless);
    public SparkMaxConfig config = new SparkMaxConfig();

    //(某個inital的函數裡面)
    config
        .idleMode(IdleMode.kBrake)
        .inverted(false);
    config.encoder
        .positionConversionFactor(1)
        .velocityConvertionFactor(1/60);

    //還有剩下的自己摸

    motor.configure(config, ResetMode.kResetSafeParameters, PersistMode.kPersistParameters); //kResetSafeParameters是要不要回復出廠設置，kPersistParameters是要不要把目前的設置Apply到控制器裡面

SparkFlex
+++++++++

跟SparkMax同樣概念

CTRE類
++++++++

.. note::
    用TalonFX舉例

.. code-block:: java

    public TalonFX motor = new TalonFX(0);
    public TalonFXConfiguration configuration = new TalonFXConfiguration(); //這樣就會弄成出廠設定了


    configuration.MotorOutput
      .withNeutralMode(NeutralModeValue.Brake)
      .withInverted(InvertedValue.CounterClockwise_Positive);
    
    motor.getConfigurator().apply(configuration); //把設定設定到馬達上

剩下的東西都差不多意思

.. note::
    其他東西如果要另外設定的話都會先講