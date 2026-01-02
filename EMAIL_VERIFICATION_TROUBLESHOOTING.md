# Email Doğrulama Sorun Giderme Rehberi

## 🔍 Sorun: Email Doğrulama Linki Gelmiyor

### ✅ Kod Tarafında Düzeltme (Yapıldı)

Kayıt işleminden sonra email doğrulama linki otomatik olarak gönderilecek şekilde kod güncellendi.

---

## 🔧 Firebase Console Kontrolleri

### 1. Authentication → Sign-in method

**Adımlar:**
1. [Firebase Console](https://console.firebase.google.com/) → Projeniz (`whoboom-b2c29`)
2. Sol menüden **Authentication** → **Sign-in method** sekmesi
3. **Email/Password** satırını bulun
4. **Enable** toggle'ının **AÇIK** olduğundan emin olun
5. **Email link (passwordless sign-in)** → İsteğe bağlı, şu an gerekli değil
6. **Save** butonuna tıklayın

**Kontrol:**
```
✅ Email/Password → Enabled olmalı
```

---

### 2. Authentication → Templates

**Email Doğrulama Template'i Kontrolü:**

1. Firebase Console → **Authentication** → **Templates** sekmesi
2. **Email address verification** template'ini bulun
3. Kontrol edin:
   - ✅ Template aktif mi?
   - ✅ Email gönderen adres doğru mu? (`noreply@whoboom-b2c29.firebaseapp.com`)
   - ✅ Action URL doğru mu?

**Özelleştirme (İsteğe bağlı):**
- Email başlığını özelleştirebilirsiniz
- Email içeriğini özelleştirebilirsiniz
- Logo ekleyebilirsiniz

---

### 3. Firebase Console → Project Settings → General

**Authorized domains kontrolü:**

1. Firebase Console → ⚙️ **Project Settings** → **General** sekmesi
2. **Authorized domains** bölümünü bulun
3. Şu domain'lerin ekli olduğundan emin olun:
   - ✅ `whoboom-b2c29.firebaseapp.com` (otomatik eklenir)
   - ✅ `localhost` (geliştirme için)
   - ✅ Kendi domain'iniz (varsa)

---

### 4. Spam Klasörü Kontrolü

Email doğrulama linki bazen spam klasörüne düşebilir:

- ✅ Gelen kutusunu kontrol edin
- ✅ Spam/Junk klasörünü kontrol edin
- ✅ Promotions klasörünü kontrol edin (Gmail)

---

### 5. Email Adresi Kontrolü

- ✅ Email adresini doğru yazdığınızdan emin olun
- ✅ Email adresinde yazım hatası var mı kontrol edin
- ✅ Farklı bir email adresi ile deneyin

---

## 🧪 Test Senaryoları

### Test 1: Kayıt Ol ve Email Kontrol Et

1. Uygulamayı çalıştırın: `flutter run`
2. "Kayıt Ol" sayfasından yeni hesap oluşturun
3. Email adresinizi kontrol edin
4. Firebase Console → Authentication → Users'da kullanıcıyı kontrol edin
   - Email verified: `false` olmalı
   - Email: Doğru email adresi görünmeli

### Test 2: Email Doğrulama Linkini Tekrar Gönder

1. Uygulamada "Email Doğrulama" sayfasına gidin
2. "Doğrulama Linkini Tekrar Gönder" butonuna tıklayın
3. Email adresinizi tekrar kontrol edin

### Test 3: Firebase Console'dan Manuel Gönder

1. Firebase Console → Authentication → Users
2. Kullanıcıyı bulun ve tıklayın
3. **Send email verification** butonuna tıklayın
4. Email adresinizi kontrol edin

---

## 🐛 Yaygın Sorunlar ve Çözümleri

### Sorun 1: Email hiç gelmiyor

**Olası Nedenler:**
- ❌ Email/Password authentication etkin değil
- ❌ Email adresi yanlış yazılmış
- ❌ Firebase Console'da email gönderimi engellenmiş

**Çözüm:**
1. Firebase Console → Authentication → Sign-in method → Email/Password → Enable
2. Email adresini kontrol edin
3. Firebase Console → Authentication → Users → Kullanıcıyı seçin → Send email verification

---

### Sorun 2: Email geliyor ama link çalışmıyor

**Olası Nedenler:**
- ❌ Authorized domains eksik
- ❌ Email template'inde yanlış URL

**Çözüm:**
1. Firebase Console → Project Settings → Authorized domains kontrol edin
2. Authentication → Templates → Email address verification → Action URL kontrol edin

---

### Sorun 3: Email spam klasörüne düşüyor

**Çözüm:**
- Email gönderen adresi: `noreply@whoboom-b2c29.firebaseapp.com`
- Bu adresi email sağlayıcınızda "güvenli gönderen" olarak işaretleyin
- Custom domain kullanarak daha güvenilir görünebilir (ileride)

---

## 📋 Kontrol Listesi

- [ ] Firebase Console → Authentication → Sign-in method → Email/Password → **Enabled**
- [ ] Firebase Console → Authentication → Templates → Email address verification → **Aktif**
- [ ] Firebase Console → Project Settings → Authorized domains → **Doğru domain'ler ekli**
- [ ] Email adresi doğru yazılmış
- [ ] Spam klasörü kontrol edildi
- [ ] Uygulamada kayıt işlemi başarılı
- [ ] Firebase Console → Authentication → Users'da kullanıcı görünüyor
- [ ] Email doğrulama linki gönderildi (Console'da kontrol edilebilir)

---

## 🔗 Firebase Console Hızlı Erişim

- [Authentication Settings](https://console.firebase.google.com/project/whoboom-b2c29/authentication/providers)
- [Email Templates](https://console.firebase.google.com/project/whoboom-b2c29/authentication/emails)
- [Users List](https://console.firebase.google.com/project/whoboom-b2c29/authentication/users)
- [Project Settings](https://console.firebase.google.com/project/whoboom-b2c29/settings/general)

---

## 💡 İpuçları

1. **Geliştirme Aşamasında**: Email doğrulama linkini Firebase Console'dan manuel olarak gönderebilirsiniz

2. **Test Email Adresi**: Geliştirme için gerçek bir email adresi kullanın (test@example.com gibi geçersiz adresler çalışmaz)

3. **Email Gecikmesi**: Bazen email'ler 1-2 dakika gecikebilir, bekleyin

4. **Rate Limiting**: Çok fazla email gönderirseniz Firebase rate limit uygulayabilir, birkaç dakika bekleyin

---

## ✅ Kod Güncellemesi

Kod tarafında düzeltme yapıldı:
- ✅ Kayıt işleminden sonra email doğrulama linki otomatik gönderilecek
- ✅ `signUpWithEmail` metodunda `sendEmailVerification()` çağrısı eklendi

Artık kayıt olduğunuzda email doğrulama linki otomatik olarak gönderilecek!

