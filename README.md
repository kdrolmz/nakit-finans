# Nakit — Bağımsız Kişisel Finans

Uygulama: https://nakit-finans.kdrolmz.workers.dev/
GitHub giriş adresi: https://kdrolmz.github.io/nakit-finans/

## Giriş ve iki cihaz

Bu sürüm ChatGPT girişini kullanmaz. Uygulama ve veritabanı sahibinin kendi Cloudflare hesabındadır. Geçiş anahtarı (WebAuthn) ile giriş yapılır. Destekleyen cihazlarda Face ID, Touch ID veya cihaz doğrulaması kullanılır.

İlk hesap yalnızca özel kurulum koduyla etkinleştirilir. Kurulum kodu bu depoda yoktur. İlk kayıt sonrasında yeniden hesap açma kapanır. Gösterilen kurtarma kodunu güvenli yerde saklayın; GitHub'a yüklemeyin. Kurtarma kodunun kullanılması eski geçiş anahtarlarını ve açık oturumları iptal eder, yeni kod üretir; finans kayıtları korunur.

iPhone ve MacBook'ta aynı geçiş anahtarını kullanın. Apple hesabınızın Parolalar / iCloud Anahtar Zinciri senkronizasyonu açıksa desteklenen cihazlara anahtar aktarılabilir. Başka bir anahtar eklemek için girişten sonraki 10 dakika içinde Ayarlar → Giriş & güvenlik bölümünü kullanın.

Oturum bu tarayıcıda en fazla 30 gün saklanır. Safari çerezleri/site verileri silinirse yeniden kimlik doğrulaması gerekir; sunucuya kaydedilen kayıtlar silinmez. Ana ekrana eklemek bu kuralı ortadan kaldırmaz. Bekleyen, henüz sunucuya ulaşmamış değişiklikler cihaz verileri silinince kaybolabilir.

## Senkronizasyon

- Ana kayıt özel D1 veritabanındadır. Açık ekranda 5 saniyede bir ve uygulamaya dönünce yenilenir.
- “Senkronize” sunucunun kaydı onayladığı anlamına gelir.
- Farklı alanlara yapılan eşzamanlı değişiklikler birleştirilir. Aynı alandaki çakışmalar sessizce üzerine yazılmaz.
- Bağlantı hatasında bekleyen değişiklik cihazda taslak olarak korunur. Senkronize olmadan tarayıcı verilerini silmeyin.
- Eski Sites / yerel HTML sürümü ayrı kalır. Oradan yedek indirip yeni uygulamada Ayarlar → Yedek içe aktar kullanın. Sonrasında yalnızca yeni adresi kullanın.

## Finans takibi

Özetin ilk kartı bu ay kalan borcu, alt satırı tüm vadelerin kalan borcunu gösterir. Giderler gün sırasındadır; ödeme işareti ilgili ayın kalan giderini ve kredi borcunu azaltır, geri alınabilir. Rapor sekmesi yoktur.

BUSKİ, Uludağ Elektrik, Türk Telekom İnternet, Turkcell Hattım ve Türk Telekom Eşimin Hattı logolu seçimlerdir. Kredi kartıyla ödenen fatura ve abonelikler kategori içinde görünür ancak genel giderde ekstreyle birlikte iki kez sayılmaz. Kredi kartı ekstresinin toplamını ayrıca girin.

Birikimler Altın & Borsa, Bireysel Emeklilik ve Ev Finansmanı olarak ayrıdır. TUPRS verisi 15 dakika gecikmelidir; altın/hisse kaynakları 60 saniyede bir kontrol edilir. Kaynak doğrulanamazsa kayıtlı değer korunur. Bu uygulama bankaya bağlanmaz, ödeme veya yatırım işlemi yapmaz.

## Gizlilik sınırı

Finans kayıtları, kurulum/kurtarma kodları ve oturum belirteçleri GitHub'a yüklenmez. Sunucu sadece özetlenmiş oturum belirteçlerini ve anahtarların açık kısmını saklar. Tüm API yazmaları aynı kaynak kontrolünden geçer. Kullanıcı doğrulaması, tek kullanımlık WebAuthn meydan okumaları, istek sınırı, güvenli HttpOnly çerez ve sürüm kontrollü güncelleme uygulanır.

Bu sürüm UÇTAN UCA ŞİFRELİ DEĞİLDİR. Cloudflare altyapısı ve yetkili hesap yöneticilerinin veriye teknik erişiminin olmadığı garantisi verilmez. Banka parolası, kart numarası ve CVV girmeyin. Uygulama kodu finans kayıtlarını bir yapay zekâ modeline göndermez.

## Geliştirici notları

Mevcut React arayüzü korunmuştur. Bağımsız sürüm `independent/` altındaki Vite istemcisi + Cloudflare Worker ile çalışır. Eski Sites bildirimi eski yayına aittir; bağımsız yayını değiştirmez.

```sh
npm ci
npm run build:independent
npx wrangler d1 migrations apply nakit-finans-private --remote --config independent/wrangler.jsonc
npm run deploy:independent
```

Başka bir hesapta kurarken Cloudflare hesap/veritabanı kimlikleri ve APP_ORIGIN değiştirilmelidir. SETUP_HASH, 32 rastgele bayttan oluşan base64url kurulum kodunun SHA-256 base64url özetidir; Wrangler secret olarak ayarlanır. Ham kurulum kodunu depoya eklemeyin. Geçiş anahtarları alan adına bağlıdır; etkinleştirmeden sonra alan adını değiştirmek yeniden anahtar kaydı gerektirir.

`scripts/independent.test.mjs`, yalnızca boş yerel test veritabanında çalıştırılır. Gerçek imzalı yazılımsal WebAuthn test anahtarıyla kayıt/giriş, sahte kaynak ve tekrar saldırılarının reddi, iki oturumla kayıt senkronizasyonu, çerez temizliği, kurtarma ve çıkış kontrollerini yapar. Fiziksel iPhone/MacBook doğrulamasının yerine geçmez.
