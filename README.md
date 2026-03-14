# Ro-Kernel-Experimental

## 🇬🇧 English

### Why Ro-Kernel-Experimental exists?
This repository contains an experimental kernel optimization project based on Linux 7.0-rc3, specifically designed for Fedora 43. The primary goal is to provide a snappy, streamlined desktop experience for end-users. By stripping away unnecessary server-related components, obsolete hardware drivers (from before 1999), and dormant network protocols, the kernel is made significantly lighter. 

Despite the aggressive slimming, it maintains essential support for both modern and "legacy" desktop setups (including IDE/SATA and USB controllers required for Legacy BIOS) to ensure compatibility across a wide range of desktop hardware.

### Build Status
This kernel is automatically compiled and hosted on **Fedora COPR**.

---

## 🇹🇷 Türkçe

### Ro-Kernel-Experimental Nedir?
Bu depo, Fedora 43 tabanlı Linux 7.0-rc3 çekirdeği üzerine kurgulanmış deneysel bir optimizasyon projesidir. Temel amacı, son kullanıcılar için en hızlı ve verimli masaüstü deneyimini sunmaktır. Sunucu odaklı bileşenler, 1999 öncesi eski donanım sürücüleri ve kullanılmayan ağ protokolleri temizlenerek çekirdek ciddi oranda hafifletilmiştir.

Çekirdek hafifletilirken; modern sistemlerin yanı sıra "Legacy" (Eski) BIOS sistemlerin de sorunsuz açılabilmesi için gerekli olan temel IDE/SATA ve USB denetleyici desteği korunmuştur.

### Derleme Bilgisi
Bu çekirdek, **Fedora COPR** platformu üzerinde otomatik olarak derlenmekte ve barındırılmaktadır.

---

## 🚀 Installation Guide / Kurulum Rehberi (Fedora 43)

### 🇬🇧 English: How to install from COPR
You can easily install the kernel on your Fedora 43 system using the following steps:

1. **Enable the COPR repository:**
   ```bash
   sudo dnf copr enable minitheguitarist/Ro-Kernel-Experimental
   ```

2. **Update and Install the Kernel:**
   ```bash
   sudo dnf update
   sudo dnf install kernel kernel-devel kernel-headers
   ```

3. **Reboot:**
   After the installation is complete, reboot your computer. You should see the **-RoASD** version in your GRUB menu.

---

### 🇹🇷 Türkçe: COPR üzerinden nasıl kurulur?
Fedora 43 sisteminize bu çekirdeği aşağıdaki adımları izleyerek kolayca kurabilirsiniz:

1. **COPR deposunu etkinleştirin:**
   ```bash
   sudo dnf copr enable minitheguitarist/Ro-Kernel-Experimental
   ```

2. **Sistemi Güncelleyin ve Kerneli Kurun:**
   ```bash
   sudo dnf update
   sudo dnf install kernel kernel-devel kernel-headers
   ```

3. **Yeniden Başlatın:**
   Kurulum tamamlandıktan sonra bilgisayarınızı yeniden başlatın. GRUB menüsünde **-RoASD** takısını içeren çekirdek sürümünü göreceksiniz.



**Ro-Main testi yapıldı**