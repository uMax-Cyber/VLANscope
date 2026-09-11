<div align="center">

[![English](https://img.shields.io/badge/README-English-blue)](README.md)
[![Русский](https://img.shields.io/badge/README-Русский-red)](README.ru.md)
[![Oʻzbekcha](https://img.shields.io/badge/README-Oʻzbekcha-green)](README.uz.md)

</div>

# VLAN muammolarini bartaraf etish qoʻllanmasi
[![CI](https://github.com/uMax-Cyber/VLANscope/actions/workflows/ci.yml/badge.svg)](https://github.com/uMax-Cyber/VLANscope/actions/workflows/ci.yml)


![Namoyish](screenshots/demo.svg)
Koʻp vendorli muhitlarda (UniFi kommutatorlari, Sophos fayrvollari, Linux koʻpriklari) VLAN muammolarini tashxislash boʻyicha amaliy qoʻllanma. Native/tagged nomuvofiqlikni aniqlash, trunk portlarini tekshirish va VLAN-lar orasidagi DHCP muammolarini debugging qilishni qamrab oladi.

## ✨ Imkoniyatlar

- **Native/tagged nomuvofiqlikni aniqlash** — native VLAN untagged trafigiga mos kelmaydigan portlarni topish
- **Trunk tekshiruvi** — trunk orqali qaysi VLAN-lar haqiqatan oʻtishini tasdiqlash
- **VLAN-lararo DHCP debugging** — UDP 67/68 uchun relay va fayrvol qoidalarini tekshirish
- **Koʻp vendorli qamrov** — bir qoʻllanmada UniFi, Sophos va Linux koʻpriklari
- **Tayyor tashxis buyruqlari** — nusxalash uchun tayyor `tcpdump` / `ip` retseptlari

## VLAN-dagi tipik muammolar (yechimlari bilan)

### 1. Native VLAN nomuvofiqligi
**Belgisi**: qurilmalar notoʻgʻri subnetdan IP oladi. Ulanish vaqti-vaqti bilan uziladi.
**Sababi**: kommutator portidagi native VLAN ≠ qurilma kutgan VLAN.
**Yechimi**: barcha portlarni audit qiling — native VLAN shu portdagi untagged trafigiga mos kelishi kerak.

### 2. DHCP VLAN-lar orasidan oʻtmaydi
**Belgisi**: bir VLAN-dagi qurilmalar boshqa VLAN-dagi serverdan DHCP ololmaydi.
**Sababi**: yoki DHCP relay sozlanmagan, yoki fayrvol VLAN-lar orasida UDP 67/68 portlarini bloklaydi.
**Yechimi**: shlyuzdagi relay sozlamasini tekshiring; DHCP portlari uchun fayrvol qoidalarini tekshiring.

### 3. Trunk VLAN teglarini yetkazmaydi
**Belgisi**: baʼzi VLAN-lar ishlaydi, boshqalari yoʻq — bir xil fizik kanalda.
**Sababi**: trunk port barcha kerakli VLAN-larga ruxsat bermaydi (tagged_vlan_mgmt = block_all).
**Yechimi**: trunk portlarda kerakli VLAN-larni aniq ruxsat bering.

### 4. Statik reserv notoʻgʻri scope-da
**Belgisi**: bitta aniq qurilma «connecting...» holatida qotib qoladi, qolganlari ishlaydi.
**Sababi**: qurilmada VLAN-A scope-sida statik DHCP reserv mavjud, lekin u VLAN-B ga ulanadi.
**Yechimi**: reservni toʻgʻri scope-ga koʻchiring (yoki global qiling).

## Tashxis vositalari

```bash
# Trunk-da qaysi VLAN-lar borligini tekshirish
tcpdump -lni <iface> vlan -c 20

# DHCP relay yoʻlini tekshirish
# Shlyuzda: har bir VLAN interfeysi uchun DHCP relay sozlamasini tekshirish

# Muayyan VLAN-dan test (vaqtinchalik subinterfeys yaratish)
ip link add link eth0 name eth0.XXX type vlan id XXX
ip link set eth0.XXX up
dhclient eth0.XXX
```

## Litsenziya
MIT

## 📬 Aloqa

Savollaringiz bormi? Yozing: **[allumaxmail@gmail.com](mailto:allumaxmail@gmail.com)**

---

<div align="center">

[![English](https://img.shields.io/badge/README-English-blue)](README.md)
[![Русский](https://img.shields.io/badge/README-Русский-red)](README.ru.md)
[![Oʻzbekcha](https://img.shields.io/badge/README-Oʻzbekcha-green)](README.uz.md)

</div>
