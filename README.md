# 🔐 GateWay - Modern License Management System

Modern ve güvenli lisans yönetim sistemi. Yazılım ürünlerinizi koruyun ve lisanslarınızı kolayca yönetin.

## ✨ Özellikler

- 🎯 **Basit ve Güvenli API** - Kolay entegrasyon için optimize edilmiş
- 📊 **Gerçek Zamanlı Dashboard** - Tüm lisanslarınızı tek yerden yönetin
- 🔒 **Güvenli Doğrulama** - Hash tabanlı güvenlik sistemi
- 📈 **Analitik ve Raporlama** - Detaylı kullanım istatistikleri
- 🌙 **Modern Arayüz** - Karanlık/Açık tema desteği
- 📱 **Responsive Tasarım** - Tüm cihazlarda mükemmel görünüm

## 🚀 Hızlı Başlangıç

### Kurulum

1. **Projeyi klonlayın**
   ```bash
   git clone https://github.com/cheraphp/gateway
   cd gateway
   ```

2. **Bağımlılıkları yükleyin**
   ```bash
   # Ana proje bağımlılıkları
   npm install
   
   # Server bağımlılıkları
   cd server
   npm install
   cd ..
   ```

3. **Projeyi başlatın**
   ```bash
   npm run dev
   ```

Bu komut hem server'ı (port 3001) hem de client'ı (port 3000) aynı anda başlatır.

### Demo Giriş Bilgileri
- **E-posta:** admin@gateway.com
- **Şifre:** herhangi bir şifre

## 📋 API Endpoints

### Base URL
```
http://localhost:3001/api
```

### License API Endpoints

#### Test Endpoint
```http
GET /license-api/test
```
API durumunu kontrol eder (Authorization gerektirmez)

#### Client Endpoint
```http
POST /license-api/client
Content-Type: application/json
Authorization: your-api-key

{
  "licensekey": "XXXXX-XXXXX-XXXXX-XXXXX",
  "product": "Product Name",
  "version": "1.0.0"
}
```

### Demo Test Verileri
- **License Key:** DEMO-12345-ABCDE-67890
- **Product:** Demo Product
- **Version:** 1.0.0
- **API Key:** Herhangi bir değer (örn: demo-api-key-12345)

## 🔧 API Kullanımı

### Node.js Örneği

```javascript
const axios = require('axios');

const startup = async () => {
  const url = 'http://localhost:3001/api/license-api/client';
  const licensekey = 'DEMO-12345-ABCDE-67890';
  const product = 'Demo Product';
  const version = '1.0.0';
  const public_api_key = 'demo-api-key-12345';

  try {
    const res = await axios.post(
      url,
      { licensekey, product, version },
      { headers: { Authorization: public_api_key } }
    );

    if (!res.data.status_code || !res.data.status_id) {
      console.log('Invalid authentication');
      return process.exit(1);
    }

    if (res.data.status_overview !== 'success') {
      console.log('Authentication failed');
      console.log(res.data.status_msg);
      return process.exit(1);
    }

    const hash = res.data.status_id;
    const hash_split = hash.split('694201337');

    // Hash doğrulama
    const decoded_hash = Buffer.from(hash_split[0], 'base64').toString();
    const license_first = licensekey.substr(0, 2);
    const license_last = licensekey.substr(licensekey.length - 2);
    const public_api_key_first = public_api_key.substr(0, 2);

    if (decoded_hash !== `${license_first}${license_last}${public_api_key_first}`) {
      console.log('Authentication failed');
      return process.exit(1);
    }

    // Zaman doğrulama
    let epoch_time_full = Math.floor(Date.now() / 1000);
    const epoch_time = epoch_time_full
      .toString()
      .substr(0, epoch_time_full.toString().length - 2);

    if (parseInt(epoch_time) - parseInt(hash_split[1]) > 1) {
      console.log('Authentication failed');
      return process.exit(1);
    }

    console.log('Successful authentication');
  } catch (err) {
    console.log('Authentication failed');
    console.log(err);
    process.exit(1);
  }
};

startup();
```

### cURL Örneği

```bash
# Test endpoint
curl -X GET "http://localhost:3001/api/license-api/test"

# Client endpoint
curl -X POST "http://localhost:3001/api/license-api/client" \
  -H "Content-Type: application/json" \
  -H "Authorization: demo-api-key-12345" \
  -d '{
    "licensekey": "DEMO-12345-ABCDE-67890",
    "product": "Demo Product",
    "version": "1.0.0"
  }'
```

## 📊 Dashboard Özellikleri

- **Lisans Yönetimi**: Lisans oluşturma, düzenleme, silme
- **Ürün Yönetimi**: Ürün tanımlama ve versiyonlama
- **Kullanıcı Yönetimi**: Kullanıcı hesapları ve roller
- **Analitik**: Gerçek zamanlı istatistikler
- **Raporlama**: Detaylı raporlar ve dışa aktarma
- **API Dokümantasyonu**: Canlı API test aracı

## 🔒 Güvenlik

- Hash tabanlı doğrulama sistemi
- Zaman damgası kontrolü
- API key koruması
- CORS koruması
- SQLite veritabanı güvenliği

## 🎨 Teknolojiler

- **Frontend**: React, Tailwind CSS, Framer Motion
- **Backend**: Node.js, Express, SQLite
- **Authentication**: JWT Token
- **Development**: Vite, Concurrently

## 📝 Geliştirme

### Proje Yapısı
```
gateway/
├── src/                    # Frontend kaynak kodları
├── server/                 # Backend API server
├── public/                 # Statik dosyalar
└── package.json           # Ana proje konfigürasyonu
```

### Komutlar
```bash
npm run dev        # Hem server hem client'ı başlat
npm run client     # Sadece frontend'i başlat
npm run server     # Sadece backend'i başlat
npm run build      # Production build
```

## 🆘 Destek

Herhangi bir sorun yaşarsanız:
1. GitHub Issues'da sorun bildirin
2. Dokümantasyonu kontrol edin
3. Demo hesabı ile test edin

---

**GateWay** ile yazılımlarınızı güvenle koruyun! 🛡️