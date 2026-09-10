# Nakit — Senkronize Kişisel Finans

Yeni GitHub deposu: https://github.com/kdrolmz/nakit-finans

## İki cihaz, aynı kayıtlar

GitHub Pages üzerindeki index.html özel uygulamayı açan bir giriş dosyasıdır. Uygulamanın sunucusu Cloudflare Workers / Sites üzerinde, kayıtlar D1 veritabanındadır. GitHub Pages tek başına özel bir veritabanı çalıştıramaz. iPhone ve MacBook'ta aynı ChatGPT hesabıyla giriş yapılmalıdır. Sites erişimi yalnızca sahibine açıktır; kullanıcı kimliği sunucuda doğrulanır. Finans kayıtları ve oturum bilgileri GitHub deposuna yüklenmez. Bu çözüm donanımsal iki-cihaz kilidi veya uçtan uca şifreleme iddiası taşımaz.

- Değişiklikler sunucuya kaydedilir; açık ekranda yaklaşık 5 saniyede yenilenir. Uygulamaya dönüldüğünde de kontrol edilir.
- “Senkronize” sunucu onayını ifade eder. Bağlantı hatasında bekleyen değişiklik cihazda geçici taslak olarak korunur; diğer cihazda görünmüş sayılmaz.
- Eşzamanlı farklı alan düzenlemeleri birleştirilir. Aynı alandaki çakışmalar sessizce üzerine yazılmaz; uyarı gösterilir. Önce yerel yedeği indir, sonra sunucudaki kayıtları açıp gerekli değişikliği tekrar uygula.
- İlk geçişte aynı site adresindeki eski, örnek olmayan yerel kayıtlar sunucu boşsa aktarılır. Başka dosya/adresteki kayıtlar için Ayarlar → Yedek içe aktar kullanılır. Eski yerel kayıt anahtarı silinmez.
- index.html eski yerel kayıt algılarsa yönlendirmeden önce yedek indirmeyi sunar. Senkronizasyon için çevrimiçi uygulamayı kullan; eski çevrimdışı HTML kopyalarında düzenlemeye devam etme.

## Kullanım

Özetin ilk kartında bu ay kalan borç, küçük alt satırda tüm vadelerin kalan kredi/kart/kişisel borcu görünür. “Aylık ödeme planı” seçili ayın planlanan gideridir. Birikimler ayrı kartta tutulur. Rapor sekmesi kaldırılmıştır.

Giderler sekmesinde tüm nakit ödemeleri gün sırasındadır. Ödeme günü 1–31 arasında seçilir; değişiklik yalnızca o ayı etkiler. Ödendi işareti kalan gideri ve ilgili kredi borcunu azaltır. Geri alınabilir.

BUSKİ, Uludağ Elektrik, Türk Telekom İnternet, Turkcell Hattım ve Türk Telekom Eşimin Hattı Ev giderleri seçicisindedir. Bu seçimlerde kredi kartı varsayılandır. Tutar, gün ve ödeme yapılan kartı seç. Ekstrenin tamamını Kredi kartları kategorisine gir: kartla ödenen faturalar ve abonelikler genel giderde ikinci kez sayılmaz. Fatura/abonelik kendi kategorisinde ayrıca görülebilir.

Birikimler Altın & Borsa, Bireysel Emeklilik ve Eminevim logolu Ev Finansmanı olarak ayrıdır. TUPRS kaynak verisi 15 dakika gecikmelidir; altın ve hisse fiyatları 60 saniyede bir kontrol edilir. Kaynak doğrulanamazsa kayıtlı değer korunur. BES ve ev finansmanı değerleri elle düzenlenir.

Safari'de güvenli uygulamayı açıp Paylaş → Ana Ekrana Ekle ile iPhone'da uygulama gibi kullanabilirsin. İnternet ve aynı hesaba giriş gereklidir. Yedek dosyaların finans bilgisi içerir; onları herkese açık depoya yükleme.

## Kaynak ve doğrulama

React / Vinext, D1, hesap başına kayıt ve sürüm kontrollü güncelleme kullanılır. Anonim API istekleri reddedilir; yazmalar aynı-origin JSON isteği gerektirir. Veritabanı sorguları parametrelidir.

Yerel geliştirme: npm install, ardından npm run dev. D1 şeması db/schema.ts, migration drizzle klasöründedir. Yayın giriş HTML dosyasını üretmek için node scripts/build-html.mjs /tam/yol/index.html kullanılır.
