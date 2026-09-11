<div align="center">

[![English](https://img.shields.io/badge/README-English-blue)](README.md)
[![Русский](https://img.shields.io/badge/README-Русский-red)](README.ru.md)
[![Oʻzbekcha](https://img.shields.io/badge/README-Oʻzbekcha-green)](README.uz.md)

</div>

# VLAN muammolarini tuzatish qoʻllanmasi
[![CI](https://github.com/uMax-Cyber/VLANscope/actions/workflows/ci.yml/badge.svg)](https://github.com/uMax-Cyber/VLANscope/actions/workflows/ci.yml)


![Namoyish](screenshots/demo.svg)
Turli vendor uskunalari ishlatiladigan muhitlarda (UniFi kommutatorlar, Sophos fayrvol, Linux bridge) VLAN muammolarini aniqlash va tuzatish boʻyicha amaliy qoʻllanma. Qoʻllanmada native/tagged nomuvofiqlik, trunk tekshirish va VLAN-lar orasida DHCP oʻtmasligi kabi tipik holatlar koʻrib chiqiladi.

## ✨ Imkoniyatlar

- **Native/tagged nomuvofiqlikni aniqlash** — native VLAN shu portdagi untagged trafikka mos kelmaydigan portlarni topish
- **Trunk tekshiruvi** — trunk orqali qaysi VLAN-lar haqiqatdan oʻtayotganini aniq bilish
- **VLAN-lararo DHCP debugging** — UDP 67/68 boʻyicha relay va fayrvol qoidalarini tekshirish
- **Koʻp vendorli qamrov** — UniFi, Sophos va Linux bridge bitta qoʻllanmada
- **Tayyor buyruqlar** — koʻchirib darrov ishlatish mumkin boʻlgan `tcpdump` / `ip` retseptlari

## VLAN-dagi tipik muammolar (yechimlari bilan)

### 1. Native VLAN nomuvofiqligi
**Belgisi**: qurilmaga boshqa subnetdan IP tushib qoladi, ulanish vaqti-vaqti bilan uziladi.
**Sababi**: kommutator portidagi native VLAN qurilma kutganiga mos kelmaydi.
**Yechimi**: barcha portlarni tekshirib chiqing — native VLAN shu portdan chiqayotgan untagged trafikka mos boʻlishi kerak.

### 2. DHCP VLAN-lar orasidan oʻtmaydi
**Belgisi**: bir VLAN-dagi qurilmalar boshqa VLAN-dagi DHCP serverdan manzila ololmaydi.
**Sababi**: DHCP relay sozlanmagan yoki fayrvol VLAN-lar orasida UDP 67/68 portini bloklayapti.
**Yechimi**: shlyuzdagi relay sozlamasini va DHCP portlariga oid fayrvol qoidalarini tekshiring.

### 3. Trunk VLAN teglarini yetkazmaydi
**Belgisi**: bir fizik kanalda ayrim VLAN-lar ishlaydi, ayrimlari yoʻq.
**Sababi**: trunk portga barcha kerakli VLAN-larga ruxsat berilmagan (tagged_vlan_mgmt = block_all).
**Yechimi**: trunk portlarda kerakli VLAN-larni aniq koʻrsatib ruxsat bering.

### 4. Statik DHCP reserv notoʻgʻri scope-da
**Belgisi**: bitta qurilma «connecting...» da qolib ketadi, qolganlari odatdagidek ishlaydi.
**Sababi**: qurilmaga VLAN-A scope-sida statik DHCP reserv yozilgan, lekin u VLAN-B ga ulangan.
**Yechimi**: reservni toʻgʻri scope-ga koʻchiring yoki uni global qiling.

## Diagnostika vositalari

```bash
# Trunkda qaysi VLAN-lar borligini koʻrish
tcpdump -lni <iface> vlan -c 20

# DHCP relay yoʻlini tekshirish
# Shlyuzda: har bir VLAN interfeysi uchun DHCP relay sozlamasini tekshiring

# Muayyan VLAN-dan test (vaqtinchalik subinterfeys ochish)
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
