# DESIGN.md — Multiverse Restaurant

> Oyunun ruhu, hikayesi, karakterleri ve tasarım niyeti için kutsal kitap.
> CLAUDE.md → kod kuralları. DESIGN.md → her şeyin _neden_ ve _ne_ olduğu.
> Her iki kişi de, Claude Code da buna bakarak karar verir.

---

## 0. OYUNUN ÖZÜ — Tek Paragraf

Dünyanın en iyi aşçısı olduğunu kanıtlamak için restoranını büyüten bir adamın hikayesi.
Adam evrendeki tehlikeli her yeri görmüş, her şeyi biliyor, hiçbir şey onu şaşırtmıyor.
Eşi ise o uzaylının bize neden öyle baktığını hâlâ düşünüyor.
Bir de ABD Başkanı var — sırf adrenalinden geliyor, sürekli mahsur kalıyor, biz kurtarıyoruz.

> **Ton:** Absürd kara komedi. Everest'e tırmanan adam zirvede selfie çekiyor.
> Evrenin sonu gelirken protagonist malzeme listesine bakıyor.

---

## 1. KARAKTERLER

### 1.1 Protagonist — Aldric

**Kod adı içeride:** `PROTO`

| Alan                    | Detay                                                                            |
| ----------------------- | -------------------------------------------------------------------------------- |
| **Tip**                 | "Her şeyi gördüm, hepsi aynı" enerjisi. Rick varyantı ama telif-güvenli.         |
| **Motivasyon**          | Dünyaca ünlü bir şefle tartıştı. İnat uğruna dünyanın en iyi restoranını açacak. |
| **Multiverse'e bakışı** | Turistik. Tanıdık. Sıradan. _"Ah, bu evren. Bunların ikinci ayını atla."_        |
| **Sürprizlere tepkisi** | Hafif sinirlilik. Planını bozan şeye kızıyor, ölüm tehlikesine değil.            |
| **Konuşma tarzı**       | Kısa, alaycı, enformasyonla dolu. Sormadan açıklıyor. Kimse istemedi.            |
| **Gizli katman**        | Her şeyi bilmesi onu _yalnız_ yapıyor — ama bunu kabul etmiyor.                  |

**İsim:** Aldric

**Savaş Rolü:** Yakın mesafe brawler. Grid'de düşmana yapışır, yüksek hasar verir. Ön cephede savaşır.

**Multiverse Cihazı:** Rick tarzı bir cihaz — Aldric'in kendisi üretmiş. Dahi seviyesinde zeki. Cihaz restoranda arka depo odasında çalıştırılıyor. Nasıl/ne zaman yaptığı sorgulanmaz — adam Rick gibi, var işte.

**Backstory:** Evren evren parça parça açılıyor. Her evrende tanıdıkları var, geçmişe ait diyaloglar çıkıyor. Oyuncu ne kadar çok evren gezerse o kadar çok öğreniyor. Backstory tek seferde anlatılmaz — oyuncuyu ilerlemeye hem mekanik hem hikaye için motive eder.

---

### 1.2 Eş — Lena

**Kod adı içeride:** `SPOUSE`

| Alan                    | Detay                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Tip**                 | Protagonistin tam zıttı. Temkinli, vicdanlı — ama yanlış şeylere takılıyor.                                                                                  |
| **Motivasyon**          | Eşini seviyor, işi büyütmek istiyor. Ve o uzaylının neden öyle baktığını öğrenmek istiyor.                                                                   |
| **Multiverse'e bakışı** | Her evreni _sosyal_ okuyor. Kültürü, normları, inceliyor.                                                                                                    |
| **Komedi kaynağı**      | Ölüm tehlikesinde değil, ahlaki detaylarda bunalıyor. _"O adamın mallarını çaldık, vicdan azabı çekiyorum."_ — Protagonist: _"O adam bizi yemek istiyordu."_ |
| **Güçlü yanı**          | Protagonist'in gözden kaçırdığı sosyal/duygusal ipuçlarını yakalıyor. Zaman zaman oyunu kurtarıyor bu.                                                       |
| **Konuşma tarzı**       | Uzun sorular. Cümle ortasında başka bir şeye atlıyor.                                                                                                        |

**İsim:** Lena

**Savaş Rolü:** Şifacı + uzak mesafe nişancı (sniper). Arkadan destek, Aldric'i iyileştirir, uzaktan hasar verir. Pozisyonu önemli — korunması lazım. Düşman Lena'ya ulaşırsa gerilim yükselir.

**Multiverse'ü ilk öğrenmesi:** Lena multiverse'ü bilmiyor. İlk gece Aldric "benimle gel, malzeme almamız lazım" diyor ve portal açıyor. Lena şok içinde ama merakından saçma sorular soruyor, atmosferin ilgi çekiciliğine kapılıyor. Aldric ise "oyalanma, almamız gereken malzemeler var" diyip sürüklüyor.

**Savaşa başlangıcı:** Lena daha önce hiç savaşmamış. Aldric ona bir cihaz/silah veriyor — "al bunu, ihtiyacın olursa kullan." Bu hem pratik hem ilişkiyi gösteriyor: Aldric sert ve alaycı ama eşini hazırlıksız getirmemiş. Sevgi gösterme şekli bu.

**Restoran rolü:** Gündüz Lena'nın alanı. Tutorial'ı Lena veriyor (ocak kullanımı, müşteri servisi) — organik, karakter üzerinden. Lena restoranda mutlu, insanlarla bağlantı kuruyor.

**Protagonist — Eş Dinamiği:**

```
Protagonist: [Tehlikeli bir evrendeyiz, düşmanlar geliyor]
              "Malzemeleri al, çıkıyoruz."

Eş:          "Tamam ama — o yaratık bize neden üç kez baktı?
              Burada selamlama ritüeli var mı? Onu incittik mi?"

Protagonist: "Bize yemek istedi. Yürü."

Eş:          "Ama iletişim kurmaya çalışıyor olabilir—"
              [yaratık saldırıyor]
              "—tamam yürüyoruz."
```

---

### 1.3 ABD Başkanı — [İSİM TBD]

**Kod adı içeride:** `POTUS`

| Alan                    | Detay                                                                                                             |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **Tip**                 | Güç sahibi, karizmatik, tamamen sorumsuz.                                                                         |
| **Motivasyon**          | Saf adrenalin. Multiverse macerası hobi olarak.                                                                   |
| **Oyundaki rolü**       | Geçici kadro üyesi — 6-7 evren boyunca zaman zaman katılır.                                                       |
| **Komedi formülü**      | Gelir. Heyecanlanır. Mahsur kalır. Biz kurtarırız. Teşekkür eder. Bir sonraki evren için mesaj atar.              |
| **Mekanik yansıması**   | Savaşta güçlü ama impulsive — planı bozan hamleler yapabilir. Ya da tam tersi: beklenmedik anda oyunu kurtarıyor. |
| **Evrenlerle ilişkisi** | Hiçbir evrenin kültürünü bilmiyor. Hepsinde aynı şekilde davranıyor. Bu bazen işe yarıyor.                        |

> **Not:** Bu karakter Başkan olarak kalacak ama gerçek bir Başkana referans vermeyecek.
> Absürd, karikatürist bir "Başkan arketipi."

---

### 1.4 Rakip Şef — Dorian

**Kod adı içeride:** `RIVAL`

| Alan                                      | Detay                                                                                                                                                                         |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Tip**                                   | Protagonist'in karşısındaki ayna. Aynı derecede yetenekli, daha az vicdan.                                                                                                    |
| **Motivasyon**                            | Başlangıçta bilinmiyor. Zamanla açılıyor.                                                                                                                                     |
| **Oyundaki rolü — Başlangıç**             | Sadece ses. Haberler, tauntlar, mesajlar. Protagonist'i kızdıran bir arka plan sesi.                                                                                          |
| **Oyundaki rolü — Orta**                  | Protagonist'in düşmanları onunla iş birliği yapıyor. Restoran rekabeti kızışıyor.                                                                                             |
| **Oyundaki rolü — Son**                   | `[TBD — Final boss mu? Twist mi? Sertan kararı.]`                                                                                                                             |
| **Gerçek motivasyon**                     | Unvanını kaybetmemek. Dünya zaten onu en iyi kabul ediyor — bu ona derin bir rahatlık ve ego vermiş. Aldric gelene kadar kimse sorgulamadı.                                   |
| **Neden düşmanlarla iş birliği yapıyor?** | Panik. Kötülük değil — köşeye sıkışmış birinin her şeyi yapması. Elindekini kaybetmemek için kendini meşrulaştırıyor.                                                         |
| **Twist potansiyeli**                     | Dorian bir noktada multiverse'ü öğreniyor. Aldric'in oradan malzeme getirdiğini fark edince _"ben kurallara göre oynadım, o değil"_ diye rahatça meşrulaştırabiliyor kendini. |
| **Önemli detay**                          | Dorian haksız değil — dünya gerçekten onu en iyi biliyor. Bu onu klasik kötü adamdan çok daha tehlikeli yapıyor.                                                              |

> **Tasarım notu:** Final sahnesinde oyuncu Dorian'a acıyabilmeli. Kaybeden biri var karşında, canavar değil.

---

### 1.5 Diğer Karakterler — TBD Kadro

Evren başına 1-2 geçici karakter eklenebilir. Şablon:

```
[KARAKTER ADI]
Evren:        [Hangi evren]
Tip:          [Arketip — savunucu, deli bilim insanı, kültür elçisi, vb.]
Kalıcılık:    [Sadece bu evren / Tekrar görünür / Restoran müşterisi olur]
Komedi rolü:  [Bu karakterin yarattığı komik durum ne?]
```

---

## 2. EVRENLER

> CLAUDE.md'de Synty paketleri belirlendi. Burada her evrenin _kimliği_ ve _tonu_ var.
> Bir evren sadece görsel değil — o evrende olaylar nasıl hissettiriyor?

### Evren Şablonu

```
## [EVREN ADI]
Synty Paketi:  [Hangi Synty seti]
Tek Cümle:     [Bu evren nedir?]
Ton:           [Nasıl hissettiriyor — tehditkar, absürd, nostaljik, yabancı?]
Protagonist'e tanıdık gelecek detay:  [Ne görünce "ah bu" diyecek?]
Sürpriz unsuru:  [Beklenmeyecek şey ne?]
Restoran bağlantısı:  [Buradan ne malzeme/tarif/dekorasyon geliyor?]
Özel mekanik:  [Bu evrende combat veya restoran nasıl farklılaşıyor?]
```

### 2.1 Cyberpunk — "Neon Altı"

| Alan                    | Detay                                                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Synty Paketi**        | Polygon City + Sci-Fi iç mekan                                                                                                        |
| **Tek Cümle**           | Korporasyonların her şeyi kontrol ettiği bir şehir — ama alt katta hâlâ insanlar yaşıyor.                                             |
| **Ton**                 | Yorgun, ıslak, neon. Tehlike sıradan, yolsuzluk beklenen.                                                                             |
| **Aldric'e tanıdık**    | _"47. kattaki güvenliği atlayın, arka merdivenle inin. Biliyorum çünkü bu şehri kurdum — o versiyonda."_                              |
| **Sürpriz unsuru**      | Korporasyonun CEO'su Dorian'la iş ortağı. Malzeme kaçakçılığını o finanse ediyor.                                                     |
| **Restoran bağlantısı** | Sentetik proteinler, illegal tatlandırıcılar, yasak baharatlar. Menüde "kaynağı sorma" notu var.                                      |
| **Lena detayı**         | Underground halkıyla empati kuruyor. _"Bunlar sadece hayatta kalmaya çalışıyor."_ Aldric: _"Herkes hayatta kalmaya çalışıyor. Yürü."_ |
| **Özel mekanik**        | `[TBD — Görünmezlik/hack kartları? Işık/gölge sistemi?]`                                                                              |

---

### 2.2 Fantezi / Dungeon — "Eski Derinlik"

| Alan                    | Detay                                                                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Synty Paketi**        | Polygon Dungeon + Polygon Fantasy Kingdom                                                                                              |
| **Tek Cümle**           | Ejderhalar, büyücüler, antik lanetler — ve bunların hepsi Aldric için turistik.                                                        |
| **Ton**                 | Epik görünüyor, içeriden saçma. Kahraman efsaneleri var ama kahramanlar berbat karar alıyor.                                           |
| **Aldric'e tanıdık**    | _"Bu zindanı biliyorum. Beşinci odada tuzak var, üçüncü sütunun yanında bas."_ Lena: _"Nasıl biliyorsun?"_ Aldric: _"Biliyorum işte."_ |
| **Sürpriz unsuru**      | Dungeon'ın "son canavarı" aslında mahsur kalmış sıradan biri. Yıllardır çıkamıyor.                                                     |
| **Restoran bağlantısı** | Dragon eti `[etik sorun — Lena itiraz ediyor]`, antik mantar, büyülü otlar. Tariflerin yarısı gerçekten büyü içeriyor.                 |
| **Lena detayı**         | Her yaratıkla konuşmaya çalışıyor. Bazen işe yarıyor — savaşı atlatıyorlar. Aldric bunu kabul etmiyor.                                 |
| **Özel mekanik**        | `[TBD — Büyü kartları, element zayıflıkları?]`                                                                                         |

---

### 2.3 Sci-Fi — "Açık Uzay"

| Alan                    | Detay                                                                                                 |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| **Synty Paketi**        | Polygon Sci-Fi Space                                                                                  |
| **Tek Cümle**           | Yıldızlar arası seyahat normal, insanlık yayıldı — ama temel sorunlar hâlâ çözülmedi.                 |
| **Ton**                 | Geniş, soğuk, sessiz. Tehlike uzaktan geliyor. Aldric burada en rahat.                                |
| **Aldric'e tanıdık**    | Bu evren Aldric'in en çok tanıdığı. Burada neredeyse evinde gibi hissediyor — bu onu rahatsız ediyor. |
| **Sürpriz unsuru**      | Bir uzay istasyonunda Aldric'in geçmişine ait bir şey var. Ne olduğu `[TBD — Sertan kararı]`.         |
| **Restoran bağlantısı** | Başka gezegenlerin ham ürünleri, uzay bitkisi, defalarca klonlanmış et `[Lena bunu çok soruyor]`.     |
| **Lena detayı**         | Uzaylı kültürleri büyülüyor onu. Her birinin sosyal normlarını not alıyor. Defteri var.               |
| **Özel mekanik**        | `[TBD — Yerçekimsiz grid? Uzaklık bazlı hasar?]`                                                      |

---

### 2.4 Apocalypse Wasteland — "Sonrası"

| Alan                    | Detay                                                                                                         |
| ----------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Synty Paketi**        | Polygon Apocalypse / Wasteland `[paket adını kontrol et]`                                                     |
| **Tek Cümle**           | Dünya bitti. İnsanlar hâlâ alışkanlıklarını sürdürüyor. Bu daha ürkütücü.                                     |
| **Ton**                 | Kuru, gri, absürd. Trajedi o kadar normalleşmiş ki komik.                                                     |
| **Aldric'e tanıdık**    | _"Bu evrenin sonu iki yüz yıl önce geldi. Hâlâ buradalar çünkü gidecek yer yok."_                             |
| **Sürpriz unsuru**      | Wasteland'in ortasında mükemmel işleyen küçük bir topluluk var. Dorian'ın bu toplulukla bağlantısı var.       |
| **Restoran bağlantısı** | Radiasyonla mutasyona uğramış bitkiler, "son nesil" hayvan ürünleri, kıyamet öncesi tariflerin son kopyaları. |
| **Lena detayı**         | Bu evren onu en çok etkiliyor. _"Bunlar nasıl devam edebiliyor?"_ Gerçek bir kırılma anı burada yaşanabilir.  |
| **Başkan bağlantısı**   | Başkan bu evrende en çok mahsur kalıyor. Wasteland'i "heyecan verici" buluyor. Aldric: _"…"_                  |
| **Özel mekanik**        | `[TBD — Kaynak kıtlığı kartları? Takım HP paylaşımı?]`                                                        |

---

### 2.5 Goblin Evreni — "Yıldız Korsanları"

| Alan                    | Detay                                                                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **Synty Paketi**        | Polygon Sci-Fi Space + Fantasy karakterler karışımı                                                                            |
| **Tek Cümle**           | Uzayın Vikingleri. Yağma yapıyorlar ama onur kodları var — ve bu kodu ciddiye alıyorlar.                                       |
| **Ton**                 | Gürültülü, kaotik, ama içinde bir düzen var. Kendi aralarında son derece organize.                                             |
| **Aldric'e tanıdık**    | Goblinlere saygı duyuyor. _"Bunlarla savaşma. Önce liderini bul, onurlu meydan oku."_ Lena: _"Ciddi misin?"_ Aldric: _"Evet."_ |
| **Sürpriz unsuru**      | Goblin kaptanı Aldric'i tanıyor. Geçmişte bir onur borcu var aralarında. `[TBD — borç ne?]`                                    |
| **Restoran bağlantısı** | Yıldızlar arası nadir baharatlar, "savaş ganimetleri" olarak el değiştirmiş malzemeler, Goblin fermente içkisi.                |
| **Lena detayı**         | Goblin kültürünü en çok Lena anlıyor. Onur kodunu öğreniyor, Goblinler ona saygı duyuyor. Aldric sinirle kabul ediyor.         |
| **Özel mekanik**        | `[TBD — Boarding saldırısı? Onur düellosu sistemi?]`                                                                           |

---

## 3. HİKAYE AKIŞI

### 3.0 Açılış Sahnesi (Sinematik)

```
SİNEMATİK:
Lena, Aldric'i ünlü herkes tarafından sevilen bir restorana gitmeye zorluyor.
Aldric sinirli ama gidiyor. Yemekleri tadıyor.
Hem kıskançlık hem "ben daha iyisini yaptım" enerjisiyle dalga geçmeye başlıyor.
Restoranın baş aşçısı / sahibi Dorian bunu duyuyor.
Aralarında diyalog başlıyor — gerilim, ego, rekabet.
Aldric: "Daha iyisini yaptığımda benimle rekabet edebilecek misin bakalım."
Dorian'ın restoranının karşısındaki küçük dükkanı tutuyor.
```

**Mekân:** Günümüz şehrinde, bildiğin bir cadde üstü. Dorian'ın restoranı caddenin bir tarafında, Aldric'in küçük dükkanı karşı tarafta. Her gündüz phase'inde rekabet görsel olarak hatırlanır.

**Dükkan:** Küçük ama dayalı döşeli — başlangıç noktası hazır. Arka tarafında depo odası var — multiverse cihazının bulunduğu yer.

### 3.1 İlk Gün Akışı

```
SİNEMATİK BITTI → GÜNDÜZ 1
═══════════════════════════
Dükkan hazır. Lena tutorial veriyor:
- Ocak kullanımı, müşteri servisi, temel mekanikler
- Gerçek satış yok — Lena "ben müşteriyim, gel benden sipariş al" diyor
- Kendi aralarında muhabbet, organik tutorial
- Lena'nın alanı burası — restoran onun dünyası

AKŞAM OLUYOR
═════════════
Aldric: "Benimle gel, malzeme almamız lazım."
Lena: "Nereden?"
Aldric depo odasına götürüyor. Cihazı çalıştırıyor. Portal açılıyor.
Lena şok. Aldric: sıradan, alışkın. "Yürü."

İLK EVREN: CYBERPUNK — "NEON ALTI"
════════════════════════════════════
Neon ışıklı karanlık sokağa çıkıyorlar.
Lena etrafına bakınırken Aldric direkt bir ara sokağa sapıp kapıyı çalıyor.
Karaborsacıyı adıyla tanıyor. Lena: "Sen... burada tanıdığın mı var?!"
Bu sahne tek başına Aldric'in kim olduğunu gösteriyor — açıklama yok, gösterme var.

Alışveriş sırasında baskın oluyor — savaş zorunlu, kaçınılmaz.
Herkes panik, Aldric "gene mi bunlar" enerjisiyle savaşa giriyor.
Lena "NE OLUYOR?!" modunda ama Aldric'in verdiği cihazla savaşmak zorunda kalıyor.
İlk combat tutorial burada.

Run devam ediyor: 5 odalık Cyberpunk evreni.
Boss'u yenerse tüm malzemeyi götürür. Ölürse %50 kayıp, restoranda uyanır.

SABAH — GÜNDÜZ 2
═════════════════
Toplanan malzemelerle ilk yemek. İlk gerçek müşteriler.
Döngü başlıyor.
```

### 3.2 Büyük Ark — 3 Perde

```
PERDE 1 — "İspat"
Protagonist restoranı kuruyor. Düşman az, evrenler tanıdık.
Rakip şef uzaktan taunt ediyor. Motivasyon net: haddini bildireceğiz.
Oyuncu sistemi öğreniyor.

PERDE 2 — "Komplikasyon"
Düşmanlar rakip şefle iş birliği yapıyor.
Evrenler beklenmedik bağlantılar göstermeye başlıyor.
Protagonist'in "her şeyi biliyorum" kabuğu çatlamaya başlıyor — küçük, sinir bozucu şekillerde.
Eş bazı sosyal detaylarla gerçekten önemli bir şey keşfediyor.

PERDE 3 — "Hesap"
Rakip şefin asıl motivasyonu açılıyor.
Final confrontation — mutfakta mı, evrendeyken mi? [TBD]
Protagonist kazanıyor. Ama kazanmak beklediği gibi hissettirmiyor. [TBD — nasıl çözüleceği]
```

### 3.3 Gündüz / Gece Döngüsü Hikaye Katmanı

```
GECE (Combat / Evren)
→ Protagonist liderlikte, eş destek
→ Savaş dışında BG3 gibi serbest hareket (click to move), NPC'lerle konuş, keşfet
→ Savaş tetiklenince grid'e kilitli sıra tabanlı combat
→ Diyalog: tanıdık evren yorumları + eşin sosyal analizleri
→ Düşman motivasyonları zamanla şef bağlantısını açıyor
→ Aldric'in backstory'si evren evren parça parça açılıyor

GÜNDÜZ (Restoran)
→ Normal müşteriler, normal insanlar
→ Ama malzemeler çok anormal
→ Müşteri diyalogları: gece olaylarına dolaylı referanslar
→ Eş burada mutlu — bu ortamı seviyor, insanlarla bağlantı kuruyor
→ Protagonist burada huzursuz — bu "sıradan" dünya onu bunaltıyor
→ Para kazanılır → restoran genişletme, dekorasyon, yemek buff'ları
```

> **Tasarım notu:** Gündüz sahneleri protagonist'in tek "kırılganlık" penceresi.
> Restoranda normal insan olmak zorunda kalıyor. Bu onu rahatsız ediyor.
> Burada eşle ilişki derinleşiyor.

---

## 4. TON & YAZI KURALLARI

### Protagonist Sesi

- Cümleler kısa. Noktalı. Bilgi yoğun.
- Tehlikeyi tanımlıyor, tepki vermiyor.
- Evren hakkında kimse sormadan bilgi veriyor. Yanlış zamanda.
- **Asla** romantik veya nostaljik değil. Duygu gösterirse hemen geri alıyor.

### Eş Sesi

- Cümleler uzun, atlıyor, bölünüyor.
- Her şeyde "ama neden?" soruyor — yanlış şeylerde.
- Ahlaki kaygıları gerçek ve samimi — absürd değil, durum absürd.
- Zaman zaman protagonist'ten daha doğru çıkıyor.

### Diyalog Altın Kuralı

> Hiçbir karakter "kahramanım" veya "harika iş çıkardın" demiyor.
> Protagonist iltifat beklemiyor. Eş iltifat ediyor ama zamanlama hep yanlış.
> Tehlike anında küçük şeyler konuşuluyor.

### Referans Ton Örnekleri

- Rick & Morty S1E1 — Rick'in portal açarken "whatever" enerjisi
- High on Life — düşmanların absürd motivasyonları
- Afterard _(kendi oyunumuz)_ — absürd kara komedi, beklenmedik ölüm anları

---

## 5. MEKANİK — HİKAYE BAĞLANTISI

> CLAUDE.md mekaniklerin _nasıl_ çalıştığını söylüyor.
> Burada mekaniklerin _neden_ var olduğu ve hikayeyle bağlantısı var.

| Mekanik                              | Hikaye Açıklaması                                                     |
| ------------------------------------ | --------------------------------------------------------------------- |
| Kartlar kalıcı, run sonunda silinmez | Protagonist öğreniyor, unutmuyor. Her evren onu biraz daha formluyor. |
| Eş sabit partner                     | İlişki oyunun duygusal zemini. Eşi kaybetmek = gerçek kayıp.          |
| Malzeme topla → restoranda kullan    | Tehlike ile sıradanlık arasındaki absürd köprü.                       |
| Başkan geçici kadro                  | Güç bu evrende anlamsız. Para satın alamıyor. Adrenalini var sadece.  |
| Düşmanlar şefle iş birliği yapıyor   | Protagonist'in "kişisel" meselesi evrensel boyuta taşınıyor.          |
| Ölüm = klonlanma, kayıp = %50 malzeme | Ölüm mekanik olarak bedelli ama hikaye olarak "gerçek ölüm" değil.  |
| Yemek buff'ları savaşı etkiler       | Restoran sadece para makinesi değil — gece hazırlığının parçası.      |
| Restoran Dorian'ın karşısında         | Rekabet her gündüz görsel olarak hatırlanır — motivasyon düşmez.     |
| Yemek pişirme timing mini-game       | Malzeme zor kazanılıyor — dikkatli pişir, çöpe atma. Ustalık hissi. |
| Aşçıdan patrona evrim                | Mekanik yük azaldıkça hikaye yükü artıyor — restoran sıkmaz.        |

### 5.1 Kart Sistemi Felsefesi — BG3 Spell Book Modeli

> Kartlar Slay the Spire gibi run-başı rastgele değil, **Baldur's Gate 3'teki spell book** gibi kalıcı.
> Karakter geliştikçe spell book büyüyor — ama turda kullanabileceğin aksiyon puanı sınırlı.

**Gerilim kaynağı kart havuzundan değil, encounter tasarımından gelir:**

- **Büyük deck = daha çok seçenek, daha güçlü olmak değil.** BG3'te 20 spell biliyorsun ama turda 1 action + 1 bonus action var. Bizde de `maxActionPoints = 2` — doğru kartı doğru anda seçmek önemli.
- **Encounter çeşitliliği.** Aynı deck'le bile her oda farklı hisseder: düşman kombinasyonları, grid layout'u, çevresel tehlikeler rastgele. "Fireball'um var ama takım arkadaşlarım da ateş hattında" gerilimi.
- **Kaynak yönetimi.** Run boyunca HP ve enerji tükeniyor — restoranda dinlenince doluyor. Gece ne kadar ileri gidersen o kadar risk, ama o kadar iyi malzeme. BG3'teki long rest/short rest ritmi.

**Kart Havuzu & Deck Sistemi:**
- Kart havuzu **ortak** — düşen kart herhangi bir karaktere verilebilir
- Ancak kartlarda **sınıf kısıtlaması** var: bazı kartları sadece belirli karakterler kullanabilir
- Kart üzerine gelinince hangi sınıfların kullanabileceği gösterilir
- Kullanamayacak karaktere verilirse uyarı çıkar ("Aldric bunu kullanamaz")
- Ortak kartlar da var — herkes kullanabilir

**Aktif Deck:**
- Her karakter için **maksimum 6 kart** aktif deck'te olabilir
- Tüm kazanılmış kartlardan bu gece için 6'sını seç (BG3 spell preparation)
- Son konfigürasyon otomatik önerilir — her gece sıfırdan seçmek zorunda değilsin

**Kart Cooldown:**
- Her kartın **kullanım cooldown'u** var (tur bazlı)
- Bazı kartlar her tur oynanabilir (cooldown: 0)
- Güçlü kartlar 3-4 turda bir oynanabilir
- Bu "en iyi kartı spam" sorununu çözer — taktik çeşitlilik zorlar

**Hareket & Aksiyon Ayrımı (BG3 modeli):**
- Hareket **bedava** — moveRange kadar yürü, aksiyon puanı harcamaz
- Aksiyon puanı (2) **sadece kart oynamak** için harcanır
- Bir turda hem yürü hem 2 kart oyna — BG3'teki movement + action + bonus action hissi

### 5.2 Evren Yapısı — Hades Modeli

Her evren **5 sabit oda**. Odalar her run'da aynı sırada — Hades gibi. Değişen şey düşman kombinasyonları, loot dropları ve diyalog satırları.

**Oda tipleri:** Savaş, keşif/diyalog, boss — sıralama evren bazında custom tasarlanır.

```
Örnek A (Cyberpunk):           Örnek B (başka evren):
ODA 1: Diyalog/keşif           ODA 1: Direkt savaş
ODA 2: Savaş (baskın)          ODA 2: Savaş
ODA 3: Savaş (daha zor)        ODA 3: Diyalog/keşif
ODA 4: Boss                    ODA 4: Savaş
ODA 5: Diyalog/keşif           ODA 5: Boss
```

> **Tasarım notu:** Her evrenin oda sıralaması Sertan tarafından custom belirlenir.
> Bazı evrenlerde 5 oda da savaş (wanted karakter oluruz), bazısında diyalog ağırlıklı.
> `UniverseData` ScriptableObject'inde oda tipi ve sırası editörden ayarlanabilir.

**İki mod:**

| Savaş Dışı (Keşif)               | Savaş (Combat)                     |
| --------------------------------- | ---------------------------------- |
| BG3 gibi serbest hareket          | Grid'e kilitli sıra tabanlı       |
| Click to move                     | Kart oyna, hareket et, tur bitir   |
| NPC'lerle konuş, loot topla      | Pozisyon bazlı taktik              |
| Ortamı gez, atmosferi yaşa       | Düşmanlarla savaş                  |
| Kamera: yakın, atmosferik         | Kamera: uzak, taktik görüş         |
| Aldric kontrol, diğerleri takip   | Her karakteri sırayla kontrol      |

**Evren başına level design yapılıyor** — her oda fiziksel mekân, atmosfer oradan geliyor. Slay the Spire harita düğümü değil.

**Kamera Sistemi:**
- **Keşif modu:** Karaktere yakın, atmosferik. 2.5D izometrik açı.
- **Savaş modu:** Kamera yukarı çeker, taktik görüş. Grid'in tamamını görebilirsin.
- **Geçiş:** Savaş tetiklenince kamera smooth şekilde yukarı çeker. Savaş bitince geri yakınlaşır.
- **Restoran:** Keşif kamerası ile aynı — yakın, 2.5D izometrik.

**Grid Sistemi — Otomatik Oluşturma:**
- Grid boyutu odaya göre değişir (sabit 8x6 değil)
- Sistem mevcut haritayı **otomatik tarar**: yükseklik, engeller, duvarlar
- Belirli yükseklikten büyük alanlar otomatik engel/disabled grid olur
- Level designer (Sertan) sadece odayı tasarlar, grid kendini ona göre şekillendirir
- Her oda için elle grid çizmeye gerek yok

**Keşif Modu Etkileşim:**
- **Mouse/Keyboard:** Tıkla → Aldric yürüyor → varınca etkileşim menüsü (BG3 gibi)
- **Gamepad:** Sol analog ile yürü → etkileşim menzilinde prompt belir → A tuşuna bas
- Aldric ana kontrol, yardımcı karakterler (Lena + diğerleri) otomatik takip
- NPC'lerle konuşma, loot toplama, obje inceleme hep yaklaş + tıkla

**Savaşa Geçiş — Aynı Mekânda (BG3 modeli):**
- Savaş tetiklenince sahne DEĞİŞMİYOR — aynı mekânda kalıyorsun
- Kamera yukarı çeker, grid overlay belirir, sıra tabanlı mod başlar
- Savaş bitince grid kaybolur, kamera yakınlaşır, keşfe devam
- Ayrı savaş sahnesi / yükleme ekranı YOK

**Savaşta Tur Sırası — Initiative Sistemi:**
- Her karakter için arkada bir **zar atılır** (D&D/BG3 initiative)
- En yüksek zar önce oynar
- **Aldric'e initiative buff** var — genelde 1. veya 2. sırada olur
- Her savaş başında yeniden atılır

**Düşman Arketipleri (her evrende):**
- **Yakın mesafe saldırgan** — sana doğru koşar, vurur
- **Uzak mesafe** — uzaktan ateş eder, kaçar
- **Tank/kalkan** — yavaş ama dayanıklı, arkadakileri koruyor
- Tema evrene göre değişir (Cyberpunk'ta robot/çeteci, Dungeon'da goblin/iskelet vs.)

**Portal Geçiş Akışı:**
```
Restoran kapandı → depo odasına git → portal aç → evren seç
    ↓
TEK EKRANDA: Deck düzenleme + yemek buff seçimi + onayla
    ↓
Evren yükleniyor → 1. oda başlıyor
```

### 5.3 Ölüm & Respawn Sistemi

**Lore:** Restoranda bir yapay zeka var. Karakterleri anlık yedekliyor. Ölünce yapay zeka seni restoranda geri ışınlıyor — bir çeşit klon ama beyin, anılar, her şey kaldığı yerden devam ediyor.

**Mekanik:**
- Ölürsen → malzemelerin **%50'si** kurtarılır, geri kalanı kayıp
- Boss'u yenersen → **tüm kazançları** götürürsün
- Ölünce gündüz oluyor — o run bitti, restoran işletmeye başlıyorsun
- Ertesi gece aynı evrene girince **1. odadan başlıyorsun** (Cult of the Lamb / roguelike klasiği)
- Ama daha güçlüsün: yeni kartların var, restorandan buff yemeklerin var

**Günlük döngü:** Her gün = 1 gece runu + 1 gündüz restoran. Temiz, tekrarlanan döngü.

### 5.4 İlerleme (Progression) Sistemi

> **Ayrı level/skill tree sistemi YOK.** İki ilerleme ekseni yeterli: Kartlar + Restoran.

```
SAVAŞTA GÜÇLENMENİN YOLU
══════════════════════════
1. Kartlar (evrenden kazanılır) → yeni ability'ler, BG3 spell book mantığı
2. Yemek buff'ları (restorandan) → savaş öncesi geçici güçlendirmeler
   "Ejderha Biftek" → bu gece +2 HP
   "Neon Ramen" → bu gece +1 aksiyon puanı

RESTORAN GELİŞTİRMENİN YOLU
════════════════════════════
1. Para (yemek satışından) → dekorasyon, genişleme, mutfak upgrade
2. Malzeme (evrenden) → yeni tarifler, yeni yemekler
3. Boss yenmek → yeni tarif/dekorasyon kilitleri açılır

İlerleme kilidi var — her şey serbest değil.
Diablo 4 / Cult of the Lamb gibi kademeli açılma.
Birden en üst seviye upgrade yasak.
```

**Neden bu yapı:**
- Kartlar = BG3 spell book. Ayrı level sistemi gereksiz — kart çeşitliliği zaten güçlenme.
- Restoran anlamlı: sadece para makinesi değil, yemek buff'larıyla savaşı etkiliyor.
- İki döngü birbirini besliyor, üçüncü sisteme gerek yok.
- 2 kişilik ekip için sürdürülebilir kapsam.

---

## 6. AÇIK SORULAR — Karar Bekleyenler

> Bu bölüm zamanla boşalacak. Her karar verilince üstüne `[KARAR: X]` yaz.

- [x] Protagonist'in adı ne? — **Aldric**
- [x] Eşin adı ne? — **Lena**
- [x] Rakip şefin adı — **Dorian**
- [x] Dorian'ın motivasyonu — **Unvanını kaybetmemek. Panikten iş birliği yapıyor, kötülükten değil.**
- [x] Açılış sahnesi — **Sinematik: Dorian'ın restoranına gidiyorlar, tartışma, karşıya dükkan tutma.**
- [x] Mekân — **Günümüz şehri, cadde üstü. Dorian karşıda.**
- [x] İlk evren — **Cyberpunk "Neon Altı". Karaborsacı tanıdık, baskınla savaş başlıyor.**
- [x] Her evren kaç oda — **5 oda, sabit sıra (Hades modeli). Oda tipleri evren bazında custom.**
- [x] Multiverse cihazı — **Aldric'in kendisi üretmiş. Restoran depo odasında çalışıyor.**
- [x] Lena multiverse'ü biliyor mu — **Hayır. İlk gece öğreniyor. Şok + merak.**
- [x] Lena neden savaşabiliyor — **Aldric ona bir cihaz/silah veriyor: "al bunu, ihtiyacın olursa kullan."**
- [x] Savaş rolleri — **Aldric: yakın mesafe brawler. Lena: şifacı + uzak mesafe nişancı.**
- [x] Ölüm mekanığı — **Yapay zeka klonlama/ışınlama. %50 malzeme kaybı. Gündüz oluyor, run bitti.**
- [x] İlerleme sistemi — **Kartlar + Restoran. Ayrı level/skill tree YOK. Yemek buff'ları savaşı etkiler.**
- [x] Protagonist'in "her şeyi bilmesi"nin arkasında bir geçmiş var mı? — **Evet, ama evren evren parça parça açılıyor.**
- [x] Kamera sistemi — **Keşif: yakın/atmosferik. Savaş: uzak/taktik. Smooth geçiş. Restoran: keşif kamerası.**
- [x] Grid sistemi — **Otomatik oluşturma: haritayı tarar, yükseklik/engel otomatik tespit, boyut odaya göre değişir.**
- [x] Savaşa geçiş — **Aynı mekânda (BG3 gibi). Sahne değişmiyor, mod switch + kamera geçişi.**
- [x] Keşif kontrolü — **Aldric ana kontrol, diğerleri otomatik takip. Yaklaş + tıkla etkileşim. Gamepad: prompt sistemi.**
- [x] Tur sırası — **Initiative zar sistemi (D&D/BG3). Aldric'e buff — genelde 1-2. sıra.**
- [x] Hareket vs aksiyon — **Hareket bedava (moveRange). Aksiyon puanı (2) sadece kart oynamak için.**
- [x] Kart havuzu — **Ortak havuz, sınıf kısıtlaması kartın üzerinde. Kullanamayan karaktere uyarı.**
- [x] Aktif deck — **Karakter başına maks 6 kart. Son konfigürasyon otomatik önerilir.**
- [x] Kart cooldown — **Tur bazlı cooldown. Bazı kartlar her tur, güçlü kartlar 3-4 turda bir.**
- [x] Portal geçiş akışı — **Tek ekran: deck düzenleme + buff seçimi + onayla → evren.**
- [x] Düşman arketipleri — **Her evrende: yakın saldırgan, uzak mesafe, tank. Tema evrene göre.**
- [x] Yemek pişirme — **Timing mini-game (Cult of the Lamb). Basit tarif 3 timing, zor tarif 5 timing. Hata = yıldız düşer.**
- [x] Restoran evrimi — **Aşçıdan patrona. Başta kendin pişir, büyüdükçe çalışan al, sonra yönet.**
- [x] Envanter erişimi — **Malzeme/tarif/dekorasyon sadece restoranda. Kartlar portal geçişinde.**
- [x] Diyalog sistemi — **İngilizce seslendirme + altyazı. Yakın çekim yok, lip sync gerekmiyor. Karakterler normal pozisyonda. Seçimli diyalog var — bazen 2-3 seçenek çıkar, sonucu etkiler (absürd durumlar, kadro kararları, ödüller).**
- [x] Save sistemi — **Auto save, restoranda dönüş sonrası. Manuel save yok. Tek save slot.**
- [x] Diyalog gösterimi — **Her ikisi: kısa replikler baloncuk, uzun/önemli diyaloglar alt kutu (portre + isim + metin + seçenekler).**
- [x] Seslendirme — **ElevenLabs AI ile yapılacak. Maliyet/zaman riski düşük.**
- [x] Restoran eşya yerleştirme — **PlateUp tarzı grid bazlı fiziksel yerleştirme. Snap to grid. Masa, ocak, tezgah, dekorasyon hepsi aynı sistemle.**
- [x] Restoran büyümesi — **Alan fiziksel olarak büyüyor. Tetikleme: evren bitir (kilit aç) + para öde. İkisi birden lazım.**
- [x] Oyun süresi — **10-12 saat. Evren başına ~2-2.5 saat (3-5 run + gündüzler).**
- [x] Release stratejisi — **Tüm sistemler + 1 evren (Cyberpunk) ile çık (Early Access / yayıncı demo). Sonraki evrenler içerik update olarak eklenir. Sistem kodu bir kere yazılır, sonra sadece içerik.**
- [x] Temizlenen evren — **Kapanır, malzeme limitsiz olur ama müşteri talebi düşer. Eski malzeme değersizleşir, oyuncu yeni evrene itilir.**
- [x] Evren ilerlemesi — **Lineer. Aynı anda 1 evren açık.**
- [ ] Dorian'ın sonu — final boss mu, twist mi, başka bir şey mi?
- [ ] Başkan isim/karikatür detayları
- [ ] Oyunun adı ne? (CLAUDE.md'de TBD)
- [ ] Eşin sosyal zekası oyunun sonunda gerçek bir plot twist açıyor mu?
- [ ] Evren özel mekanikleri — 5 evrenin hepsinde `[TBD]`
- [ ] Yapay zekanın (depo) detayları — kişiliği var mı, upgrade edilebilir mi?
- [ ] Yemek buff sistemi detayları — hangi yemekler, hangi buff'lar?
- [x] Evren seçimi — **Hikaye sıralı, lineer. Aynı anda sadece 1 evren açık. Boss yenince kapanır, sonraki açılır.**
- [x] Temizlenen evrene dönüş — **Hayır. Evren kapanır, malzeme limitsiz olur ama müşteriler o tarifleri talep etmeyi bırakır. Eski malzeme değersizleşir, oyuncu yeni evrene itilir.**

---

## 7. REFERANS MATERYALLER

| Referans              | Ne için                                                                          |
| --------------------- | -------------------------------------------------------------------------------- |
| Rick & Morty          | Protagonist tonu, multiverse yorgunluğu, beklenmedik tehlike anları              |
| Into the Breach       | Combat hissi — her hamlenin ağırlığı, grid okuma                                 |
| Baldur's Gate 3       | Sıra tabanlı grid combat modeli, kart = spell book mantığı, encounter tasarımı   |
| Slay the Spire        | Run loop — kart kazanma tatmini                                                  |
| Cult of the Lamb      | Üs büyüme hissi, iki mod arası geçiş ritmi, ölüm/kayıp modeli                    |
| Hades                 | Sabit oda yapısı (her run aynı odalar), değişen düşman kombinasyonları             |
| High on Life          | Absürd düşman motivasyonları, düşmanların konuşması                              |
| Afterard _(kendi)_    | Kara komedi tonu, ölüm/başarısızlık anlarının yazımı                             |

---

_Son güncelleme: Release stratejisi (tüm sistemler + 1 evren ile çık, sonraki evrenler update), evren ilerlemesi (lineer), temizlenen evren (kapanır, malzeme limitsiz ama talep düşer), save (auto, tek slot), diyalog (seçimli, baloncuk + alt kutu), seslendirme (ElevenLabs), restoran yerleştirme (PlateUp grid bazlı), büyüme (evren kilidi + para) eklendi._
_Bir sonraki adım: Evren özel mekanikleri, yapay zeka detayları, yemek buff sistemi, oyun adı. KOD YAZMAYA HAZIR._
