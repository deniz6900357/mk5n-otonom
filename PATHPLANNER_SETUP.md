# PathPlanner Otonom Yapılandırması

Bu proje artık PathPlanner ile tam otonom desteğine sahip.

## Yapılan Değişiklikler

### 1. PathPlanner Kütüphanesi Eklendi
- **Dosya**: `vendordeps/PathplannerLib.json`
- **Versiyon**: 2025.2.7

### 2. CommandSwerveDrivetrain - AutoBuilder Yapılandırması
- **Dosya**: `src/main/java/frc/robot/subsystems/CommandSwerveDrivetrain.java`
- **Eklenen Metodlar**:
  - `configurePathPlanner()`: AutoBuilder'ı yapılandırır
  - `getCurrentRobotChassisSpeeds()`: Mevcut robot hızlarını döndürür
  - `resetPoseForPath(Pose2d)`: PathPlanner için pose sıfırlama

- **PID Sabitleri** (İhtiyaca göre ayarlayın):
  - Translation PID: kP=5.0, kI=0.0, kD=0.0
  - Rotation PID: kP=5.0, kI=0.0, kD=0.0

### 3. RobotContainer - Auto Chooser
- **Dosya**: `src/main/java/frc/robot/RobotContainer.java`
- **Eklenenler**:
  - `SendableChooser<Command> autoChooser`: Otonom seçici
  - SmartDashboard'a "Auto Chooser" olarak eklendi
  - `getAutonomousCommand()`: Seçilen otonom komutunu döndürür

### 4. PathPlanner Dizin Yapısı
```
src/main/deploy/pathplanner/
├── autos/
│   └── Example Auto.auto       # Örnek otonom rutini
├── paths/
│   └── Example Path.path       # Örnek path
└── settings.json               # PathPlanner ayarları
```

## Kullanım

### PathPlanner GUI ile Path Oluşturma

1. **PathPlanner GUI'yi indirin**:
   - https://github.com/mjansen4857/pathplanner/releases

2. **Projeyi açın**:
   - PathPlanner GUI'de bu projeyi açın
   - `src/main/deploy/pathplanner/` dizinini kullanır

3. **Path oluşturun**:
   - "Paths" sekmesinden yeni path oluşturun
   - Waypoint'ler ekleyin ve path'i ayarlayın
   - `.path` dosyası olarak kaydedilir

4. **Auto oluşturun**:
   - "Autos" sekmesinden yeni auto oluşturun
   - Path'leri ve komutları sıralayın
   - `.auto` dosyası olarak kaydedilir

### Robot Kodunda Kullanım

Auto'lar otomatik olarak SmartDashboard'daki "Auto Chooser" dropdown'ında görünür.

#### Named Command Ekleme (Opsiyonel)
Auto'larda kullanmak için özel komutlar ekleyebilirsiniz:

```java
// RobotContainer constructor'ında
NamedCommands.registerCommand("intakeNote", intakeCommand);
NamedCommands.registerCommand("shootNote", shootCommand);
```

#### Programatik Auto Çalıştırma
```java
Command autoCommand = new PathPlannerAuto("Auto İsmi");
```

## Ayarlamalar

### Robot Boyutları
`src/main/deploy/pathplanner/settings.json` dosyasında:
- `robotWidth`: 0.7112m (28 inch)
- `robotLength`: 0.7112m (28 inch)

### Hız Limitleri
- `maxVelocity`: 3.0 m/s
- `maxAcceleration`: 3.0 m/s²
- `maxAngularVelocity`: 540 deg/s
- `maxAngularAcceleration`: 720 deg/s²

### PID Tuning
`CommandSwerveDrivetrain.configurePathPlanner()` metodunda:
```java
new PIDConstants(5.0, 0.0, 0.0), // Translation PID
new PIDConstants(5.0, 0.0, 0.0)  // Rotation PID
```

**Not**: Bu değerler başlangıç değerleridir. Robotunuzun davranışına göre ayarlamanız gerekebilir.

## Alliance Rengi
PathPlanner otomatik olarak Red Alliance için path'leri mirror eder. Origin her zaman Blue Alliance tarafında kalır.

## Sorun Giderme

### "Failed to load PathPlanner config" hatası
- Robot kodunun `src/main/deploy/pathplanner/settings.json` dosyasına erişebildiğinden emin olun
- RobotConfig GUI'den doğru yüklendiğinden emin olun

### Auto Chooser'da auto görünmüyor
- `src/main/deploy/pathplanner/autos/` klasöründe `.auto` dosyaları olduğundan emin olun
- Dosya formatının doğru olduğundan emin olun

### Path takip etmiyor / Kötü davranış
- PID sabitlerini ayarlayın
- Max hız ve ivme değerlerini düşürün
- Robot karakterizasyonunun doğru olduğundan emin olun
