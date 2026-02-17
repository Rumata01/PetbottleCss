# CSS Kütüphanesi Dosya Hiyerarşisi

Bu hiyerarşi, **Sidebar Yenilemesi**, **UX İyileştirmeleri** ve **Blok Düzeltmeleri** raporlarında belirtilen tüm özellikleri destekleyecek şekilde tasarlanmıştır.

Modüler bir yapı kurarak her parçayı (layout, component, theme) ayrı dosyalarda yönetmenizi sağlar.

## Önerilen Klasör Yapısı (`src/styles/`)

```text
src/styles/
├── main.css                   # Ana giriş dosyası (Tüm importlar burada)
├── base/                      # Temel ayarlar
│   ├── _variables.css         # CSS Değişkenleri (Renkler, Fontlar, Spacing)
│   ├── _reset.css             # CSS Reset
│   └── _typography.css        # Genel yazı tipi ayarları (Settings'den gelenler buraya bağlanacak)
├── themes/                    # Tema tanımları
│   ├── _light.css
│   ├── _dark.css
│   ├── _forest.css
│   ├── _ocean.css
│   └── _sunset.css
├── layout/                    # Sayfa düzeni
│   ├── _app.css               # Ana container grid yapısı
│   ├── _sidebar.css           # Sidebar, Dosya Gezgini, Yeni Butonlar
│   ├── _editor.css            # Editör alanı, genişlik, padding
│   ├── _split-view.css        # [YENİ] Split Screen (PDF/Office viewer için)
│   └── _modals.css            # [YENİ] Settings Modal ve Dialoglar
├── components/                # UI Bileşenleri
│   ├── blocks/                # Editör Blokları
│   │   ├── _base.css          # Ortak blok stilleri
│   │   ├── _text.css          # P, H1-H3
│   │   ├── _lists.css         # Bullet, Numbered, Checkbox
│   │   ├── _code.css          # [GÜNCELLEME] PrismJS/Highlight stilleri
│   │   ├── _quote.css         # [DÜZELTME] Alıntı stilleri (Border fix)
│   │   ├── _callout.css       # Bilgi kutusu
│   │   ├── _toggle.css        # [DÜZELTME] Açılır/Kapanır animasyonları
│   │   └── _divider.css       # [DÜZELTME] Görünür HR stilleri
│   └── ui/                    # Arayüz Elemanları
│       ├── _buttons.css       # Sidebar ve Modal butonları
│       ├── _inputs.css        # Arama ve Form inputları
│       ├── _command-panel.css # Slash menüsü tasarım iyileştirmeleri
│       ├── _context-menu.css  # [YENİ] Drag handle sağ tık menüsü
│       ├── _drag-handle.css   # [İYİLEŞTİRME] Sürükleme kolu hizalaması
│       ├── _toast.css         # Bildirimler
│       └── _file-icons.css    # [YENİ] PDF, DOCX, PPTX ikon stilleri
└── utilities/                 # Yardımcı sınıflar
    ├── _animations.css        # Fade-in, Slide-in efektleri
    └── _helpers.css           # Text-center, flex, hidden vb.
```

---

## Dosya İçerik Detayları ve Gerekçeler

### 1. `base/_variables.css`
Implementasyon raporundaki **Yazı Tipi ve Boyut Ayarları** burada tanımlanacak CSS değişkenleri ile yönetilecek.
```css
:root {
  /* Settings Menüsünden Dinamik Değişecek Değerler */
  --font-main: 'Inter', sans-serif;
  --font-code: 'Fira Code', monospace;
  --font-size-base: 16px;
  --line-height-base: 1.6;
  
  /* Layout */
  --sidebar-width: 260px;
  --header-height: 40px;
}
```

### 2. `layout/_sidebar.css`
**Sidebar Yenilemesi** raporundaki "Ayarlar Butonu" ve "Klasör Seçimi" burada stillendirilecek.
*   `.sidebar-header`: Yeni butonlar için flex yapı.
*   `.file-item`: Sürükle-bırak (Drag & Drop) için hover ve `dragging` durumları.

### 3. `layout/_split-view.css` (YENİ)
**Split Ekran** özelliği için ekranı ikiye bölen grid yapısı.
```css
.app-split-container {
  display: grid;
  grid-template-columns: 1fr 1fr; /* Editör | Önizleme */
  height: 100vh;
}
.preview-pane {
  border-left: 1px solid var(--border-color);
  background: var(--bg-secondary);
}
```

### 4. `layout/_modals.css` (YENİ)
**Ayarlar Menüsü** için.
*   `SettingsModal`: Ortalanmış, backdrop blur efektli modern modal.
*   İçerisinde font seçimi slider'ları ve tema grid'i olacak.

### 5. `components/ui/_drag-handle.css`
**UX İyileştirmeleri** raporundaki "Simetri Sorunu" burada çözülecek.
```css
.block-wrapper {
  display: grid;
  grid-template-columns: 24px 1fr; /* Handle | İçerik */
  align-items: baseline; /* Text ile hizalama */
}
.drag-handle {
  opacity: 0; /* Hover ile gelecek */
  cursor: grab;
  /* Dikey hizalama ayarı */
  transform: translateY(4px); 
}
.block-wrapper:hover .drag-handle {
  opacity: 1;
}
```

### 6. `components/blocks/_code.css`
**Syntax Highlighting** için.
*   PrismJS veya benzeri kütüphanelerin temaları (One Dark, Dracula vb.) buraya entegre edilecek.
*   Dil etiketi (`infoString`) için yeni stil.

### 7. `components/blocks/_quote.css` & `_divider.css`
**Blok Hataları** raporundaki düzeltmeler.
*   `blockquote`: CSS çakışmalarını önlemek için `!important` gerekirse kullanılacak veya spesifik selector yazılacak.
*   `hr`: Yüksekliği `0` değil, `1px` yapılacak ve renk atanacak.
