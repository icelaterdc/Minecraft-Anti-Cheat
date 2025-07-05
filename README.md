# IceLater - Minecraft Anti Cheat

A highly modular, comprehensive, and extensible Anti-Cheat plugin for Spigot/Paper Minecraft servers. Engineered to detect and prevent a wide variety of cheats—such as speed, fly, reach, auto-click, packet rate, and kill aura—and automatically ban or kick offenders once configurable thresholds are exceeded.

---

## 🚀 Features

* **Modular Check Framework**: Each cheat detection (Speed, Fly, Reach, NoFall, FastBreak, AutoClick, PacketRate, KillAura) is implemented as a separate `Check` class, making it easy to add, remove, or customize checks.
* **Dynamic Registration**: Automatically loads and registers only the checks enabled in `config.yml`.
* **Configurable Thresholds**: Fine-tune detection sensitivity per check (e.g., maximum distance, CPS, PPS, attack rate, etc.).
* **Violation Counting & Banning**: Maintains a cumulative violation count per player. Once the `ban-threshold` is reached, the plugin bans and kicks the offender with a customizable message.
* **Bypass Permission**: Grant `anticheat.bypass` to exempt trusted players from all checks.
* **Admin Commands**:

  * `/anticheat status` — List active checks and their settings.
  * `/anticheat reload` — Reloads configuration without restarting the server.
* **Lightweight & Performant**: Designed to minimize overhead by only listening to necessary events and using efficient data structures.

---

## ⚙️ Requirements

* **Java**: Java 17+
* **Server**: Spigot or Paper 1.20.x (compatible with Paper API 1.20.1-R0.1-SNAPSHOT)
* **Build Tool**: Maven

---

## 📥 Installation

1. **Build the Plugin**

   ```bash
   git clone https://github.com/icelaterdc/Minecraft-Anti-Cheat.git
   cd AdvancedAntiCheatPlus
   mvn clean package
   ```

   This produces `MinecraftAntiCheat-1.1.0.jar` in the `target/` directory.

2. **Deploy to Server**

   * Copy the JAR to your server’s `plugins/` folder.
   * Start or restart your server.

3. **Configuration**

   * On first run, a folder `plugins/MinecraftAntiCheat/` is created containing `config.yml`.
   * Edit `config.yml` to enable/disable checks and adjust thresholds.
   * Use `/anticheat reload` to apply changes without a full restart.

---

## 🛡️ Cheat Detection & Ban Conditions

Each detection check increments a player-specific violation counter when a rule is triggered. When the total count reaches the `ban-threshold`, the following occurs:

1. The player is added to the server’s ban list.
2. The player is immediately kicked with the `ban-message` defined in `config.yml`.

### Default Checks & Conditions

| Check Name      | Event Listened              | Trigger Condition                                   | Config Keys                               |
| --------------- | --------------------------- | --------------------------------------------------- | ----------------------------------------- |
| SpeedCheck      | `PlayerMoveEvent`           | Movement distance > `max-distance`                  | `max-distance`, `threshold`               |
| FlyCheck        | `PlayerMoveEvent`           | Vertical movement > `max-height`                    | `max-height`, `threshold`                 |
| ReachCheck      | `PlayerInteractEntityEvent` | Attack distance > `max-reach`                       | `max-reach`, `threshold`                  |
| NoFallCheck     | `EntityDamageEvent`         | Fall damage event                                   | `threshold`                               |
| FastBreakCheck  | `BlockDamageEvent`          | Time between block breaks < `max-speed`             | `max-speed`, `threshold`                  |
| AutoClickCheck  | `PlayerInteractEvent`       | Clicks per second > `max-cps`                       | `max-cps`, `threshold`                    |
| PacketRateCheck | `PlayerMoveEvent`           | Packets per second > `max-packets-per-second`       | `max-packets-per-second`                  |
| KillAuraCheck   | `EntityDamageByEntityEvent` | Attacks per window > `attack-threshold` (ms window) | `detection-window-ms`, `attack-threshold` |

> **Note**: Multiple detections across different checks accumulate toward the same ban threshold.

---

## 📝 Configuration (`config.yml`)

```yaml
checks:
  SpeedCheck:
    enabled: true
    max-distance: 1.2
    threshold: 5
  FlyCheck:
    enabled: true
    max-height: 1.5
    threshold: 3
  ReachCheck:
    enabled: true
    max-reach: 3.5
    threshold: 4
  NoFallCheck:
    enabled: true
    threshold: 3
  FastBreakCheck:
    enabled: true
    max-speed: 0.7
    threshold: 5
  AutoClickCheck:
    enabled: true
    max-cps: 12
    threshold: 20
  PacketRateCheck:
    enabled: true
    max-packets-per-second: 20
    threshold: 10
  KillAuraCheck:
    enabled: true
    detection-window-ms: 1000
    attack-threshold: 8

ban-on-violation: true
ban-threshold: 15
ban-message: "You have been banned for cheating! - AntiCheat"
```

* **enable**: Activate/deactivate each check.
* **threshold**: Number of infractions of that type before a violation is recorded.
* **global ban-threshold**: Total violations before ban.
* **ban-message**: Kick message shown upon ban.

---

## 🤝 Contributing

We welcome contributions and enhancements! To propose changes:

1. **Fork the repository**.
2. **Create a feature branch**:

   ```bash
   git checkout -b feature/YourFeatureName
   ```
3. **Commit your changes** with clear messages.
4. **Push to your fork**:

   ```bash
   git push origin feature/YourFeatureName
   ```
5. **Open a Pull Request** against `main` in the upstream repository.
6. Ensure your code is **formatted** and **documented**, and **passes existing tests** (if any).

---

## 📜 License

Distributed under the MIT License. See [LICENSE](LICENSE) for details.

---

#### Maintainers

* **IceLater** – Plugin architect, core development
* *Contributors* – Thanks to everyone who has contributed

For questions, issues, or support, please open an issue on GitHub.
