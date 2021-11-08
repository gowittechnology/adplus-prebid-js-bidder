# AdPlus Prebid.js Bidder Adaptör Geliştirici Kılavuzu

Bu kılavuz AdPlus Prebid.js bidder adaptörünün kullanımına ait geliştiricilerin ihtiyaç duyacağı bilgileri içermektedir. Burada yer alan yönergeleri izlemeden önce uygulamanızda Prebid.js entegrasyonu yapılmış olmalıdır. Prebid.js entegrasyonu için [Prebid.js](https://docs.prebid.org/prebid/prebidjs.html) dökümanlarından faydalanabilirsiniz.

## İçerik Tablosu

- [Genel Bakış](#overview)
- [AdPlus Prebid.js Bidder Adaptör Uygulama Entegrasyonu](#setup)
  - [Adım 1 - AdPlus Digital ID Scriptinin Uygulamaya Yüklenmesi](#include-the-dg-id)
  - [Adım 2 - Bidder Konfigürasyonu](#bidder-configuration)
    - [Reklam Boyutları](#sizing)
    - [Parametre Tanımları](#parameters)
      - [Zorunlu Parametreler](#banner-params)
      - [Opsiyonel Parametreler](#banner-opt-params)
  - [Adım 3 - Prebid.js'e Reklam Birimlerinin Eklenmesi](#add-units)

<a name="overview"/>

## Genel Bakış

AdPlus Prebid.js bidder adaptör **web uygulamalarınıza** header bidding yöntemi ile görsel reklamların eklenmesini sağlayan bir Prebid.js adaptörüdür.

AdPlus Prebid.js bidder adaptör aracılığıyla web uygulamalarınıza [Banner](https://blog.creatopy.com/beginner-guide-banner-ad/) reklamlar entegre edebilirsiniz.

Bu döküman AdPlus Prebid.js bidder adaptör'ü uygulamanıza nasıl entegre edebileceğinizi anlatır.

<a name="setup"/>

## AdPlus Prebid.js Bidder Adaptör Uygulama Entegrasyonu

Bu bölümde Web uygulamanıza AdPlus reklamlarını Prebid.js aracılığıyla ekleyebilmeniz için gereken adımlar yer almaktadır.

<a name="include-the-dg-id"/>

### Adım 1 - AdPlus Digital ID scriptinin uygulamaya yüklenmesi 

AdPlus Digital ID scriptini uygulamanıza yüklemeniz gereklidir. Bu script sayfanızın [`<head>`](https://www.w3schools.com/tags/tag_head.asp#:~:text=The%20element%20is%20a,scripts%2C%20and%20other%20meta%20information.) tagi içine yerleştirilmelidir.

```html
<script async src="https://ssp.ad-plus.com.tr/sdk/adplus_dg_id.js"></script>
```
Eğer uygulamanızda head tagi içine Digital Id scriptini yerleştiremiyorsanız aşağıdaki scripti body içerisinde de çağırabilirsiniz.

```html
<script type="text/javascript">
  (function () {
    var e = document.createElement("script");
    (e.type = "text/javascript"),
      (e.async = !0),
      (e.id = "adplusads-dg-id-library"),
      (e.src = "https://ssp.ad-plus.com.tr/sdk/adplus_dg_id.js"),
      window.top.document.getElementsByTagName("head")[0].appendChild(e);
  })();
</script>
```

<a name="bidder-configuration"/>

### Adım 2 - Bidder Konfigürasyonu

AdPlus bidder adaptörün çalışabilmesi için Prebid.js konfigürasyonunuza aşağıdaki gibi örnek bir AdPlus bidder adaptöre ait konfigürasyonları sayfanızın [`<body>`](https://www.w3schools.com/tags/tag_body.asp#:~:text=The%20tag%20defines%20the,%2C%20tables%2C%20lists%2C%20etc.) tagi içine yerleştirilmelidir. Aşağıdaki örnekte sağlanan parametreler envanter id'si **'-1'** ve reklam alanı id'si **'-3'** bir kampanya için **300 piksel genişlik** ve **250 piksel yükseklik**teki bir alana header bidding yöntemiyle AdPlus tarafından **teklif verilmesini** sağlayacaktır.

```html
<script type="text/javascript">
  var adUnits = [
        {
          code: "my-div", // Sayfada reklamı içerek DOM elementi.
          mediaTypes: {
            banner: {
              sizes: [[300, 250]],
            },
          },
          bids: [
            {
              bidder: "adplus",
              params: {
                inventoryId: "-1",
                adUnitId: "-3",
              },
            },
          ],
        },
      ];
</script>
```
<a name="sizing"/>

#### Reklam Boyutları

Reklam boyutları [Adım 2](#bidder-configuration) yer alan örnekteki **mediaTypes**  isimli nesne aracılığı ile sağlanmalıdır. Bu nesnede ilgili reklam tipine ait genişlik ve yükseklik değerlerinin piksel tipinde sağlanması zorunludur. AdPlus bidder adaptör yalnızca **banner** tipinde reklamları desteklemektedir. AdPlus bidder adaptör için örnek bir mediaTypes nesnesi aşağıdaki gibidir.

```javascript
mediaTypes: {
  banner: {
    sizes: [[300, 250]], // Raklamın boyutu piksel tipinde genişlik ve yükseklik değerleri içeren bir array içinde sağlanır
  },
},
```

<a name="parameters"/>

#### Parametre Tanımları

Sayfanıza AdPlus Prebid.js adaptör ile ekleyeceğiniz **Banner** reklamın zorunlu ve opsiyonel parametreleri aşağıdaki gibi olmalıdır. Bu alanlar [Adım 2](#bidder-configuration) de tanımlanamanız gereken nesnede **params** altında sağlanmalıdır.

<a name="banner-params"/>

**Zorunlu Alanlar**

- **adUnitId:** String - Geliştiricinin SSP panelinden aldığı Ad Unit entegrasyon ID si.
- **inventoryId:** String - Geliştiricinin SSP panelinden aldığı uygulama entegrasyon ID si.

<a name="banner-opt-params"/>

**Opsiyonel Alanlar**
- **yearOfBirth:** Number - Cihaz sahibinin doğum yılı.
- **gender:** String - Cihaz sahibinin cinsiyeti. Değeri **"F" (Kadın)** veya **"M" (Erkek)** olmalıdır.
- **categories:** Array < String > - Sayfa iceriğinin [IAB kategorileri](https://developers.mopub.com/publishers/ui/iab-category-blocking/).
- **latitude:** Number - Cihaz enlem değeri.
- **longitude:** Number - Cihaz boylam değeri.
- **extraData:** Object < String, Object > - Geliştiricinin göndermek istediği diğer datalar. Key-Value Dizisi

<a name="add-units"/>

### Adım 3 - Prebid.js'e Reklam Birimlerinin Eklenmesi 

Uygulamanızda yüklü bulunan Prebid.js kütüphanesi aracılığıyla [Adım 2](#bidder-configuration) de yer alan AdPlus'a ait nesnenin yüklenmeis gereklidir. Prebid.js nesnesinin uygulamanızdaki isimlendirilmesine bağlı olarak bu işlem genelde aşağıdaki satırlar aracılığıyla ve **addAdUnits** metotu ile yapılmaktadır.

```html
<script type="text/javascript">
  var adUnits = [
        {
          code: "my-div", // Sayfada reklamı içerek DOM elementi.
          mediaTypes: {
            banner: {
              sizes: [[300, 250]],
            },
          },
          bids: [
            {
              bidder: "adplus",
              params: {
                inventoryId: "-1",
                adUnitId: "-3",
              },
            },
          ],
        },
      ];
  
   // Prebid.js nesnesi  
   var pbjs = pbjs || {};
   // Adım 2 de oluşturulan nesne addAdUnits metotu aracılığıyla Prebid.js'e tanımlanır.
   pbjs.addAdUnits(adUnits);
</script>
```
