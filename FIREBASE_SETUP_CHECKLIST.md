# Firebase Yapılandırma Kontrol Listesi

## ✅ Kod Tarafında Kontrol (Tamamlandı)

- [x] `google-services.json` dosyası eklendi
- [x] `firebase_options.dart` dosyası oluşturuldu
- [x] Firebase initialization kodu eklendi
- [x] Package name `com.company.whoboom` olarak güncellendi
- [x] Gradle yapılandırması tamamlandı
- [x] Firebase paketleri pubspec.yaml'da mevcut

## 🔍 Firebase Console'da Kontrol Edilmesi Gerekenler

### 1. Firebase Authentication Ayarları

**Adımlar:**
1. [Firebase Console](https://console.firebase.google.com/) → Projenizi seçin (`whoboom-b2c29`)
2. Sol menüden **"Authentication"** → **"Sign-in method"** sekmesine gidin
3. Şu sign-in methodlarının aktif olduğundan emin olun:

   - ✅ **Email/Password**: 
     - Etkin mi? → Evet olmalı
     - Email link (passwordless) → İsteğe bağlı
   
   - ⚠️ **Google Sign-In** (isteğe bağlı):
     - Etkin mi? → İleride aktif edilecek
     - Şu an kodda TODO olarak işaretli

**Kontrol:**
```
✅ Email/Password → Enabled olmalı
```

---

### 2. Firebase Realtime Database Kuralları

**Adımlar:**
1. Firebase Console → **"Realtime Database"** → **"Rules"** sekmesine gidin
2. Şu kuralları ayarlayın (geliştirme için):

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null",
    "users": {
      "$userId": {
        ".read": "$userId === auth.uid || true",
        ".write": "$userId === auth.uid || !data.exists()"
      }
    },
    "tweets": {
      ".read": "auth != null",
      ".write": "auth != null",
      "$tweetId": {
        ".read": true,
        ".write": "auth != null && (!data.exists() || data.child('userId').val() === auth.uid)"
      }
    },
    "chats": {
      "$conversationId": {
        ".read": "auth != null && (data.child('userId1').val() === auth.uid || data.child('userId2').val() === auth.uid)",
        ".write": "auth != null && (data.child('userId1').val() === auth.uid || data.child('userId2').val() === auth.uid)"
      }
    },
    "notifications": {
      "$userId": {
        ".read": "$userId === auth.uid",
        ".write": "auth != null"
      }
    },
    "followers": {
      ".read": "auth != null",
      ".write": "auth != null"
    },
    "following": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

**ÖNEMLİ:** Production için daha sıkı kurallar ayarlayın!

**Kontrol:**
```
✅ Rules sekmesinde kurallar ayarlandı mı?
✅ "Publish" butonuna tıklandı mı?
```

---

### 3. Firebase Storage Kuralları

**Adımlar:**
1. Firebase Console → **"Storage"** → **"Rules"** sekmesine gidin
2. Şu kuralları ayarlayın:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    // Kullanıcı profil resimleri
    match /users/{userId}/profile/{fileName} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Kullanıcı kapak resimleri
    match /users/{userId}/cover/{fileName} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Tweet görüntüleri
    match /tweets/{tweetId}/{fileName} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    
    // Chat görüntüleri
    match /chats/{conversationId}/{messageId}/{fileName} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

**Kontrol:**
```
✅ Storage Rules sekmesinde kurallar ayarlandı mı?
✅ "Publish" butonuna tıklandı mı?
```

---

### 4. Database Oluşturma

**Realtime Database:**
1. Firebase Console → **"Realtime Database"**
2. **"Create Database"** butonuna tıklayın (eğer yoksa)
3. **Location** seçin (örn: `europe-west1` veya size yakın bir bölge)
4. **Start in test mode** → Daha sonra Rules'ı güncelleyeceğiz

**Kontrol:**
```
✅ Realtime Database oluşturuldu mu?
✅ Database URL'i doğru mu?
```

---

## 🧪 Test Etme

### 1. Uygulamayı Çalıştırın:
```bash
flutter run
```

### 2. Test Senaryoları:

**Authentication Test:**
- [ ] Kayıt ol sayfasından yeni kullanıcı oluşturun
- [ ] Email doğrulama linki geldi mi?
- [ ] Giriş yap çalışıyor mu?

**Database Test:**
- [ ] Kullanıcı kaydı Firebase'de görünüyor mu?
- [ ] Tweet gönderebiliyor musunuz?
- [ ] Tweet'ler listeleniyor mu?

**Storage Test:**
- [ ] Profil resmi yüklenebiliyor mu? (TODO: Implement edilecek)

---

## ⚠️ Önemli Notlar

1. **Güvenlik Kuralları**: Şu anki kurallar geliştirme için. Production'a geçmeden önce mutlaka sıkılaştırın!

2. **Firebase Storage**: Storage paketi henüz `pubspec.yaml`'da yok. İhtiyaç duyduğunuzda ekleyin:
   ```yaml
   firebase_storage: ^12.1.0
   ```
   Ve sonra `flutter pub get` çalıştırın.

3. **Google Sign-In**: Şu an kodda TODO. İleride Firebase Console'da aktif edip kodu güncelleyeceğiz.

4. **Database Indexing**: Büyük veri setleri için database index'leri oluşturmanız gerekebilir.

---

## 📝 Hızlı Kontrol Listesi

- [ ] Firebase Console'da Authentication → Email/Password aktif mi?
- [ ] Realtime Database oluşturuldu mu ve kurallar ayarlandı mı?
- [ ] Storage oluşturuldu mu ve kurallar ayarlandı mı?
- [ ] Uygulama çalışıyor mu? (`flutter run`)
- [ ] Kayıt ol işlemi çalışıyor mu?
- [ ] Giriş yap işlemi çalışıyor mu?

---

## 🔗 Faydalı Linkler

- [Firebase Console](https://console.firebase.google.com/)
- [Firebase Authentication Dokümantasyonu](https://firebase.google.com/docs/auth)
- [Firebase Realtime Database Dokümantasyonu](https://firebase.google.com/docs/database)
- [Firebase Storage Dokümantasyonu](https://firebase.google.com/docs/storage)

