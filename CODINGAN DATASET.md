# DATASET-MENOLAK-USANG
Codingan Dataset untuk artikel *Menolak Usang: Rahasia Utama Mengapa Musik Dewa 19, Sheila on 7, dan Peterpan Bertahan Sangat Lama*.

## 1. Dari Terbentuk sampai Album Pertama
```python
import pandas as pd
import matplotlib.pyplot as plt

# 1. DATA (sumber: Wikipedia)
data = {
    "Band": ["Dewa 19", "Sheila on 7", "Peterpan / Noah"],
    "Terbentuk": [1986, 1996, 2000],
    "Album pertama": [1992, 1999, 2003]
}
warna_band = {"Dewa 19": "blue", "Sheila on 7": "green", "Peterpan / Noah": "red"}

# 2. MEMBUAT TABEL DAN MENGHITUNG SELISIH
df = pd.DataFrame(data)
df["Selisih (tahun)"] = df["Album pertama"] - df["Terbentuk"]
warna = [warna_band.get(b, "gray") for b in df["Band"]]

print("TABEL TAHUN TERBENTUK DAN ALBUM PERTAMA")
display(df)

# 3. MEMBUAT GRAFIK
fig, ax = plt.subplots(figsize=(10, 1.8 + 0.9 * len(df)))

for i, baris in df.iterrows():
    ax.plot([baris["Terbentuk"], baris["Album pertama"]], [i, i],
            color=warna[i], linewidth=3)
    ax.plot(baris["Terbentuk"], i, "o", color=warna[i], markersize=10)
    ax.plot(baris["Album pertama"], i, "s", color=warna[i], markersize=10)
    ax.text(baris["Terbentuk"], i + 0.18, baris["Terbentuk"], ha="center", fontsize=11)
    ax.text(baris["Album pertama"], i + 0.18, baris["Album pertama"], ha="center", fontsize=11)

ax.plot([], [], "ko", label="Band terbentuk")
ax.plot([], [], "ks", label="Album pertama")

# 4. JUDUL DAN LABEL
ax.set_title("Dari Terbentuk sampai Album Pertama", fontsize=14)
ax.set_xlabel("Tahun", fontsize=12)
ax.set_yticks(range(len(df)))
ax.set_yticklabels(df["Band"], fontsize=11)
ax.set_ylim(-0.5, len(df) - 0.3)
ax.set_xlim(df["Terbentuk"].min() - 3, df["Album pertama"].max() + 5)
ax.legend(loc="lower right")
ax.grid(alpha=0.4)

# 5. MENAMPILKAN GRAFIK
plt.tight_layout()
plt.show()
```

## 2. Album Baru dan Peristiwa Penting
```python
import pandas as pd
import matplotlib.pyplot as plt

# 1. DATA (sumber: Wikipedia; reuni 2018 dari Medium)
# Album studio saja, tanpa OST, kompilasi, dan album rekaman ulang
album = {
    "Dewa 19": [1992, 1994, 1995, 1997, 2000, 2002, 2004, 2006, 2007],
    "Sheila on 7": [1999, 2000, 2002, 2004, 2006, 2008, 2011, 2014],
    "Peterpan / Noah": [2003, 2004, 2007, 2012, 2019]
}

peristiwa = [
    ("Dewa 19", 2011, "Bubar (2011)"),
    ("Dewa 19", 2018, "Reuni (2018)"),
    ("Sheila on 7", 2018, "Single (2018)"),
    ("Sheila on 7", 2024, "Single (2024)"),
    ("Sheila on 7", 2026, "Single (2026)"),
    ("Peterpan / Noah", 2012, "Jadi Noah (2012)"),
    ("Peterpan / Noah", 2024, "Hiatus (2024)")
]

warna_band = {"Dewa 19": "blue", "Sheila on 7": "green", "Peterpan / Noah": "red"}

# 2. MEMBUAT TABEL DATA
df_peristiwa = pd.DataFrame(peristiwa, columns=["Band", "Tahun", "Peristiwa"])
band = list(album)
baris_y = {b: len(band) - 1 - i for i, b in enumerate(band)}   # band pertama di atas
semua_tahun = [t for ys in album.values() for t in ys] + list(df_peristiwa["Tahun"])

print("TABEL PERISTIWA PENTING")
display(df_peristiwa)

# 3. MEMBUAT GRAFIK
fig, ax = plt.subplots(figsize=(10, 2.2 + 1.3 * len(band)))

for b, tahun in album.items():
    ax.plot(tahun, [baris_y[b]] * len(tahun), "o-", color=warna_band.get(b, "gray"),
            linewidth=1.5, markersize=8, label=f"Album baru: {b}")

for i, (b, tahun, nama) in enumerate(peristiwa):
    y = baris_y[b]
    ax.plot(tahun, y, "k^", markersize=10)
    ax.annotate(nama, (tahun, y), xytext=(0, 12 + 12 * (i % 2)),
                textcoords="offset points", ha="center", fontsize=10)

ax.plot([], [], "k^", label="Peristiwa penting")

# 4. JUDUL DAN LABEL
ax.set_title(f"Album Baru dan Peristiwa Penting, {min(semua_tahun)}–{max(semua_tahun)}", fontsize=14)
ax.set_xlabel("Tahun", fontsize=12)
ax.set_yticks(list(baris_y.values()))
ax.set_yticklabels(list(baris_y.keys()), fontsize=11)
ax.set_ylim(-0.9, len(band) - 0.2)
ax.set_xlim(min(semua_tahun) - 2, max(semua_tahun) + 5)
ax.legend(loc="lower left", ncol=2)
ax.grid(alpha=0.4)

# 5. MENAMPILKAN GRAFIK
plt.tight_layout()
plt.show()
```

## 3. Jumlah Album dan Jeda Rata-rata
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 1. DATA TAHUN RILIS ALBUM STUDIO (sumber: Wikipedia)
album = {
    "Dewa 19": [1992, 1994, 1995, 1997, 2000, 2002, 2004, 2006, 2007],
    "Sheila on 7": [1999, 2000, 2002, 2004, 2006, 2008, 2011, 2014],
    "Peterpan / Noah": [2003, 2004, 2007, 2012, 2019]
}
warna_band = {"Dewa 19": "blue", "Sheila on 7": "green", "Peterpan / Noah": "red"}

# 2. MENGHITUNG JUMLAH ALBUM DAN JEDA RATA-RATA
baris = []
for band, tahun in album.items():
    jeda = np.diff(sorted(tahun))
    baris.append({
        "Band": band,
        "Jumlah album": len(tahun),
        "Jeda rata-rata (tahun)": round(jeda.mean(), 1)
    })

df = pd.DataFrame(baris)
warna = [warna_band.get(b, "gray") for b in df["Band"]]

print("TABEL JUMLAH ALBUM DAN JEDA RATA-RATA")
display(df)

# 3. MEMBUAT GRAFIK
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 6))

bar1 = ax1.bar(df["Band"], df["Jumlah album"], color=warna, width=0.55)
ax1.set_title("Jumlah Album Studio", fontsize=13)
ax1.set_ylabel("Album", fontsize=12)
ax1.set_ylim(0, df["Jumlah album"].max() * 1.2)
ax1.bar_label(bar1, fontsize=11, padding=2)

bar2 = ax2.bar(df["Band"], df["Jeda rata-rata (tahun)"], color=warna, width=0.55)
ax2.set_title("Jeda Rata-rata Antar Album", fontsize=13)
ax2.set_ylabel("Tahun", fontsize=12)
ax2.set_ylim(0, df["Jeda rata-rata (tahun)"].max() * 1.2)
ax2.bar_label(bar2, labels=[f"{v:.1f}".replace(".", ",") for v in df["Jeda rata-rata (tahun)"]], fontsize=11, padding=2)

for ax in (ax1, ax2):
    ax.tick_params(axis="x", labelsize=10)
    ax.set_axisbelow(True)
    ax.grid(axis="y", alpha=0.4)

# 4. MENAMPILKAN GRAFIK
plt.tight_layout()
plt.show()
```

## 4. Album Terlaris Tiap Band
```python
import pandas as pd
import matplotlib.pyplot as plt

# 1. DATA ALBUM TERLARIS (sumber: Wikipedia, minimal)
data = {
    "Band": ["Peterpan", "Dewa 19", "Sheila on 7"],
    "Album": ["Bintang di Surga (2004)", "Bintang Lima (2000)", "Kisah Klasik untuk Masa Depan (2000)"],
    "Terjual (juta kopi)": [3.2, 1.7, 1.7]
}
warna_band = {"Peterpan": "red", "Dewa 19": "blue", "Sheila on 7": "green"}

# 2. MEMBUAT TABEL DATA
df = pd.DataFrame(data)
warna = [warna_band.get(b, "gray") for b in df["Band"]]

print("TABEL ALBUM TERLARIS TIAP BAND")
display(df)

# 3. MEMBUAT GRAFIK
fig, ax = plt.subplots(figsize=(10, 6))

nama = df["Band"] + "\n" + df["Album"].str.wrap(24)
bar = ax.bar(nama, df["Terjual (juta kopi)"], color=warna, width=0.5)

# 4. JUDUL DAN LABEL
ax.set_title("Album Terlaris Tiap Band", fontsize=14)
ax.set_ylabel("Juta kopi terjual", fontsize=12)
ax.set_ylim(0, df["Terjual (juta kopi)"].max() * 1.2)
ax.tick_params(axis="x", labelsize=10)
ax.set_axisbelow(True)
ax.grid(axis="y", alpha=0.4)
ax.bar_label(bar, labels=[f"{v:.1f} juta".replace(".", ",") for v in df["Terjual (juta kopi)"]], fontsize=11, padding=3)

# 5. MENAMPILKAN GRAFIK
plt.tight_layout()
plt.show()
```

## 5. Cara Menghadapi Perubahan
```python
import pandas as pd
import matplotlib.pyplot as plt

# 1. DATA (sumber: Wikipedia, Medium)
data = {
    "Band": ["Dewa 19", "Sheila on 7", "Peterpan / Noah"],
    "Perubahan": [
        "Ganti vokalis, bubar 2011",
        "Personel berganti, band jalan terus",
        "Jadi Noah 2012, hiatus 2024"
    ],
    "Hasil": [
        "Reuni 2018, tampil lagi di panggung",
        "Single baru 2018, 2024, 2026",
        "Album 2012 terjual > 1 juta kopi"
    ]
}

# 2. MEMBUAT TABEL DATA
df = pd.DataFrame(data)

print("TABEL CARA MENGHADAPI PERUBAHAN")
display(df)

# 3. MEMBUAT TABEL GAMBAR
fig, ax = plt.subplots(figsize=(10, 1.4 + 0.6 * len(df)))
ax.axis("off")

tabel = ax.table(
    cellText=df.values,
    colLabels=df.columns,
    loc="center",
    cellLoc="left"
)
tabel.auto_set_font_size(False)
tabel.set_fontsize(12)
tabel.auto_set_column_width(list(range(len(df.columns))))
tabel.scale(1, 2.2)

# 4. JUDUL
ax.set_title("Cara Tiap Band Menghadapi Perubahan", fontsize=14)

# 5. MENAMPILKAN GAMBAR
plt.tight_layout()
plt.show()
```

## 6. Kota dalam Satu Rangkaian Konser
```python
import pandas as pd
import matplotlib.pyplot as plt

# 1. DATA KONSER (sumber: GNFI, Wikipedia)
data = {
    "Band": ["Dewa 19", "Sheila on 7", "Peterpan"],
    "Rangkaian konser": [
        "Tur 2022 (30 kota + Kuala Lumpur)",
        "Tur Juli–September 2024",
        "Rekor MURI 24 jam, 18 Juli 2004"
    ],
    "Jumlah kota": [31, 5, 6]
}
warna_band = {"Dewa 19": "blue", "Sheila on 7": "green", "Peterpan": "red"}

# 2. MEMBUAT TABEL DATA
df = pd.DataFrame(data)
warna = [warna_band.get(b, "gray") for b in df["Band"]]

print("TABEL RANGKAIAN KONSER")
display(df)

# 3. MEMBUAT GRAFIK
fig, ax = plt.subplots(figsize=(10, 6))

nama = df["Band"] + "\n" + df["Rangkaian konser"].str.wrap(26)
bar = ax.bar(nama, df["Jumlah kota"], color=warna, width=0.5)

# 4. JUDUL DAN LABEL
ax.set_title("Kota yang Didatangi dalam Satu Rangkaian Konser", fontsize=14)
ax.set_ylabel("Jumlah kota", fontsize=12)
ax.set_ylim(0, df["Jumlah kota"].max() * 1.15)
ax.tick_params(axis="x", labelsize=10)
ax.set_axisbelow(True)
ax.grid(axis="y", alpha=0.4)
ax.bar_label(bar, fontsize=11, padding=3)

# 5. MENAMPILKAN GRAFIK
plt.tight_layout()
plt.show()
```

## 7. Statistik Spotify
```python
import pandas as pd
import matplotlib.pyplot as plt

# 1. DATA SPOTIFY (sumber: Musicmetricsvault, 23–24 Oktober 2026)
# Kolom "Grup": profil dengan grup sama ditumpuk jadi satu batang pada grafik total putaran
data = {
    "Profil": ["Sheila on 7", "Dewa 19", "Noah", "Peterpan"],
    "Grup": ["Sheila on 7", "Dewa 19", "Peterpan + Noah", "Peterpan + Noah"],
    "Pendengar bulanan (juta)": [11.9, 10.5, 8.6, 2.8],
    "Total putaran (miliar)": [3.5, 2.7, 1.7, 1.4]
}
warna_profil = {"Sheila on 7": "green", "Dewa 19": "blue", "Noah": "red", "Peterpan": "black"}

# 2. MEMBUAT TABEL DATA
df = pd.DataFrame(data)
warna = [warna_profil.get(p, "gray") for p in df["Profil"]]

print("TABEL STATISTIK SPOTIFY")
display(df)

# 3. MEMBUAT GRAFIK
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 6),
                               gridspec_kw={"width_ratios": [1.25, 1]})

bar1 = ax1.bar(df["Profil"], df["Pendengar bulanan (juta)"], color=warna, width=0.6)
ax1.set_title("Pendengar Bulanan", fontsize=13)
ax1.set_ylabel("Juta pendengar", fontsize=12)
ax1.set_ylim(0, df["Pendengar bulanan (juta)"].max() * 1.2)
ax1.bar_label(bar1, labels=[f"{v:.1f}".replace(".", ",") for v in df["Pendengar bulanan (juta)"]], fontsize=11, padding=2)

# Total putaran: batang per grup, ditumpuk per profil
grup = list(dict.fromkeys(df["Grup"]))
total = df.groupby("Grup")["Total putaran (miliar)"].sum()
bawah = {g: 0 for g in grup}
for _, baris in df.iterrows():
    nilai = baris["Total putaran (miliar)"]
    banyak = (df["Grup"] == baris["Grup"]).sum()
    ax2.bar(baris["Grup"].replace(" + ", "\n+ "), nilai, bottom=bawah[baris["Grup"]],
            color=warna_profil.get(baris["Profil"], "gray"), width=0.6,
            label=baris["Profil"] if banyak > 1 else None)
    bawah[baris["Grup"]] += nilai
for i, g in enumerate(grup):
    ax2.text(i, total[g] + total.max() * 0.015, f"{total[g]:.1f}".replace(".", ","),
             ha="center", fontsize=11)
ax2.set_title("Total Putaran", fontsize=13)
ax2.set_ylabel("Miliar putaran", fontsize=12)
ax2.set_ylim(0, total.max() * 1.2)
if ax2.get_legend_handles_labels()[0]:
    ax2.legend(loc="upper right")

for ax in (ax1, ax2):
    ax.tick_params(axis="x", labelsize=10)
    ax.set_axisbelow(True)
    ax.grid(axis="y", alpha=0.4)

# 4. MENAMPILKAN GRAFIK
plt.tight_layout()
plt.show()
```

## 8. Lagu Paling Banyak Diputar
```python
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.patches import Patch

# 1. DATA LAGU (sumber: Musicmetricsvault, 23–24 Oktober 2026)
# Urutkan per band; jumlah lagu per band boleh berbeda
data = {
    "Band": ["Sheila on 7"] * 3 + ["Dewa 19"] * 3 + ["Noah"] * 3 + ["Peterpan"] * 3,
    "Lagu": [
        "Dan...", "Bila Kau Tak Disampingku", "Seberapa Pantas",
        "Kangen", "Aku Milikmu", "Separuh Nafas",
        "Separuh Aku", "Menunggumu", "Bintang Di Surga",
        "Menghapus Jejakmu", "Mungkin Nanti", "Yang Terdalam"
    ],
    "Putaran (juta)": [
        546.2, 268.0, 251.3,
        393.2, 340.3, 253.7,
        330.6, 120.2, 87.6,
        141.5, 110.7, 84.6
    ]
}
warna_band = {"Sheila on 7": "green", "Dewa 19": "blue", "Noah": "red", "Peterpan": "black"}

# 2. MEMBUAT TABEL DATA
df = pd.DataFrame(data)

print("TABEL LAGU TERPOPULER TIAP BAND")
display(df)

# 3. MEMBUAT GRAFIK BATANG HORIZONTAL
fig, ax = plt.subplots(figsize=(10, 2.5 + 0.35 * len(df)))

posisi = []
y = 0
band_sebelumnya = df["Band"].iloc[0]
for _, baris in df.iterrows():
    if baris["Band"] != band_sebelumnya:   # jarak antar band
        y -= 0.5
        band_sebelumnya = baris["Band"]
    ax.barh(y, baris["Putaran (juta)"], color=warna_band.get(baris["Band"], "gray"), height=0.7)
    ax.text(baris["Putaran (juta)"] + df["Putaran (juta)"].max() * 0.01, y,
            f'{baris["Putaran (juta)"]:.1f}'.replace(".", ","), va="center", fontsize=10)
    posisi.append(y)
    y -= 1

# 4. JUDUL DAN LABEL
ax.set_yticks(posisi)
ax.set_yticklabels(df["Lagu"], fontsize=10)
ax.set_xlim(0, df["Putaran (juta)"].max() * 1.15)
ax.set_title("Lagu Paling Banyak Diputar Tiap Band", fontsize=14)
ax.set_xlabel("Juta putaran", fontsize=12)
ax.legend(handles=[Patch(color=warna_band.get(b, "gray"), label=b) for b in dict.fromkeys(df["Band"])],
          loc="lower right")
ax.set_axisbelow(True)
ax.grid(axis="x", alpha=0.4)

# 5. MENAMPILKAN GRAFIK
plt.tight_layout()
plt.show()
```

