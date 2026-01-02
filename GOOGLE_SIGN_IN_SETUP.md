# Google Sign In Yapılandırması - Flutter Projesi

## ✅ ÖNEMLİ: Flutter vs Cordova

**Flutter projeleri için Firebase Dynamic Links GEREKLİ DEĞİLDİR!**

- ❌ Flutter projeleri Cordova değildir
- ✅ Flutter native paketler kullanır (`google_sign_in`)
- ✅ Firebase Dynamic Links'e ihtiyaç yok
- ✅ OAuth akışı native olarak yönetilir

---

## 🔧 Google Sign In Aktif Etme

### 1. Kod Tarafında (Şimdi Yapılacak)

Kod zaten hazır, sadece aktif edilecek.

### 2. Firebase Console Ayarları

**Adımlar:**
1. [Firebase Console](https://console.firebase.google.com/) → Projeniz
2. **Authentication** → **Sign-in method**
3. **Google** satırını bulun
4. **Enable** toggle'ını açın
5. **Project support email** seçin veya girin
6. **Save** butonuna tıklayın

**Kontrol:**
```
✅ Google → Enabled olmalı
```

### 3. Android Yapılandırması

Android için `google-services.json` zaten mevcut ve doğru yapılandırılmış.

**SHA-1 Fingerprint (Production için):**
- Release build için SHA-1 fingerprint eklenmeli
- Firebase Console → Project Settings → Your apps → Android app
- "Add fingerprint" butonuna tıklayın

**Debug için SHA-1 alma:**
```bash
# Windows
keytool -list -v -keystore "%USERPROFILE%\.android\debug.keystore" -alias androiddebugkey -storepass android -keypass android

# macOS/Linux
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

### 4. iOS Yapılandırması

iOS için `GoogleService-Info.plist` zaten mevcut.

**URL Scheme ekleme:**
- `Info.plist` dosyasına `REVERSED_CLIENT_ID` URL scheme eklenmeli
- Xcode'da otomatik eklenir veya manuel eklenebilir

---

## 📝 Kod Aktif Etme

Google Sign In kodu zaten hazır, şimdi aktif edilecek.

