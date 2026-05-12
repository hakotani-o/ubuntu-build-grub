# ubuntu-build-grub (実験的プロジェクト)

Orange Pi 5 / 5 Plus で **U-Boot + GRUB** を使用して **Ubuntu 26.04** を起動させ
るための実験的なプロジェクトです。

> [!IMPORTANT]
> **「ほぼ純正」のUbuntuイメージを目指しました。**
> 中身は極力Ubuntu標準のパッケージ構成を維持しつつ、ブートプロセスのみをマニアッ
クに改造した「ロマン仕様」のイメージです。

## 🚀 「5分間」の挑戦 (The 5-Minute Challenge)
このビルドの最大の特徴は、電源を入れてから画面が反応するまでが「非常に長い」こと
です。
* **起動時間:** 電源ONからログイン画面まで **約4分〜5分**。
* **沈黙の4分間:** 電源を入れてから約4分間、ディスプレイには何も表示されません。
カーネルが起動し始めてようやく画面にログが流れ始めます。
* **なぜこんなに遅いのか？:**
  ほぼUbuntu純正の構成を維持したまま、RockchipプラットフォームでU-BootからGRUBを
ロードするという特殊な構成をとっているためです。実用性よりも「最新の構成で動くこ
と」を優先しました。

## ⚙️ システム構成と注意点
* **OS:** Ubuntu 26.04 (Development branch) **※ほぼ純正のディスクイメージです**
* **U-Boot:** 2026.04 (NVMe/SCSI対応) + grub.cfg リメイクスクリプト
  * devicetreeのパスを解決するカスタムスクリプトを同梱。
* **ブート方式:** ディスクイメージに U-Boot は含まれていません。
  * 別途、SDカード等に U-Boot を書き込んでブートさせる必要があります。
* **重要：SPLの初期化:**
  * 古いバージョンのSPL（mtd0等）が書き込まれているとブートしません。
  * 私は **SPLが消去（Erase）された状態** でテストを行いました。
* **デバッグ推奨:** トラブル解決には **TTL-シリアルコンバーター** が必須です。GRUBメニューはシリアルコンソールに出力されます。

## ✅ 動作確認済み (What Works)
* **対応ボード:** Orange Pi 5 / 5 Plus
* **ブートローダー:** U-Boot + GRUB によるカーネルロード
* **GUI (Gnome):** デスクトップ表示まで到達
* **アナログオーディオ:** オーディオジャック出力（私の環境では動作）
* **ストレージ:** 2.5inch SSD (M.2 -> JMB582 -> SATAコンバーター経由)
  * ディスクのリムーバブル環境（抜き差し可能な構成）を実現するためにこの構成を採
用しています。
  * ※NVMe(M.2)からの直接ブートは未確認ですが、U-Boot自体は対応しています。

## ⚠️ 既知の問題・あきらめたこと
* **USB 3.0 ポート:** 動作しません。USB 2.0のみ使用可能です。
* **ディスプレイ出力:** 起動プロセスの後半（4分後以降）まで何も映りません。

## 🌟 その後の追加情報
* **Linux Kernel Stable (7.0.y)** に変更すると **USB3.0ポート** が動作しました。
* **ubuntu-build** のカーネルを使用するとブート時間が約半分になります。



---
## ubuntu-build-grub (Experimental)
This repository focuses on the **U-Boot -> GRUB** boot sequence for **Orange Pi 5 and Plus**.

## 🌟 What's New & Key Discoveries
* USB 3.0 Support with Stable Kernel: We discovered that switching from the generic Ubuntu kernel to a custom-built Linux Kernel Stable (7.0.y) enables full functionality of USB 3.0 ports.

* 50% Faster Boot Time: By using our optimized custom kernel instead of the Ubuntu generic kernel, the boot time has been cut in half—now only 2 minutes and 20 seconds to the login screen.

* Filesystem Integrity: The build script has been improved to ensure a stable boot by organizing /boot assets correctly during the image assembly process.

## 🚀 Key Features
* U-Boot -> GRUB Chain: Implementing a standard Linux boot flow. This moves the boot process closer to the architecture used by PC and Server distributions.

* Serial Console Focused: Currently, the GRUB menu and boot logs are output via the Serial Console, making it a powerful setup for headless servers or deep technical debugging.

## ⚠️ Important Note
This is an experimental project. While the boot time is optimized to 2:20, it is intentionally slower than a direct U-Boot boot to allow for GRUB's flexibility and potential compatibility with generic GRUB-based tools.
* It will not boot if an older version of SPL (such as mtd0) is written.
* I tested with SPL erased.
* Requires external U-Boot.
* Direct booting from NVMe (M.2) is unconfirmed, but U-Boot itself is supported.

---

    
