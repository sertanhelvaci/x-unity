# MULTIVERSE RESTAURANT — Game Project Plan

> Claude Code için kapsamlı proje rehberi. Bu dosyayı oku ve tüm kararları buna göre ver.

---

## 1. PROJE ÖZETI

**Oyun Adı:** (TBD)
**Platform Hedefi:** Steam (önce) → PS5 → Xbox
**Engine:** Unity 6 LTS — Universal Render Pipeline (URP)
**Ekip:** 2 kişi — 1 Programmer, 1 Artist
**Tür:** Roguelike + Sıra Tabanlı Grid Combat + Shop Management

### Karakterler

| Kod Adı  | İsim       | Rol                                                       |
| -------- | ---------- | --------------------------------------------------------- |
| `PROTO`  | **Aldric** | Protagonist. Her şeyi görmüş, hiçbir şey onu şaşırtmıyor. |
| `SPOUSE` | **Lena**   | Eş, sabit partner. Yanlış şeylere vicdan azabı çekiyor.   |
| `RIVAL`  | **Dorian** | Rakip şef. Unvanını kaybetmemek için her şeyi yapıyor.    |
| `POTUS`  | (TBD)      | ABD Başkanı. Adrenalinden geliyor, biz kurtarıyoruz.      |

> Karakter detayları, diyalog tonu ve hikaye kararları için `.claude/DESIGN.md` dosyasını oku.

### Tek Cümle Pitch

Gündüzleri başka evrenlerde topladığın malzemeleri restoranında satan, geceleri ise o evrenlerin tehlikeli bölgelerine grid tabanlı sıra sıra savaşarak giren bir multiverse roguelike.

### Referans Oyunlar

- **Cult of the Lamb** → Kalıcılık modeli + üs büyüme hissi + ölüm/kayıp modeli
- **Into the Breach** → Grid combat: pozisyon bazlı taktik
- **Baldur's Gate 3** → Sıra tabanlı grid combat, kart = spell book mantığı (kalıcı, büyüyen), keşif modu (click to move serbest hareket)
- **Hades** → Sabit oda yapısı (her run aynı 5 oda, değişen düşman kombinasyonları), level design odaklı
- **Slay the Spire** → Kart kazanma tatmini
- **Rick & Morty** → Yazarlık tonu: absürd, kara komedi, multiverse kaosunda sıradan insan

---

## 2. PLATFORM STRATEJİSİ

### Yol Haritası

```
AŞAMA 1          AŞAMA 2          AŞAMA 3
────────         ────────         ────────
Steam            PS5              Xbox
PC/Mac/Linux     Developer hesabı Developer hesabı
Steamworks SDK   Sony sertifikasyon Microsoft sertifikasyon
En az bürokratik
İlk gelir & feedback buradan gelir
```

### Platform Başvuru Gereksinimleri

- **Steam:** Steamworks hesabı, $100 tek seferlik ücret. En hızlı yol.
- **PS5:** PlayStation Partner hesabı, Sony'ye başvuru, sertifikasyon testi (TRC).
- **Xbox:** ID@Xbox programı, Microsoft onayı, sertifikasyon testi (XR).

> **Not:** PS5 ve Xbox başvurularını Steam'de oyun stabilize olunca başlat.
> Sertifikasyon süreçleri aylarca sürebilir, erken başvurmak avantaj.

### Platform Başına Teknik Gereksinimler

| Gereksinim                | Steam              | PS5            | Xbox              |
| ------------------------- | ------------------ | -------------- | ----------------- |
| Controller desteği        | Opsiyonel          | Zorunlu        | Zorunlu           |
| UI controller navigasyonu | Opsiyonel          | Zorunlu        | Zorunlu           |
| Cloud save                | Steam Cloud        | PS Cloud       | Xbox Cloud        |
| Achievement sistemi       | Steam Achievements | PS Trophies    | Xbox Achievements |
| Çözünürlük desteği        | Esnek              | 4K/60fps hedef | 4K/60fps hedef    |
| HDR                       | Opsiyonel          | Bekleniyor     | Bekleniyor        |

---

## 3. CROSS-PLATFORM MİMARİ KURALLARI

> **Claude Code Notu:** Bunlar pazarlık konusu değil. Her satır kod bu kurallara göre yazılacak.

### 3.1 Input Sistemi — Unity New Input System (Zorunlu)

**Asla** `Input.GetKeyDown`, `Input.GetMouseButton` gibi eski API kullanma.
Her şey Unity'nin **New Input System** paketi üzerinden gidecek.

```csharp
// Input Actions Asset yapısı (.inputactions dosyası)
// Editörde oluşturulacak, kod generate edilecek

// PlayerInputActions (auto-generated)
// ├── Exploration
// │   ├── Move            (Mouse: Click to move / Controller: Sol Analog)
// │   ├── Interact        (Mouse: Click / Controller: A butonu)
// │   └── Cancel          (Keyboard: Esc / Controller: B butonu)
// ├── Combat
// │   ├── SelectCell      (Mouse: Click / Controller: A butonu)
// │   ├── CancelSelection (Mouse: RightClick / Controller: B butonu)
// │   ├── EndTurn         (Keyboard: Space / Controller: Y butonu)
// │   └── NavigateCards   (Mouse: Scroll / Controller: L/R Bumper)
// └── UI
//     ├── Navigate        (Mouse / Controller: D-Pad veya Sol Analog)
//     ├── Submit          (Mouse: Click / Controller: A)
//     └── Cancel          (Keyboard: Esc / Controller: B)
```

```csharp
// LocalPlayerController.cs — Input System ile
using UnityEngine.InputSystem;

public class LocalPlayerController : MonoBehaviour, IPlayerController
{
    private PlayerInputActions inputActions;

    private void Awake()
    {
        inputActions = new PlayerInputActions();
    }

    private void OnEnable()
    {
        inputActions.Combat.SelectCell.performed += OnSelectCell;
        inputActions.Combat.EndTurn.performed += OnEndTurn;
        inputActions.Enable();
    }

    private void OnDisable()
    {
        inputActions.Combat.SelectCell.performed -= OnSelectCell;
        inputActions.Combat.EndTurn.performed -= OnEndTurn;
        inputActions.Disable();
    }

    private void OnSelectCell(InputAction.CallbackContext ctx)
    {
        // Mouse veya controller — fark etmez, aynı kod
        var command = new GameCommand
        {
            Type = CommandType.Move,
            TargetCell = GetTargetCellFromInput()
        };
        SubmitCommand(command);
    }

    private void OnEndTurn(InputAction.CallbackContext ctx)
    {
        SubmitCommand(new GameCommand { Type = CommandType.EndTurn });
    }

    public void SubmitCommand(GameCommand command)
    {
        EventBus.Publish(new PlayerCommandEvent { Command = command });
    }

    public void OnTurnStarted(PlayerUnit unit) { /* UI güncelle */ }
}
```

### 3.2 UI Controller Navigasyonu

PS5 ve Xbox'ta fare yok. Tüm UI controller ile gezilecek.

```csharp
// Tüm UI elemanları Unity'nin EventSystem'ını kullanacak
// Her buton, kart, menü elemanı Selectable component'ine sahip olacak

// CardHandUI.cs — Controller navigasyonu ile
public class CardHandUI : MonoBehaviour
{
    [SerializeField] private CardUI[] cards;

    private void OnEnable()
    {
        // Controller'da L/R Bumper ile kartlar arasında geçiş
        inputActions.Combat.NavigateCards.performed += OnNavigateCards;

        // İlk kartı otomatik seç (controller için)
        if (cards.Length > 0)
            EventSystem.current.SetSelectedGameObject(cards[0].gameObject);
    }

    private void OnNavigateCards(InputAction.CallbackContext ctx)
    {
        float direction = ctx.ReadValue<float>();
        // Seçili kartı sola/sağa kaydır
    }
}
```

### 3.3 Platform Soyutlama Katmanı

Her platformun achievement, save, overlay sistemi farklı. Bunları doğrudan çağırma.

```csharp
// IPlatformService.cs — Interface
public interface IPlatformService
{
    void UnlockAchievement(string achievementId);
    void SaveToCloud(string data);
    string LoadFromCloud();
    void ShowPlatformOverlay();
}

// SteamPlatformService.cs
public class SteamPlatformService : IPlatformService
{
    public void UnlockAchievement(string achievementId)
    {
        // Steamworks.NET ile
        // SteamUserStats.SetAchievement(achievementId);
    }
    // ...
}

// PSPlatformService.cs — PS5 SDK ile doldurulacak (ileride)
public class PSPlatformService : IPlatformService { /* ... */ }

// XboxPlatformService.cs — GDK ile doldurulacak (ileride)
public class XboxPlatformService : IPlatformService { /* ... */ }

// PlatformManager.cs — Hangi platform? Otomatik seçer
public class PlatformManager : MonoBehaviour
{
    public static IPlatformService Service { get; private set; }

    private void Awake()
    {
        #if UNITY_PS5
            Service = new PSPlatformService();
        #elif UNITY_GAMECORE
            Service = new XboxPlatformService();
        #else
            Service = new SteamPlatformService();
        #endif
    }
}
```

### 3.4 Çözünürlük & Ekran Yönetimi

```csharp
// ResolutionManager.cs
public class ResolutionManager : MonoBehaviour
{
    // Hedef çözünürlükler
    // Steam: esnek, oyuncu seçer
    // PS5/Xbox: 3840x2160 (4K) veya 1920x1080 (1080p) — platform kontrol eder

    private void Awake()
    {
        // UI Scale — tüm platformlarda düzgün görünsün
        // Canvas Scaler: Scale With Screen Size
        // Reference Resolution: 1920x1080
        // Match: 0.5 (width/height dengesi)
    }
}
```

> **Eşe Not:** Tüm UI elemanları 1920x1080 referans çözünürlükte tasarlanacak.
> Canvas Scaler otomatik scale eder. 4K'da da, 1080p'de de güzel görünür.

### 3.5 Save Sistemi — Çok Katmanlı

```csharp
// SaveSystem.cs — Platform agnostic
public static class SaveSystem
{
    private static string LocalSavePath =>
        System.IO.Path.Combine(Application.persistentDataPath, "save.json");

    public static void Save(SaveData data)
    {
        var json = JsonUtility.ToJson(data, prettyPrint: true);

        // Her zaman locale kaydet
        File.WriteAllText(LocalSavePath, json);

        // Platform cloud save (varsa)
        PlatformManager.Service?.SaveToCloud(json);
    }

    public static SaveData Load()
    {
        // Önce cloud'u dene, yoksa locale bak
        var cloudData = PlatformManager.Service?.LoadFromCloud();
        if (!string.IsNullOrEmpty(cloudData))
            return JsonUtility.FromJson<SaveData>(cloudData);

        if (!File.Exists(LocalSavePath)) return new SaveData();
        return JsonUtility.FromJson<SaveData>(File.ReadAllText(LocalSavePath));
    }
}
```

---

## 4. CORE GAMEPLAY LOOP

```
┌──────────────────────────────────────────────────────────────┐
│                    GÜNLÜK DÖNGÜ (1 gün = 1 run + 1 gündüz)  │
│                                                              │
│   GÜNDÜZ PHASE                 GECE PHASE                    │
│   ────────────                 ───────────                   │
│   Restoran aç                  Depo odasından portal aç      │
│   Yemek yap & sat → PARA   ◄── Evrene ışınlan               │
│   Restoran genişlet            5 oda: keşif + grid combat    │
│   Dekorasyon al                Savaş dışı: BG3 serbest hrkт │
│   Yemek buff'ı hazırla         Savaşta: grid sıra tabanlı   │
│   NPC'lerle diyalog            Düşman öldür → KART KAZAN     │
│   Yeni evren seç               Loot topla → MALZEME          │
│                                Boss yendim? → Tüm loot al    │
│                                Öldüm? → %50 malzeme kayıp    │
└──────────────────────────────────────────────────────────────┘
```

### Para & Güçlenme Döngüsü

```
GECE                          GÜNDÜZ                    TEKRAR GECEYE
─────                         ──────                    ──────────────
Kart kazan (spell book)  →    Malzemeyle yemek yap →    Daha iyi kartlar
Malzeme topla            →    Sat → para kazan     →    Yemek buff'larıyla gir
                              Restoran geliştir         Daha büyük restoran
                              Dekorasyon al             Daha fazla müşteri
                              Buff yemek hazırla        Daha fazla para

Güçlenme 1: Kartlar (evrenden) — BG3 spell book, kalıcı
Güçlenme 2: Yemek buff'ları (restorandan) — geçici, run başı
Ayrı level/skill tree YOK.
```

### Kalıcılık Modeli

```
KALICI — Hiç silinmez                  RUN DATA — Gece bitince sıfır
──────────────────────                 ──────────────────────────────
Restoran seviyesi & upgrades           Şu anki oda index'i
Restoran dekorasyonu & genişleme       Karakterlerin mevcut HP'si
Açılmış karakterler                    Bu geceye ait yemek buff'ları
Kazanılmış kartlar (karakter bazlı)    Oda içi henüz toplanmamış loot
Toplam malzeme envanteri
Para / currency
Hangi evrenler boss'u temizlendi
Açık menü tarifleri
```

### Ölüm & Respawn

- Restoranda yapay zeka var — karakterleri anlık yedekliyor
- Ölünce yapay zeka restoranda geri ışınlıyor (klon ama anılar/beyin kaldığı yerden)
- Malzemelerin **%50'si** kurtarılır, geri kalanı kayıp
- Boss'u yenersen **tüm kazançları** götürürsün
- Ölünce gündüz oluyor — run bitti, restoran işletmeye başla
- Ertesi gece **1. odadan** başla (roguelike klasiği)

### Evren Yapısı

- Her evren **5 sabit oda** — Hades modeli, her run aynı sıra
- Oda tipleri: savaş, keşif/diyalog, boss — **evren bazında custom sıralama**
- `UniverseData` ScriptableObject'inde oda tipi ve sırası editörden ayarlanır
- Odalar sabit ama düşman kombinasyonları, loot dropları, diyalog satırları değişir
- İlk evren: **Cyberpunk "Neon Altı"**
- Evren seçimi **hikaye sıralı** — mevcut evreni bitirmeden (boss yenmeden) sonraki açılmaz

### Kamera Sistemi

- **Keşif modu:** Karaktere yakın, atmosferik. 2.5D izometrik.
- **Savaş modu:** Kamera yukarı çeker, taktik görüş. Grid tamamı görünür.
- **Restoran:** Keşif kamerası ile aynı — yakın, 2.5D izometrik.
- **Geçiş:** Smooth lerp — savaş tetiklenince kamera yavaşça yukarı çeker, bitince geri.

> **Claude Code Notu:** `CameraController` iki mod destekler: `CameraMode.Exploration` ve `CameraMode.Combat`. Geçiş smooth.

### Grid Sistemi — Otomatik Oluşturma

- Grid boyutu **odaya göre değişir** — sabit 8x6 değil
- Sistem mevcut haritayı **otomatik tarar**: yükseklik, engeller, duvarlar
- Belirli yükseklikten büyük alanlar otomatik engel/disabled grid olur
- Level designer sadece odayı tasarlar, grid kendini şekillendirir
- `GridManager` artık `width/height` parametresi almak yerine sahneyi analiz eder

> **Claude Code Notu:** `GridManager.InitializeGrid()` yerine `GridManager.ScanAndBuildGrid()` — raycast veya collider tabanlı harita tarama. `GridCell.IsWalkable` otomatik belirlenir.

### Savaş Geçişi — Aynı Mekân (BG3 modeli)

- Savaş tetiklenince sahne DEĞİŞMİYOR — aynı mekânda mod switch
- Kamera yukarı çeker, grid overlay belirir, sıra tabanlı başlar
- Savaş bitince grid kaybolur, kamera yakınlaşır, keşfe devam
- Ayrı savaş sahnesi / yükleme ekranı YOK

### Savaş Mekanikleri

**Initiative (Tur Sırası):**
- Her savaş başında her karakter için **zar atılır** (D&D/BG3 initiative)
- En yüksek zar önce oynar
- Aldric'e **initiative buff** — genelde 1. veya 2. sırada
- `CharacterData`'ya `initiativeBonus` alanı ekle

**Hareket & Aksiyon Ayrımı (BG3 modeli):**
- Hareket **bedava** — `moveRange` kadar yürü, aksiyon puanı harcamaz
- Aksiyon puanı (2) **sadece kart oynamak** için
- Bir turda hem yürü hem 2 kart oyna

**Düşman Arketipleri (her evrende):**
- Yakın mesafe saldırgan — sana koşar, vurur
- Uzak mesafe — uzaktan ateş, kaçar
- Tank/kalkan — yavaş, dayanıklı, arkadakileri koruyor
- Tema evrene göre değişir

### Keşif Modu

- Aldric **ana kontrol** — oyuncu sadece Aldric'i yönetir
- Yardımcı karakterler (Lena + diğerleri) **otomatik takip**
- Savaşta her karakteri **sırayla kontrol** et
- **Mouse/Keyboard:** Tıkla → yürü → varınca etkileşim menüsü
- **Gamepad:** Sol analog ile yürü → menzilde prompt → A bas
- Input Action Map: `Exploration` (serbest hareket) + `Combat` (grid) + `UI`

---

## 5. RESTORAN SİSTEMİ (GÜNDÜZ PHASE)

### 5.1 Yemek Pişirme Mekanığı — Timing Mini-Game

```
TARİF SEÇ → TIMING OYUNU → SONUÇ
═══════════════════════════════════

Basit tarif: 3 timing check
Zor tarif:   5 timing check

Her timing: bar ilerliyor, doğru alanda bas
Başarılı → ★ kazanırsın
Kaçırdın → yıldız düşer

Sonuç:
5/5 ★ → mükemmel yemek, müşteri çok memnun, yüksek fiyat
3/5 ★ → orta yemek, normal fiyat
0/5 ★ → yemek çöp, malzeme boşa gitti
```

> **Claude Code Notu:** `CookingMinigame` sistemi: tarif zorluğuna göre timing sayısı, `RecipeData`'da `timingCount` alanı.

### 5.2 Restoran Evrimi — Aşçıdan Patrona

```
SEVİYE 1 — AŞÇI MODU (Başlangıç)
══════════════════════════════════
Aldric pişiriyor (timing mini-game), Lena servis yapıyor
Az müşteri, az tarif
Oyuncu her şeyi kendisi yapıyor
4 masa, küçük mutfak

SEVİYE 2 — HİBRİT
══════════════════
Çalışanlar alıyorsun — aşçı, garson
Çalışanlar basit tarifleri hallediyor (otomatik)
Sen sadece zor/özel tarifleri yapıyorsun
Artan boş zamanında NPC diyalogları başlıyor
8 masa, büyük mutfak, outdoor alan

SEVİYE 3 — PATRON MODU
═══════════════════════
Mutfağa girmiyorsun, çalışanlar yapıyor
Kavgalar, müşteri şikayetleri, Başkan ziyareti
Hikaye restoran diyaloglarından taşınıyor
Menü/personel/strateji kararları
16 masa, profesyonel mutfak, 2. kat
```

> **Tasarım notu:** Mekanik yük azaldıkça hikaye yükü artıyor. Gündüz phase asla sıkmaz.

### 5.3 Yemek Buff Sistemi

Restoranda hazırlanan yemekler savaş öncesi geçici buff verir. Tarife + malzemeye bağlı.

```
YEMEK BUFF ÖRNEKLERİ (detaylar TBD)
─────────────────────────────────────
"Ejderha Biftek"   → bu gece +2 HP
"Neon Ramen"       → bu gece +1 aksiyon puanı
"Goblin Fermente"  → bu gece +1 hareket menzili
```

> **Claude Code Notu:** `RecipeData`'ya buff alanları ekle. Buff sistemi portal geçiş ekranında seçilir, run başında uygulanır, run sonunda sıfırlanır.

### 5.4 Dekorasyon Sistemi

Salt görsel — mekanik etkisi yok. Slot tipleri: Duvar, Zemin, Masa Üstü, Köşe, Dış Cephe.
Her evrenin exclusive dekorasyonları var — boss yenince açılır.

### 5.5 Müşteri NPC Diyalogları

Gündüz phase'inde tetiklenir. Rick & Morty tonu. İlk ziyaret / memnun / memnun değil diyalogları.
Restoran büyüdükçe daha önemli NPC'ler gelir — Başkan dahil. Hikaye burada derinleşir.

### 5.6 Depo Odası & Yapay Zeka

Restoranda arka tarafta depo odası var:
- Multiverse cihazı burada çalışıyor (portal açma)
- Yapay zeka burada — klonlama/yedekleme sistemi
- Restoran upgrade'leri yapay zeka üzerinden yapılır
- İlerlemeye göre kilitler açılır (Diablo 4 / Cult of the Lamb kademeli açılma)

### 5.7 Envanter Erişimi

- **Malzemeler, tarifler, dekorasyonlar:** Sadece restoranda erişilebilir
- **Kart deck düzenleme + buff seçimi:** Portal geçişinde tek ekranda

---

## 6. KART SİSTEMİ & KARAKTER GELİŞİMİ

### Kart Kazanma

- Düşman öldürünce %40 drop şansı
- Loot odalarında garanti 3 seçenek, 1 tanesini seç
- Kazanılan kartlar **kalıcı** — ölünce silinmez

### Kart Havuzu & Deck Sistemi

- Kart havuzu **ortak** — drop olan kart herhangi bir karaktere verilebilir
- Kartlarda **sınıf kısıtlaması** var — bazı kartlar sadece belirli karakterler kullanabilir
- Kart üzerine gelinince hangi sınıflar kullanabileceğini gösterir
- Kullanamayacak karaktere verilirse uyarı: "Aldric bunu kullanamaz"
- Ortak kartlar da var — herkes kullanabilir
- Her karakter için **maksimum 6 kart** aktif deck'te
- Son deck konfigürasyonu otomatik önerilir (her gece sıfırdan seçme yok)
- Deck düzenleme + buff seçimi portal geçişinde **tek ekranda** yapılır

### Kart Cooldown

- Her kartın **tur bazlı cooldown'u** var
- Bazı kartlar her tur oynanabilir (cooldown: 0)
- Güçlü kartlar 3-4 turda bir oynanabilir
- `CardData`'ya `cooldownTurns` alanı ekle

### Kart Tipleri

| Tip         | Açıklama                     |
| ----------- | ---------------------------- |
| Temel       | Başlangıç kartları           |
| Nadir       | Run içinde kazanılan         |
| Evren Kartı | Sadece o evrende drop olur   |
| Pasif       | Savaş başında otomatik aktif |

### Karakter Savaş Rolleri

| Karakter | Rol                   | Pozisyon        | Açıklama                                              |
| -------- | --------------------- | --------------- | ----------------------------------------------------- |
| Aldric   | Yakın mesafe brawler  | Ön cephe        | Düşmana yapışır, yüksek hasar verir                   |
| Lena     | Şifacı + Nişancı     | Arka destek     | Uzaktan hasar + iyileştirme. Korunması lazım.         |

> **Tasarım notu:** Pozisyon hatası yaparsan Lena'ya düşman ulaşır — gerilim buradan gelir. Into the Breach / BG3 taktik hissi.

### Karakter Gelişim Sistemi

> **Ayrı level/skill tree YOK.** Güçlenme iki kaynaktan gelir:
> 1. **Kartlar** — BG3 spell book. Evrenden kazanılır, kalıcı. Deck büyüdükçe seçenek artar.
> 2. **Yemek buff'ları** — Restorandan. Geçici, run başı. Restoran geliştikçe daha güçlü buff'lar.
>
> **Claude Code Notu:** `CharacterData`'da level/XP/skill tree alanı YAPMA. Kart deck'i ve yemek buff'ları yeterli.

---

## 7. SAVAŞ EKİBİ

Mimari **2-4 karakter** destekler. `List<PlayerUnit>` kullan, hardcode etme.

### Sabit Kadro

- **Aldric** (`PROTO`) — Baştan itibaren her zaman ekipte.
- **Lena** (`SPOUSE`) — Baştan itibaren her zaman ekipte. Aldric'in partneri.

### Dinamik Kadro

Run içinde ve hikaye ilerledikçe geçici veya kalıcı karakterler eklenir.

- **Başkan** (`POTUS`) — Geçici. Belirli evrenlerde katılır, 6-7 evren boyunca zaman zaman görünür.
- Evrenlerden devşirilen diğer karakterler — `[TBD hikayeye göre]`

> **Claude Code Notu:** `TurnManager` ve `CombatManager` her zaman `List<PlayerUnit>` ile çalışır.
> Aldric ve Lena özel cased değil — aynı `PlayerUnit` sistemini kullanır.

---

## 8. MULTİPLAYER STRATEJİSİ

Single-player, ama co-op mimarisi hazır. `IPlayerController` interface'i şimdiden var.

---

## 9. TEKNİK MİMARİ

### 9.1 Katmanlı Yapı

```
┌────────────────────────────────────────────┐
│           PRESENTATION LAYER               │
│   CardUI, HUD, ShopUI, DialogUI,           │
│   RestaurantUI, DecorationUI, CameraRig    │
├────────────────────────────────────────────┤
│            GAME LOGIC LAYER                │
│   CombatManager, ShopManager,              │
│   RestaurantManager, DecorationManager,    │
│   RunManager, EnemyAI, TurnManager         │
├────────────────────────────────────────────┤
│              DATA LAYER                    │
│   ScriptableObjects, SaveSystem,           │
│   EventBus, GameStateManager               │
├────────────────────────────────────────────┤
│           PLATFORM LAYER                   │
│   PlatformManager, IPlatformService,       │
│   SteamService, PSService, XboxService     │
└────────────────────────────────────────────┘
```

### 9.2 Klasör Yapısı

```
Assets/
├── _Game/
│   ├── Scripts/
│   │   ├── Core/
│   │   │   ├── GameManager.cs
│   │   │   ├── GameStateManager.cs
│   │   │   ├── EventBus.cs
│   │   │   ├── GameCommand.cs
│   │   │   └── Constants.cs
│   │   ├── Platform/                      ← YENİ
│   │   │   ├── IPlatformService.cs
│   │   │   ├── PlatformManager.cs
│   │   │   ├── SteamPlatformService.cs
│   │   │   ├── PSPlatformService.cs       ← Şimdilik stub
│   │   │   └── XboxPlatformService.cs     ← Şimdilik stub
│   │   ├── Input/                         ← YENİ
│   │   │   ├── PlayerInputActions.cs      ← Auto-generated
│   │   │   └── InputManager.cs
│   │   ├── Grid/
│   │   │   ├── GridManager.cs
│   │   │   ├── GridCell.cs
│   │   │   ├── GridVisualizer.cs
│   │   │   └── PathfindingService.cs
│   │   ├── Combat/
│   │   │   ├── TurnManager.cs
│   │   │   ├── CombatManager.cs
│   │   │   ├── CombatResolver.cs
│   │   │   └── AbilityHandler.cs
│   │   ├── Cards/
│   │   │   ├── CardSystem.cs
│   │   │   ├── CardData.cs
│   │   │   ├── CardDeck.cs
│   │   │   ├── CardRewardSystem.cs
│   │   │   ├── CardRegistry.cs
│   │   │   ├── CardEffectBase.cs
│   │   │   └── CardEffects/
│   │   ├── Characters/
│   │   │   ├── CharacterBase.cs
│   │   │   ├── PlayerUnit.cs
│   │   │   ├── EnemyUnit.cs
│   │   │   └── CharacterData.cs
│   │   ├── Controllers/
│   │   │   ├── IPlayerController.cs
│   │   │   ├── LocalPlayerController.cs   ← New Input System kullanır
│   │   │   └── AIController.cs
│   │   ├── Enemies/
│   │   │   ├── EnemyAIBase.cs
│   │   │   └── Strategies/
│   │   ├── Run/
│   │   │   ├── RunManager.cs
│   │   │   ├── RoomGenerator.cs
│   │   │   ├── UniverseData.cs
│   │   │   └── RoomData.cs
│   │   ├── Restaurant/
│   │   │   ├── ShopManager.cs
│   │   │   ├── RestaurantManager.cs
│   │   │   ├── DecorationManager.cs
│   │   │   ├── RecipeSystem.cs
│   │   │   ├── RestaurantData.cs
│   │   │   ├── DecorationData.cs
│   │   │   ├── RecipeData.cs
│   │   │   ├── IngredientData.cs
│   │   │   └── CustomerNPCData.cs
│   │   ├── Dialogue/
│   │   │   ├── DialogueManager.cs
│   │   │   ├── DialogueData.cs
│   │   │   └── DialogueTrigger.cs
│   │   ├── Persistence/
│   │   │   ├── SaveSystem.cs              ← Cloud save destekli
│   │   │   ├── SaveData.cs
│   │   │   └── RunData.cs
│   │   └── UI/
│   │       ├── CardHandUI.cs              ← Controller navigasyonu var
│   │       ├── CardRewardUI.cs
│   │       ├── CharacterHUD.cs
│   │       ├── ShopUI.cs
│   │       ├── RestaurantUI.cs
│   │       ├── DecorationUI.cs
│   │       ├── ResolutionManager.cs       ← YENİ
│   │       └── DialogueUI.cs
│   │
│   ├── Input/                             ← YENİ
│   │   └── PlayerInputActions.inputactions ← Unity Input System asset
│   │
│   ├── ScriptableObjects/
│   │   ├── Cards/
│   │   ├── Characters/
│   │   ├── Enemies/
│   │   ├── Ingredients/
│   │   ├── Recipes/
│   │   ├── Decorations/
│   │   ├── Restaurant/
│   │   └── Universes/
│   │
│   ├── Prefabs/
│   │   ├── Characters/
│   │   ├── Enemies/
│   │   ├── Grid/
│   │   ├── Decorations/
│   │   ├── Restaurant/
│   │   ├── VFX/
│   │   └── UI/
│   │
│   └── Scenes/
│       ├── MainMenu.unity
│       ├── DayPhase.unity
│       ├── NightPhase.unity
│       └── TestCombat.unity
│
└── Synty/
```

---

## 10. CORE SİSTEMLER — KOD

### 10.1 EventBus

```csharp
public static class EventBus
{
    private static readonly Dictionary<Type, List<Delegate>> listeners = new();

    public static void Subscribe<T>(Action<T> listener)
    {
        var type = typeof(T);
        if (!listeners.ContainsKey(type)) listeners[type] = new List<Delegate>();
        listeners[type].Add(listener);
    }

    public static void Unsubscribe<T>(Action<T> listener)
    {
        var type = typeof(T);
        if (listeners.ContainsKey(type)) listeners[type].Remove(listener);
    }

    public static void Publish<T>(T eventData)
    {
        var type = typeof(T);
        if (!listeners.ContainsKey(type)) return;
        foreach (var listener in listeners[type])
            (listener as Action<T>)?.Invoke(eventData);
    }
}

// Tüm event tipleri
public struct EnemyDiedEvent            { public EnemyUnit Enemy; }
public struct TurnStartedEvent          { public CharacterBase Character; }
public struct CardPlayedEvent           { public CardData Card; public GridCell Target; }
public struct CardRewardAvailableEvent  { public List<CardData> Options; public CharacterBase ForCharacter; }
public struct CardAddedToDeckEvent      { public CardData Card; public CharacterBase Character; }
public struct IngredientCollectedEvent  { public IngredientData Ingredient; }
public struct PhaseChangedEvent         { public GamePhase NewPhase; }
public struct RoomClearedEvent          { public RoomData Room; }
public struct BossDefeatedEvent         { public UniverseData Universe; }
public struct CharacterDamagedEvent     { public CharacterBase Character; public int Amount; }
public struct HandUpdatedEvent          { public List<CardData> Cards; }
public struct RoundStartedEvent         { public int RoundNumber; }
public struct RevenueEarnedEvent        { public int Amount; }
public struct RestaurantExpandedEvent   { public int NewLevel; }
public struct DecorationPlacedEvent     { public DecorationData Decoration; }
public struct PlayerCommandEvent        { public GameCommand Command; }
public struct DialogueStartedEvent      { }
public struct DialogueEndedEvent        { }
public struct DialogueLineEvent         { public DialogueLine Line; }
public struct AchievementUnlockedEvent  { public string AchievementId; }
```

### 10.2 GameCommand & Input

```csharp
// GameCommand.cs
public enum CommandType { Move, PlayCard, EndTurn }

public struct GameCommand
{
    public CommandType Type;
    public Vector2Int TargetCell;
    public CardData Card;
    public int PlayerId;
}

// IPlayerController.cs
public interface IPlayerController
{
    void OnTurnStarted(PlayerUnit unit);
    void SubmitCommand(GameCommand command);
}

// LocalPlayerController.cs — New Input System
using UnityEngine.InputSystem;

public class LocalPlayerController : MonoBehaviour, IPlayerController
{
    private PlayerInputActions inputActions;

    private void Awake() => inputActions = new PlayerInputActions();

    private void OnEnable()
    {
        inputActions.Combat.SelectCell.performed += OnSelectCell;
        inputActions.Combat.EndTurn.performed += OnEndTurn;
        inputActions.Enable();
    }

    private void OnDisable()
    {
        inputActions.Combat.SelectCell.performed -= OnSelectCell;
        inputActions.Combat.EndTurn.performed -= OnEndTurn;
        inputActions.Disable();
    }

    private void OnSelectCell(InputAction.CallbackContext ctx)
    {
        // Mouse veya controller — aynı kod, platform fark etmez
        EventBus.Publish(new PlayerCommandEvent
        {
            Command = new GameCommand { Type = CommandType.Move }
        });
    }

    private void OnEndTurn(InputAction.CallbackContext ctx) =>
        SubmitCommand(new GameCommand { Type = CommandType.EndTurn });

    public void SubmitCommand(GameCommand command) =>
        EventBus.Publish(new PlayerCommandEvent { Command = command });

    public void OnTurnStarted(PlayerUnit unit) { }
}
```

### 10.3 GridSystem

```csharp
// GridCell.cs
public class GridCell
{
    public int X { get; private set; }
    public int Y { get; private set; }
    public bool IsWalkable { get; set; } = true;
    public CharacterBase OccupyingCharacter { get; set; }
    public GameObject VisualObject { get; set; }

    public GridCell(int x, int y) { X = x; Y = y; }
    public bool IsOccupied => OccupyingCharacter != null;
    public Vector2Int Position => new Vector2Int(X, Y);
    public int DistanceTo(GridCell other) =>
        Mathf.Max(Mathf.Abs(X - other.X), Mathf.Abs(Y - other.Y));
}

// GridManager.cs
public class GridManager : MonoBehaviour
{
    public static GridManager Instance { get; private set; }

    [SerializeField] private int width = 8;
    [SerializeField] private int height = 6;
    [SerializeField] private float cellSize = 1f;
    [SerializeField] private GameObject cellPrefab;

    private GridCell[,] grid;

    private void Awake()
    {
        if (Instance != null) { Destroy(gameObject); return; }
        Instance = this;
        InitializeGrid();
    }

    private void InitializeGrid()
    {
        grid = new GridCell[width, height];
        for (int x = 0; x < width; x++)
        for (int y = 0; y < height; y++)
        {
            grid[x, y] = new GridCell(x, y);
            var obj = Instantiate(cellPrefab, GridToWorld(x, y), Quaternion.identity, transform);
            grid[x, y].VisualObject = obj;
        }
    }

    public Vector3 GridToWorld(int x, int y) => new Vector3(x * cellSize, 0, y * cellSize);

    public GridCell GetCell(int x, int y)
    {
        if (x < 0 || x >= width || y < 0 || y >= height) return null;
        return grid[x, y];
    }

    public GridCell GetCell(Vector2Int pos) => GetCell(pos.x, pos.y);

    public List<GridCell> GetReachableCells(GridCell origin, int moveRange)
    {
        var result = new List<GridCell>();
        for (int x = 0; x < width; x++)
        for (int y = 0; y < height; y++)
        {
            var cell = grid[x, y];
            if (cell == origin || !cell.IsWalkable || cell.IsOccupied) continue;
            if (origin.DistanceTo(cell) <= moveRange) result.Add(cell);
        }
        return result;
    }

    public List<GridCell> GetCellsInRange(GridCell origin, int range)
    {
        var result = new List<GridCell>();
        for (int x = 0; x < width; x++)
        for (int y = 0; y < height; y++)
        {
            var cell = grid[x, y];
            if (cell != origin && origin.DistanceTo(cell) <= range) result.Add(cell);
        }
        return result;
    }
}
```

### 10.4 CharacterBase & CardDeck

```csharp
// CharacterBase.cs
public abstract class CharacterBase : MonoBehaviour
{
    [SerializeField] protected CharacterData data;

    public int CurrentHP { get; protected set; }
    public int MaxHP => data.maxHP;
    public int MoveRange => data.moveRange;
    public int ActionPoints { get; protected set; }
    public bool IsAlive => CurrentHP > 0;
    public GridCell CurrentCell { get; private set; }
    public CharacterData Data => data;

    protected virtual void Awake() => CurrentHP = MaxHP;

    public void PlaceOnCell(GridCell cell)
    {
        if (CurrentCell != null) CurrentCell.OccupyingCharacter = null;
        CurrentCell = cell;
        cell.OccupyingCharacter = this;
        transform.position = GridManager.Instance.GridToWorld(cell.X, cell.Y);
    }

    public void TakeDamage(int amount)
    {
        CurrentHP = Mathf.Max(0, CurrentHP - amount);
        EventBus.Publish(new CharacterDamagedEvent { Character = this, Amount = amount });
        if (!IsAlive) OnDeath();
    }

    public void Heal(int amount) => CurrentHP = Mathf.Min(MaxHP, CurrentHP + amount);

    public virtual void OnTurnStart()
    {
        ActionPoints = data.maxActionPoints;
        EventBus.Publish(new TurnStartedEvent { Character = this });
    }

    protected virtual void OnDeath() => CurrentCell.OccupyingCharacter = null;
    public abstract void StartTurn();
}

// CardDeck.cs
public class CardDeck : MonoBehaviour
{
    private List<CardData> permanentDeck = new();
    public IReadOnlyList<CardData> PermanentDeck => permanentDeck;

    public void InitializeWithStartingCards(List<CardData> startingCards) =>
        permanentDeck = new List<CardData>(startingCards);

    public void InitializeFromSave(List<string> savedCardIds)
    {
        permanentDeck = new List<CardData>();
        foreach (var id in savedCardIds)
        {
            var card = CardRegistry.Instance.GetCard(id);
            if (card != null) permanentDeck.Add(card);
        }
    }

    /// <summary>
    /// Run içinde düşmandan veya loot'tan kart kazanıldığında çağrılır. Kalıcıdır.
    /// </summary>
    public void AddCard(CardData card)
    {
        permanentDeck.Add(card);
        EventBus.Publish(new CardAddedToDeckEvent { Card = card, Character = GetComponent<CharacterBase>() });
    }

    public List<CardData> GetCombatHand() => new List<CardData>(permanentDeck);
    public List<string> GetCardIdsForSave() => permanentDeck.ConvertAll(c => c.cardId);
}
```

### 10.5 TurnManager

```csharp
public class TurnManager : MonoBehaviour
{
    public static TurnManager Instance { get; private set; }

    private List<CharacterBase> turnOrder = new();
    private int currentIndex = 0;

    public CharacterBase ActiveCharacter => turnOrder.Count > 0 ? turnOrder[currentIndex] : null;
    public PlayerUnit LastActingPlayer { get; private set; }
    public int RoundNumber { get; private set; } = 1;

    private void Awake() => Instance = this;

    /// <summary>
    /// Combat başında çağrılır. 2-4 karakter listesini destekler.
    /// </summary>
    public void InitializeCombat(List<CharacterBase> allCharacters)
    {
        turnOrder = new List<CharacterBase>(allCharacters);
        currentIndex = 0;
        RoundNumber = 1;
        StartCurrentTurn();
    }

    public void EndCurrentTurn()
    {
        currentIndex++;
        if (currentIndex >= turnOrder.Count)
        {
            currentIndex = 0;
            RoundNumber++;
            EventBus.Publish(new RoundStartedEvent { RoundNumber = RoundNumber });
        }

        turnOrder.RemoveAll(c => !c.IsAlive);
        if (turnOrder.Count == 0) return;
        if (currentIndex >= turnOrder.Count) currentIndex = 0;

        StartCurrentTurn();
    }

    private void StartCurrentTurn()
    {
        if (ActiveCharacter is PlayerUnit p) LastActingPlayer = p;
        ActiveCharacter.OnTurnStart();
        ActiveCharacter.StartTurn();
    }

    public void RemoveCharacter(CharacterBase character)
    {
        int idx = turnOrder.IndexOf(character);
        turnOrder.Remove(character);
        if (idx < currentIndex) currentIndex--;
        if (currentIndex >= turnOrder.Count) currentIndex = 0;
    }
}
```

### 10.6 SaveData (Güncellenmiş)

```csharp
[Serializable]
public class SaveData
{
    public List<string> unlockedCharacterIds = new();
    public List<string> clearedUniverses = new();
    public List<string> unlockedUniverses = new() { "universe_001" };
    public List<string> persistentInventoryIds = new();
    public SerializableDictionary<string, List<string>> characterCardDecks = new();
    public RestaurantState restaurantState = new();
}

[Serializable]
public class RestaurantState
{
    public int currentLevel = 1;
    public int currency = 0;
    public List<string> placedDecorationIds = new();
    public List<string> unlockedRecipeIds = new();
}
```

---

## 11. SCRIPTABLEOBJECT ŞEMALARI

```csharp
[CreateAssetMenu(menuName = "Game/CharacterData")]
public class CharacterData : ScriptableObject
{
    public string characterId;
    public string characterName;
    public Sprite portrait;
    public int maxHP = 10;
    public int moveRange = 3;
    public int maxActionPoints = 2;
    public int baseDamage = 3;
    public List<CardData> startingCards;
    public List<CardData> possibleCardDrops;
    // public List<CardUnlockEntry> cardUnlockProgression; // TASARIM BEKLİYOR
}

public enum CardRarity { Common, Rare, UniverseSpecific, Passive }
public enum CardTargetType { SingleEnemy, SingleAlly, Self, Area, EmptyCell }
public enum CardEffectType { Damage, Heal, Move, Buff, Debuff, Special }

[CreateAssetMenu(menuName = "Game/CardData")]
public class CardData : ScriptableObject
{
    public string cardId;
    public string cardName;
    [TextArea] public string description;
    public Sprite artwork;
    public CardRarity rarity;
    public int actionPointCost = 1;
    public CardTargetType targetType;
    public int range = 1;
    public int aoeRadius = 0;
    public CardEffectType effectType;
    public int effectValue = 5;
    public string universeOrigin;
    public GameObject vfxPrefab;
}

[CreateAssetMenu(menuName = "Game/UniverseData")]
public class UniverseData : ScriptableObject
{
    public string universeId;
    public string universeName;
    [TextArea] public string flavourText;
    public Sprite universeIcon;
    public Material skyboxMaterial;
    public List<RoomData> rooms;
    public List<IngredientData> possibleIngredients;
    public List<CardData> universeExclusiveCards;
    public List<RecipeData> universeExclusiveRecipes;
    public List<DecorationData> universeExclusiveDecorations;
    public DialogueData introDialogue;
}

[CreateAssetMenu(menuName = "Game/RestaurantData")]
public class RestaurantData : ScriptableObject
{
    public List<RestaurantLevel> levels;
}

[Serializable]
public class RestaurantLevel
{
    public int level;
    public int upgradeCost;
    public int customerCapacity;
    public float revenueMultiplier;
    public List<DecorationSlotType> availableSlotTypes;
    public GameObject levelPrefab;
}

[CreateAssetMenu(menuName = "Game/DecorationData")]
public class DecorationData : ScriptableObject
{
    public string decorationId;
    public string decorationName;
    [TextArea] public string flavourText;
    public Sprite thumbnail;
    public GameObject prefab;
    public DecorationSlotType slotType;
    public int purchaseCost;
    public string universeOrigin;
}

public enum DecorationSlotType { Wall, Floor, TableTop, Corner, Exterior }
```

---

## 12. GELİŞTİRME AŞAMALARI (ROADMAP)

### Faz 0 — Proje Kurulumu (1 Hafta)

- [ ] Unity 6 LTS + URP kurulumu
- [ ] **New Input System** paketini kur ve `.inputactions` asset'ini oluştur
- [ ] Klasör yapısını kur
- [ ] EventBus yaz ve test et
- [ ] GameStateManager yaz
- [ ] GameCommand + IPlayerController yaz
- [ ] PlatformManager + IPlatformService stub'larını yaz
- [ ] Git repo kur — `main` / `develop` branch

### Faz 1 — Grid Combat Core (6-8 Hafta)

- [ ] GridManager + GridCell + GridVisualizer
- [ ] CharacterBase + PlayerUnit + EnemyUnit
- [ ] LocalPlayerController (**New Input System** ile)
- [ ] TurnManager (2-4 karakter listesi)
- [ ] CardData + CardDeck + CardSystem
- [ ] CardHandUI (controller navigasyonlu)
- [ ] AggressiveAI
- [ ] **MILESTONE:** Controller ve mouse/keyboard ile combat çalışıyor

### Faz 2 — Kart Kazanma (2-3 Hafta)

- [ ] CardRewardSystem + CardRewardUI (controller navigasyonlu)
- [ ] CardRegistry
- [ ] **MILESTONE:** Düşman öldür → kart seç → bir sonraki oda

### Faz 3 — Combat Polish (3-4 Hafta)

- [ ] AoE hedefleme, buff/debuff
- [ ] Animasyon + VFX
- [ ] **MILESTONE:** Bir oda başından sonuna polish'li

### Faz 4 — Run Loop (4-5 Hafta)

- [ ] RunManager + RoomGenerator
- [ ] Ölünce kalıcılık
- [ ] Boss odası
- [ ] **MILESTONE:** 3 odalı evren tamamlanabiliyor

### Faz 5 — Restoran & Gündüz (4-6 Hafta)

- [ ] RestaurantManager (genişleme)
- [ ] DecorationManager + DecorationUI (controller navigasyonlu)
- [ ] RecipeSystem + ShopManager
- [ ] CustomerNPC diyalogları
- [ ] Gündüz ↔ Gece geçişi
- [ ] **MILESTONE:** Tam döngü çalışıyor, restoran büyüyor

### Faz 6 — Narrative (Paralel)

- [ ] DialogueManager + DialogueUI
- [ ] İlk 3 evren diyalogu
- [ ] Müşteri NPC diyalogları

### Faz 7 — Steam Çıkışı Hazırlığı

- [ ] SteamPlatformService (Steamworks.NET entegrasyonu)
- [ ] Steam Achievements
- [ ] Steam Cloud Save
- [ ] ResolutionManager + grafik ayarları menüsü
- [ ] Tam controller desteği testi
- [ ] SaveSystem cloud entegrasyonu

### Faz 8 — Konsol Port Hazırlığı (Steam sonrası)

- [ ] PSPlatformService (PS5 SDK)
- [ ] XboxPlatformService (GDK)
- [ ] PS Trophies / Xbox Achievements
- [ ] Konsol sertifikasyon testleri (TRC / XR)
- [ ] 4K çözünürlük testi
- [ ] HDR desteği

---

## 13. SYNTY ASSET KULLANIM KURALLARI

**Eş İçin Kurallar:**

1. `Synty/` klasörüne dokunma. `_Game/Prefabs/` altına kopyala.
2. Her restoran seviyesi için ayrı prefab.
3. Her evren için ayrı atmosfer: skybox, ışık rengi, tile paleti.
4. Dekorasyon prefabları `_Game/Prefabs/Decorations/` altında.
5. **Kamera:** xRotation 45°, yRotation 45° — 2.5D izometrik.
6. **UI:** Canvas Scaler → Scale With Screen Size → 1920x1080 referans. 4K ve 1080p otomatik scale eder.

| Kullanım                 | Synty Paketi                            |
| ------------------------ | --------------------------------------- |
| Restoran iç mekan        | Polygon Modular Fantasy Interior        |
| Cyberpunk evreni         | Polygon City + Sci-Fi iç mekan          |
| Fantezi / Dungeon evreni | Polygon Dungeon + Fantasy Kingdom       |
| Sci-Fi evreni            | Polygon Sci-Fi Space                    |
| Apocalypse evreni        | Polygon Apocalypse / Wasteland          |
| Goblin evreni            | Polygon Sci-Fi Space + Fantasy karışımı |

---

## 14. KOD YAZARKEN GENEL KURALLAR

1. **Asla** `Input.GetKeyDown` / `Input.GetMouseButton` kullanma — New Input System
2. Sistemler birbirine doğrudan referans vermez — EventBus
3. Platform servisleri `IPlatformService` üzerinden — direkt Steamworks çağırma
4. Tüm veri tanımları ScriptableObject
5. Oyuncu aksiyonları GameCommand struct'ına sarılır
6. Ekip boyutu hardcode değil — `List<PlayerUnit>`
7. `FindFirstObjectByType` kullan (Unity 6)
8. `OnEnable`/`OnDisable`'da EventBus subscribe/unsubscribe
9. Her public method XML comment
10. Tüm UI elemanları `Selectable` component'i ile controller navigasyonuna açık

---

## 15. CLAUDE CODE İÇİN ÖZEL NOTLAR

- **Unity 6 LTS + URP**
- **New Input System zorunlu** — eski Input API yasak
- **IPlatformService** — Steam/PS/Xbox direkt çağrısı yok, interface üzerinden
- **Referanslar:** Cult of the Lamb, Into the Breach, Baldur's Gate 3 (combat + spell book modeli), Slay the Spire, Rick & Morty tonu
- **Kart felsefesi:** BG3 spell book mantığı — kartlar kalıcı, deck büyüyor ama aksiyon puanı sınırlı. Gerilim kartlardan değil encounter tasarımından gelir.
- **Karakterler:** Aldric (`PROTO`) + Lena (`SPOUSE`) sabit. Diğerleri `List<PlayerUnit>` ile dinamik.
- **Co-op hazır mimari** — `IPlayerController` interface'i her zaman
- **Kartlar kalıcı** — `CardDeck`, run sonunda silinmez
- **Karakter gelişim:** Level/skill tree YOK. Güçlenme = kartlar + yemek buff. `CharacterData`'da XP/level alanı yapma.
- **Savaş rolleri:** Aldric = yakın mesafe brawler, Lena = şifacı + uzak mesafe nişancı
- **Evren yapısı:** 5 sabit oda (Hades modeli), oda sırası evren bazında custom, `UniverseData`'da editörden ayarlanır
- **İki mod:** Keşif (BG3 serbest hareket, click to move) + Combat (grid sıra tabanlı). Aynı mekânda geçiş, sahne değişmez.
- **Kamera:** Keşif = yakın/atmosferik. Savaş = uzak/taktik. Smooth geçiş. Restoran = keşif kamerası.
- **Grid:** Otomatik oluşturma — haritayı tarar, engeller/yükseklik otomatik tespit. Boyut odaya göre değişir.
- **Initiative:** Zar sistemi (D&D/BG3). Aldric'e buff. `CharacterData.initiativeBonus`
- **Hareket:** Bedava (moveRange). Aksiyon puanı sadece kart oynamak için.
- **Kart deck:** Ortak havuz, sınıf kısıtlaması var. Karakter başına maks 6 aktif kart. Cooldown sistemi.
- **Portal ekranı:** Tek ekranda deck düzenleme + buff seçimi + onayla.
- **Yemek pişirme:** Timing mini-game (Cult of the Lamb). Basit 3, zor 5 timing. Hata = yıldız düşer.
- **Restoran evrimi:** Aşçıdan patrona. Başta kendin pişir, büyüdükçe çalışan al, sonra yönet + hikaye.
- **Ölüm:** Yapay zeka klonlama, %50 malzeme kaybı, 1. odadan başla. Boss yenersen tüm loot
- **Depo odası:** Portal cihazı + yapay zeka + upgrade'ler burada
- **Envanter:** Malzeme/tarif/dekorasyon sadece restoranda. Kart deck portal ekranında.
- **UI:** controller navigasyonu her menüde, EventSystem kullan
- **Canvas:** Scale With Screen Size, 1920x1080 referans
- Kod açıklamalarını **Türkçe** yaz
- **Evrenler:** Cyberpunk, Fantezi/Dungeon, Sci-Fi, Apocalypse, Goblin — detaylar DESIGN.md'de
- **Ton:** Diyalog yazarken Aldric kısa/alaycı, Lena uzun/vicdan azabı çeken. Detay için DESIGN.md oku.

## 16. TASARIM BİBLE

- Hikaye, karakter, evren ve ton kararları için `.claude/DESIGN.md` dosyasını oku.
- Kod kararlarında "bu hikayeyle uyumlu mu?" sorusunu bu dosyaya bakarak cevapla.
- **Karakter adları:** Aldric, Lena, Dorian — kod içinde string olarak bu isimler kullanılır.
- **Diyalog yazarken:** Aldric'in sesi kısa/alaycı, Lena'nın sesi uzun/vicdan azabı çeken. DESIGN.md Bölüm 4'e bak.
- **Evren ScriptableObject'leri oluştururken:** DESIGN.md Bölüm 2'deki evren kimliklerini ve restoran bağlantılarını referans al.

---

_Son güncelleme: Kamera sistemi (keşif/savaş/restoran), grid otomatik oluşturma, BG3 aynı mekân savaş geçişi, initiative zar sistemi, hareket bedava + aksiyon kart için, kart havuzu (ortak + sınıf kısıtlaması + 6 kart limit + cooldown), portal tek ekran hazırlık, yemek pişirme timing mini-game, restoran aşçıdan patrona evrimi, keşif modu (Aldric kontrol + takip sistemi + gamepad desteği), envanter erişim kuralları eklendi._
