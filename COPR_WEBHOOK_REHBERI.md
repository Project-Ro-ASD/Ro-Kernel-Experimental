# Fedora COPR & GitHub Webhook Entegrasyon Rehberi

Bu rehber, GitHub reponuza (Project-Ro-ASD/Ro-Kernel-Experimental) her kod yüklediğinizde COPR'ın otomatik olarak derleme yapmasını ve derleme bittiğinde GitHub Releases üzerinden RPM'lerin otomatik yayımlanmasını adım adım anlatır.

---

## 1. Aşama: GitHub'dan COPR'a (Kod İtilince Derleme Başlatma)
GitHub üzerinde kodda bir değişiklik yaptığınızda COPR'ın bunu görüp derlemeye başlaması için şu adımları izleyin:

1. **COPR'da Webhook URL'nizi bulun:**
   - Fedora COPR hesabınıza giriş yapın ve projenize (`Ro-Kernel-S` vb.) gidin.
   - **Settings** (Ayarlar) sekmesine, ardından yan menüden **Integrations** sayfasına tıklayın.
   - Burada "Webhook URL" bilgisini göreceksiniz. (Örn: `https://copr.fedorainfracloud.org/webhooks/custom/...`) Bu adresi kopyalayın.

2. **GitHub'a Webhook'u Ekleyin:**
   - [Projenizin GitHub sayfasına](https://github.com/Project-Ro-ASD/Ro-Kernel-Experimental) gidin.
   - **Settings** > **Webhooks** > **Add webhook** yolunu izleyin.
   - **Payload URL:** Kopyaladığınız COPR adresini yapıştırın.
   - **Content type:** `application/json` olarak seçin.
   - **Which events would you like to trigger this webhook?** "Just the push event." seçeneğini işaretleyin ve **Add webhook** butonuna basarak kaydedin.

Artık GitHub'a yaptığınız her "*push*" işlemi sonrası COPR otomatik olarak yeni bir paket derleme kuyruğa alacaktır!

---

## 2. Aşama: COPR'dan GitHub'a (Derleme Bitince Otomatik Release Dağıtma)
COPR derlemesi bittikten sonra RPM paketlerini alıp GitHub sayfanıza `Release` olarak yükleyen bir otomasyon (`.github/workflows/copr_release.yml`) zaten deponuzda hazır bulunmaktadır. 

**Ancak küçük bir zorluk var:** COPR, işi bittiğinde doğrudan GitHub'ın `repository_dispatch` ucu ile konuşamaz çünkü GitHub "Kişisel Erişim Tokeni" (`Authorization` header) istemektedir. COPR ise standart, şifresiz bir "POST" isteği atar.

Bu süreci tamamen otomatik hale getirmek için **iki seçeneğiniz vardır**:

### Seçenek A: Zamanlanmış (Cron) Yoklama Yöntemi (Önerilen)
Webhooks ile uğraşmak yerine `.github/workflows/copr_release.yml` dosyanızı her gün (örneğin gece yarısı) veya her çalışmanızı takip eden belirli saatlerde otomatik çalışması için ayarlayabilirsiniz (YAML dosyasına eklendi). 

### Seçenek B: Aracı Bir Servis (Pipedream vb.) Kullanmak (Tam Webhook)
COPR'ın attığı isteği GitHub'a token ile iletmek isterseniz [Pipedream.com](https://pipedream.com/) gibi bir aracı servis kurabilirsiniz:
1. Pipedream'de yeni bir HTTP Webhook oluşturun (Size bir URL verir).
2. Bu URL'yi **COPR** projenizin `Settings` altındaki `Webhook` kısmına yapılandırın.
3. Pipedream'de gelen isteği GitHub'ın `https://api.github.com/repos/Project-Ro-ASD/Ro-Kernel-Experimental/dispatches` adresine `POST` atacak şekilde ayarlayın. İsteğin "Headers" kısmına:
   - `Accept: application/vnd.github.v3+json`
   - `Authorization: Bearer <GITHUB_PERSONAL_ACCESS_TOKEN>` ekleyin.
4. İsteğin "Body" (JSON) kısmına `{"event_type": "copr_build_complete"}` mesajını yerleştirin.

---

## 3. Aşama: Sistemi Manuel (Elle) Test Etme
Özellikle ilk kurulumları yaptığınızda RPM toplama işleminin düzgün çalıştığını görmek için GitHub'dan manuel olarak tetikleyebilirsiniz:

1. Reponuzda üst menüden **Actions** kısmına tıklayın.
2. Soldan **COPR to GitHub Releases** (veya yml adınız) işini seçin.
3. Sağ üstte yer alan **Run workflow** tuşuna basarak otomasyonu kendiniz çalıştırın.
4. Bitmesini bekleyin ve ardından **Releases** sekmesine bakarak RPM dosyalarınızın yüklenip yüklenmediğini teyit edin!
